# Remove `client host` and `client address` Claims from Keycloak Token

**Module:** Keycloak / Token Sanitization  
**Category:** Security & Performance  

---

## 🎯 Goal
Remove default `clientHost` and `clientAddress` claims from JWT access/ID tokens to eliminate token bloat and prevent leaking internal hostnames or IP addresses.

---

## 🛠️ Method 1: Keycloak Admin Console (UI)

1. Go to **Clients** → Select Client (e.g. `akashic-system` or frontend client).
2. Open **Client scopes** tab → Click **`<client-id>-dedicated`** scope (or **Mappers** tab).
3. Find:
   * `client host` (Protocol mapper: `oidc-client-host-mapper`)
   * `client address` (Protocol mapper: `oidc-client-ip-address-mapper` / `oidc-client-address-mapper`)
4. **Action**: Select both and click **Delete** (or edit each and toggle **Add to access token** → `OFF`).

---

## 💻 Method 2: Keycloak Admin REST API / Code

```typescript
// 1. List client's dedicated mappers
const mappers = await kcAdminClient.clients.listProtocolMappers({ id: keycloakClientUUID });

// 2. Filter & delete client host and client address mappers
const mappersToDelete = mappers.filter(
  (m) => m.name === 'client host' || m.name === 'client address'
);

for (const mapper of mappersToDelete) {
  if (mapper.id) {
    await kcAdminClient.clients.delProtocolMapper({
      id: keycloakClientUUID,
      mapperId: mapper.id,
    });
  }
}
```

---

## ✅ Verification
1. Request a new token (`client_credentials` or `password` grant).
2. Decode JWT payload and confirm `clientHost` and `clientAddress` keys are removed.

---

## 🏷️ Tags
#keycloak #jwt #token-cleanup #client-mapper #security
