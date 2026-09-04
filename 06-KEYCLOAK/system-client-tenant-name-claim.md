# Keycloak System Client: `tenant_name` Hardcoded Claim Configuration

**Date:** 2026-09-04  
**Module:** Keycloak / Tenant Provisioning  
**Category:** IAM / Client Configuration  

---

## 🔍 Investigation & Current Behavior

Based on thorough inspection of the [`akashic-tenant-provisioning-api`](file:///D:/Dhira-Work/akashic-tenant-provisioning-api) repository:

### Current Implementation State: **Scenario A**
* **System Client Creation**: The `system-client` (client ID: `akashic-system`) is created during tenant provisioning inside `PlatformClientService.createSystemClient(...)` in `src/modules/platform/services/platform-client.service.ts`.
* **Protocol Mapper State**: Currently, **no protocol mapper is created for `akashic-system` at all**.
* **Other Clients in Codebase**: `createPublicClient`, `createAdminServiceClient`, and `createBiClient` currently attach `TENANT_NAME_MAPPER` (`oidc-usermodel-attribute-mapper`), which maps from user profile attributes. However, `akashic-system` is a service account confidential client (client credentials grant) and does not map `tenant_name`.

---

## 🎯 Requirement

Backend microservices and internal consumers validating M2M service tokens issued to `akashic-system` require the `tenant_name` claim directly inside the JWT token payload to accurately identify the tenant namespace and route requests without additional database lookups.

---

## ⚙️ Required Configuration

To include `tenant_name` in tokens generated via `client_credentials`, a **Hardcoded Claim** mapper must be added to the `akashic-system` client in Keycloak:

| Configuration Property | Value | Description |
| :--- | :--- | :--- |
| **Mapper Name** | `tenant_name` | Name of the protocol mapper in Keycloak |
| **Protocol** | `openid-connect` | OpenID Connect protocol |
| **Mapper Type** | `oidc-hardcoded-claim-mapper` | Keycloak Hardcoded Claim mapper type |
| **Claim Name** | `tenant_name` | Token claim key injected into the JWT |
| **Claim Value** | `<CURRENT_TENANT_NAME>` | Dynamically evaluated value from the provisioning tenant |
| **JSON Type** | `String` | JSON data type for the claim value |
| **Add to ID Token** | `true` | Include in ID token |
| **Add to Access Token** | `true` | Include in Access token (critical for API calls) |
| **Add to UserInfo** | `true` | Include in UserInfo endpoint response |
| **Add to Token Response** | `false` | Exclude from raw token response wrapper |

---

## 📍 Implementation Location

* **Repository**: `akashic-tenant-provisioning-api`
* **Module / Service**: `PlatformClientService` (`src/modules/platform/services/platform-client.service.ts`)
* **Method**: `createSystemClient(params)`
* **Orchestrator Step**: `PlatformProvisionService.provision(...)` (Step 11) in `src/modules/platform/services/platform-provision.service.ts`

### Code Addition Blueprint

Inside `PlatformClientService.createSystemClient`:

```typescript
// After creating client record:
const keycloakClientId = await this.createClientRecord(kcAdminClient, payload);

// Add dynamic Hardcoded Claim mapper for tenant_name:
await kcAdminClient.clients.addProtocolMapper(
  { id: keycloakClientId },
  {
    protocol: 'openid-connect',
    protocolMapper: 'oidc-hardcoded-claim-mapper',
    name: 'tenant_name',
    config: {
      'claim.name': 'tenant_name',
      'claim.value': tenantName, // Dynamically sourced from provisioning parameter
      'jsonType.label': 'String',
      'id.token.claim': 'true',
      'access.token.claim': 'true',
      'userinfo.token.claim': 'true',
      'access.tokenResponse.claim': 'false',
    },
  },
);

const clientSecret = await this.fetchClientSecret(kcAdminClient, keycloakClientId);
```

---

## 🔄 Provisioning Flow

```text
Start Tenant Provisioning (POST /api/v1/platform/provision)
        ↓
Get Current Provisioning Context (tenantName, tenantId, realmName)
        ↓
Create Keycloak Realm & User Profile Schema
        ↓
Create / Locate system-client ('akashic-system')
        ↓
Add Hardcoded Claim Protocol Mapper ('oidc-hardcoded-claim-mapper')
        ↓
Set Claim Name = 'tenant_name'
Set Claim Value = Current Tenant Name (e.g., 'tenantd')
        ↓
Assign Realm Management Admin Roles to Service Account
        ↓
Persist Client ID & Secret to Database
        ↓
Generate Token via Client Credentials Grant
        ↓
JWT Payload contains "tenant_name": "<CURRENT_TENANT_NAME>"
```

---

## 🧪 Validation & Verification

1. **Trigger Provisioning**: Provision a new tenant (e.g. `tenantd`).
2. **Fetch Service Account Token**:
   ```bash
   curl -X POST "https://auth.example.com/realms/akashic-tenantd-realm/protocol/openid-connect/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "grant_type=client_credentials" \
     -d "client_id=akashic-system" \
     -d "client_secret=YOUR_SYSTEM_CLIENT_SECRET"
   ```
3. **Inspect JWT Token Payload**:
   Decode the resulting `access_token` at jwt.io or using `jwt-cli` and verify the claim:
   ```json
   {
     "iss": "https://auth.example.com/realms/akashic-tenantd-realm",
     "aud": "account",
     "clientId": "akashic-system",
     "tenant_name": "tenantd"
   }
   ```

---

## ⚠️ Important Notes

* **Dynamic Value**: The `claim.value` must always be derived dynamically from the `tenantName` parameter passed into `createSystemClient`. Never hardcode static names like `tenantd` in source code or constants.
* **Mapper Type Distinction**: Do not use `oidc-usermodel-attribute-mapper` for `system-client` because client credentials tokens originate from service accounts, whereas `oidc-hardcoded-claim-mapper` guarantees the claim is always stamped on every token issued to `akashic-system`.
* **Zero Disruption to Existing Mappers**: This enhancement only attaches the mapper to `akashic-system` and does not alter the configuration of other clients (`akashic-<tenant>`, `akashic-<tenant>-admin-service`, `akashic-bi`).

---

## 🏷️ Tags

#keycloak #system-client #tenant-name #protocol-mapper #hardcoded-claim #tenant-provisioning #jwt #m2m #reusable
