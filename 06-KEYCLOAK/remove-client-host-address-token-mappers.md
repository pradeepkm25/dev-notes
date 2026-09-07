# Remove `client host` and `client address` Claims from Keycloak Token

**Module:** Keycloak / Token Sanitization  
**Category:** Security & Performance  

---

## 🎯 Goal
Remove `clientHost` and `clientAddress` (or `Client IP Address`) claims from JWT tokens generated for service account clients (e.g. `akashic-system`) to eliminate token bloat and prevent leaking internal hostnames/IPs.

---

## 🔍 Key Discovery (Parent Client Scope)

When inspecting a client in Keycloak:
* Path: **Clients** → Select Client (e.g. `akashic-system`) → **Client scopes** → **Evaluate** tab.
* You will see:
  | Name | Parent client scope | Category |
  | :--- | :--- | :--- |
  | `Client Host` | `service_account` | Token mapper |
  | `Client IP Address` | `service_account` | Token mapper |

> [!NOTE]
> `Client Host` and `Client IP Address` are **inherited from the `service_account` Client Scope**, not from client-dedicated mappers.

---

## 🛠️ Method 1: Keycloak Admin Console (UI)

### Option A: Remove from `service_account` Client Scope (Realm-Wide)
1. Go to **Client scopes** (left sidebar navigation).
2. Click **`service_account`** client scope.
3. Go to the **Mappers** tab.
4. Select **`Client Host`** and **`Client IP Address`** → Click **Delete** (or edit each and toggle **Add to access token** → `OFF`).

### Option B: Remove `service_account` Scope from Specific Client
1. Go to **Clients** → Select `akashic-system`.
2. Go to **Client scopes** tab → **Setup** tab.
3. If `service_account` is assigned as a default scope and only custom claims (like `tenant_name`) are needed, remove or change it to optional.

---

## 💻 Method 2: Keycloak Admin REST API / Code

```typescript
// 1. Fetch the 'service_account' client scope
const clientScopes = await kcAdminClient.clientScopes.find();
const saScope = clientScopes.find((s) => s.name === 'service_account');

if (saScope?.id) {
  // 2. List mappers under 'service_account' scope
  const mappers = await kcAdminClient.clientScopes.listProtocolMappers({
    id: saScope.id,
  });

  // 3. Delete 'Client Host' and 'Client IP Address' mappers
  const targetMappers = mappers.filter(
    (m) => m.name === 'Client Host' || m.name === 'Client IP Address' || m.name === 'client host' || m.name === 'client address'
  );

  for (const mapper of targetMappers) {
    if (mapper.id) {
      await kcAdminClient.clientScopes.delProtocolMapper({
        id: saScope.id,
        mapperId: mapper.id,
      });
    }
  }
}
```

---

## ✅ Verification

1. **In UI**: Go to **Clients** → `akashic-system` → **Client scopes** → **Evaluate** tab → Click **Generated access token** to confirm claims are gone.
2. **Via Curl**: Generate a `client_credentials` token and decode at jwt.io to verify payload contains only desired claims (`tenant_name`, `clientId`, etc.).

---

## 🏷️ Tags
#keycloak #jwt #token-cleanup #client-scopes #service-account #security
