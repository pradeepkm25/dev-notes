# Keycloak 26: System Client Access Token Lifespan (24 Hours)

**Date:** 2026-10-13  
**Module:** Keycloak / IAM / Security  
**Category:** Client Configuration & Lifespan Management  
**Target Client:** `akashic-system`  
**Keycloak Version:** 26.x  
**Status:** Verified in UI | Pending Code Update in Provisioning API  

---

## 🎯 1. Purpose & Requirement

* **Objective:** Configure the `akashic-system` service account client in each tenant realm with a **24-hour access token validity** (`86,400` seconds).
* **Why 24 Hours?** Background worker tasks, asynchronous batch ingestions, and internal microservice-to-microservice (M2M) communications use `client_credentials` grants. A 24-hour lifespan prevents frequent token renegotiation overhead and avoids refresh token complexity for non-interactive backend jobs.
* **Strict Scope:** This configuration applies **only** to the `akashic-system` client. Realm-wide token defaults (e.g., 900s / 15m) and interactive user clients (`akashic-{tenantName}`) remain unchanged.

---

## 🛠️ 2. Keycloak Admin Console Steps (UI)

1. Open Keycloak Admin Console (v26.x).
2. Select target tenant realm (e.g., `akashic-tenantd-realm`).
3. In left sidebar: **Clients** → Click **`akashic-system`**.
4. Open the **Settings** tab.
5. Scroll down to **Advanced** (or **Advanced settings** section; exact label varies slightly across Keycloak 26 minor releases).
6. Locate **Access Token Lifespan**.
7. Change the setting from realm default to **24 hours** (or `1` day / `86400` seconds).
8. Click **Save**.

---

## ⚙️ 3. Technical Configuration (Code Blueprint)

* **Keycloak Attribute Key:** `access.token.lifespan`
* **Attribute Value:** `"86400"` (string in seconds)

### Target Location in Provisioning Service
* **Repository:** `akashic-tenant-provisioning-api`
* **File:** [`src/modules/platform/services/platform-client.service.ts`](file:///D:/Dhira-Work/akashic-tenant-provisioning-api/src/modules/platform/services/platform-client.service.ts)
* **Method:** `createSystemClient(params)`

### Incorporating for Newly Provisioned Clients
Preserve existing attributes (`LOGOUT_ATTRIBUTES`) and append `access.token.lifespan`:

```typescript
const payload = buildServiceAccountClient({
  clientId,
  name: 'Akashic System',
  authorizationServicesEnabled: false,
  standardFlowEnabled: true,
  directAccessGrantsEnabled: true,
  redirectUris: ['/*'],
  webOrigins: this.keycloakCfg.webOrigins,
  attributes: {
    ...LOGOUT_ATTRIBUTES,
    'access.token.lifespan': '86400', // 24 hours in seconds
  },
});
```

### Updating Existing Tenant Realms
For tenant realms that are already provisioned, update via Admin Console or run an update script:

```typescript
const [client] = await kcAdminClient.clients.find({ clientId: 'akashic-system' });
if (client?.id) {
  await kcAdminClient.clients.update(
    { id: client.id },
    {
      attributes: {
        ...client.attributes,
        'access.token.lifespan': '86400',
      },
    },
  );
}
```

---

## 🧪 4. Verification

### Step 1: Request Token via `client_credentials`
```bash
curl -X POST "https://auth.example.com/realms/akashic-YOUR_TENANT-realm/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "client_id=akashic-system" \
  -d "client_secret=YOUR_SYSTEM_CLIENT_SECRET"
```

### Step 2: Inspect Response
```json
{
  "access_token": "eyJh...",
  "expires_in": 86400,
  "token_type": "Bearer",
  "not-before-policy": 0,
  "scope": "email profile"
}
```
* **Expected Result:** `expires_in: 86400` (86,400 seconds = 24 hours).

### Troubleshooting Shorter Expiry
If `expires_in` returns less than `86400` (e.g., `900`):
1. **Session Lifespan Constraint:** Keycloak bounds the effective token lifespan to the client session duration. Verify in **Realm settings** → **Sessions** that `Client Session Idle` and `Client Session Max` are $\ge 24$ hours (`86400`s).
2. **Attribute Verification:** Check `GET /admin/realms/{realm}/clients/{id}` to ensure `'access.token.lifespan'` is saved under `attributes`.
3. **Client Check:** Confirm token was requested with `client_id=akashic-system` and not another client.

---

## 🔒 5. Scope & Security Considerations

* **Isolated Lifetime:** Keep interactive web sessions short-lived (15 minutes). Only `akashic-system` receives the 24-hour lifespan.
* **Session Limits Interaction:** Client session timeouts cap access token expiration.
* **Token Exposure Risk:** A 24-hour bearer token has a wider window of misuse if compromised. Keep system client secrets strictly in vault/environment secrets and restrict token distribution to internal backend microservices.
* **No Secrets Committed:** Always sanitize documentation with placeholders (`YOUR_SYSTEM_CLIENT_SECRET`, `YOUR_TENANT`).

---

## 📌 6. Implementation Status

| Stage | Status | Notes |
| :--- | :---: | :--- |
| **Keycloak 26 Admin UI** | ✅ Confirmed | Tested and verified via client settings |
| **Tenant Provisioning Code** | ⏳ Pending | Blueprint documented (`ENH-007`); pending codebase merge |

---

## 🏷️ Tags & Keywords
#keycloak-26 #system-client #akashic-system #access-token-lifespan #86400 #24-hour-token #tenant-provisioning #security
