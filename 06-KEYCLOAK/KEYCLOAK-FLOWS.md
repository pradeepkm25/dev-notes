# Keycloak Authentication & IAM Flows

Integration patterns, OIDC token mapping, client configuration, and service account setups in Keycloak.

---

## 🔑 Multi-Tenant Token Enrichment Pattern

To allow the backend API to identify the tenant and user roles without extra database lookups on every request, configure Keycloak Protocol Mappers:

### 1. User Attribute Mapper: `tenant_id`
- **Mapper Type**: `User Attribute`
- **Name**: `tenant_id_mapper`
- **User Attribute**: `tenant_id`
- **Token Claim Name**: `tenant_id`
- **Claim JSON Type**: `String`
- **Add to ID Token**: `true`
- **Add to Access Token**: `true`

### 2. Client Roles Mapper
- Ensure `resource_access.<client-id>.roles` includes application-level roles (`TENANT_ADMIN`, `DATA_STEWARD`, `MEMBER`).

---

## 🔄 Service Account (M2M) Setup Flow

When backend microservices communicate asynchronously or perform background tenant migrations:

1. In Keycloak Admin Console, go to **Clients** -> Select Client (e.g., `tenant-provisioning-service`).
2. Set **Client Authentication**: `ON`.
3. Enable **Service Accounts Roles** in Capability config.
4. Under **Service Account Roles** tab: Assign `realm-admin` or specific fine-grained roles.
5. Retrieve credentials via POST to token endpoint using `grant_type=client_credentials`.

---

## 🏷️ Tags
#keycloak #oidc #jwt #mappers #service-accounts #auth
