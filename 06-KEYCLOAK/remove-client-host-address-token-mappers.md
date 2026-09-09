# Remove `client host` and `client address` Claims from Keycloak Token

**Module:** Keycloak / Token Sanitization  
**Category:** Security & Performance  
**Implementation Approach:** Option A (Realm-Level Client Scope Sanitization)  

---

## 🎯 Goal
Remove default `clientHost` and `clientAddress` (Client IP Address) token claims from service account JWT tokens (such as `akashic-system`) during tenant provisioning to prevent internal IP/hostname leakage and avoid token bloat.

---

## 🔍 Root Cause Analysis (Parent Client Scope)

When inspecting service account clients in Keycloak:
* **Navigation**: **Clients** → Select Client (e.g. `akashic-system`) → **Client scopes** → **Evaluate** tab.
* **Findings**:
  | Name | Parent client scope | Category |
  | :--- | :--- | :--- |
  | `Client ID` | `service_account` | Token mapper |
  | `Client Host` | `service_account` | Token mapper |
  | `Client IP Address` | `service_account` | Token mapper |

> [!NOTE]
> `Client Host` and `Client IP Address` are **inherited from the `service_account` Client Scope**, not client-dedicated mappers.

---

## 🏛️ Selected Architecture: Option A (Realm-Level Sanitization)

Rather than detaching the `service_account` scope per client, Option A purges the `Client Host` and `Client IP Address` protocol mappers directly from the realm's `service_account` client scope during tenant provisioning.

### Advantages:
1. Standardizes clean, secure service account tokens across all microservices (`akashic-system`, `admin-service`, etc.).
2. Retains the standard `clientId` mapping from `service_account` scope.
3. Zero repetitive per-client scope detachment logic.

---

## 💻 Production Implementation (`PlatformRealmService`)

* **Repository**: `akashic-tenant-provisioning-api`
* **File**: `src/modules/platform/services/platform-realm.service.ts`
* **Method**: `sanitizeServiceAccountClientScope(kcAdminClient, realmName, correlationId)`

```typescript
private async sanitizeServiceAccountClientScope(
  kcAdminClient: KcAdminClient,
  realmName: string,
  correlationId: string,
): Promise<void> {
  try {
    const clientScopes = await kcAdminClient.clientScopes.find();
    const saScope = clientScopes.find((s) => s.name === 'service_account');

    if (!saScope?.id) {
      return;
    }

    const mappers = await kcAdminClient.clientScopes.listProtocolMappers({
      id: saScope.id,
    });

    const targetNames = new Set(['client host', 'client ip address', 'client address']);
    const targetMappers = mappers.filter((m) => {
      const name = m.name?.trim().toLowerCase();
      const sessionNote = m.config?.['user.session.note'];
      return (
        (name && targetNames.has(name)) ||
        sessionNote === 'clientHost' ||
        sessionNote === 'clientAddress'
      );
    });

    for (const mapper of targetMappers) {
      if (mapper.id) {
        await kcAdminClient.clientScopes.delProtocolMapper({
          id: saScope.id,
          mapperId: mapper.id,
        });
      }
    }

    if (targetMappers.length > 0) {
      this.logger.log(
        `Service account client scope sanitized — removed ${targetMappers.length} mapper(s) [${correlationId}]`,
        {
          context: PlatformRealmService.name,
          realm: realmName,
          removedMappers: targetMappers.map((m) => m.name),
        },
      );
    }
  } catch (error) {
    this.logger.warn(
      `Failed to sanitize service_account client scope [${correlationId}]: ${
        error instanceof Error ? error.message : String(error)
      }`,
      {
        context: PlatformRealmService.name,
        realm: realmName,
      },
    );
  }
}
```

### Execution Points in `createRealm`:
1. **New Realm**: Invoked right after configuring OTP, SMTP, and theme settings.
2. **Existing Realm**: Invoked when an existing realm is detected and reused.

---

## 🛠️ Manual UI Alternative (Keycloak Admin Console)

1. Go to **Client scopes** (left sidebar navigation).
2. Click **`service_account`** client scope.
3. Open the **Mappers** tab.
4. Select **`Client Host`** and **`Client IP Address`** → Click **Delete** (or edit each and toggle **Add to access token** → `OFF`).

---

## ✅ Verification

1. **In UI**: Go to **Clients** → `akashic-system` → **Client scopes** → **Evaluate** tab → Click **Generated access token** to confirm claims are absent.
2. **Via Token Request**:
   ```bash
   curl -X POST "https://auth.example.com/realms/akashic-tenant-realm/protocol/openid-connect/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "grant_type=client_credentials" \
     -d "client_id=akashic-system" \
     -d "client_secret=YOUR_CLIENT_SECRET"
   ```
   Decode JWT payload:
   - `clientHost`: ❌ Absent
   - `clientAddress`: ❌ Absent
   - `clientId`: ✅ Present
   - `tenant_name`: ✅ Present

---

## 🏷️ Tags
#keycloak #jwt #token-cleanup #client-scopes #service-account #security #tenant-provisioning
