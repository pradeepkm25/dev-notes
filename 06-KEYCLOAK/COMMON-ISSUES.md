# Keycloak Common Issues & Troubleshooting

Frequently encountered Keycloak configuration errors, token decoding issues, CORS pitfalls, and their solutions.

---

## 🛑 Issue 1: "Invalid parameter: redirect_uri"

### Symptoms
User attempts to log in but Keycloak shows: `Invalid parameter: redirect_uri`.

### Root Cause
The `redirect_uri` sent by the frontend does not strictly match the **Valid Redirect URIs** configured in the Keycloak Client.

### Solution
1. In Keycloak Admin Console -> **Clients** -> Select your Client.
2. Under **Valid Redirect URIs**, ensure exact URL (e.g., `https://acme-corp.example.com/*` or `http://localhost:3000/*` for dev).
3. Under **Web Origins**, add `+` or the origin `https://acme-corp.example.com` to prevent CORS issues.

---

## 🛑 Issue 2: Missing `tenant_id` Claim in JWT Token

### Symptoms
Backend returns `401 Unauthorized` or `400 Bad Request: Missing tenant context` even though the user successfully logged in.

### Root Cause
The user does not have the `tenant_id` attribute set on their Keycloak user profile, or the Client Scope protocol mapper is missing.

### Solution
1. Verify User Profile: Go to **Users** -> Select User -> **Attributes** -> Check key `tenant_id` is present and populated.
2. Verify Client Scope Mapper: Check that the client scope assigned to this client has the protocol mapper for `tenant_id` enabled for `Access Token`.

---

## 🛑 Issue 3: 403 Forbidden with Valid Token (Audience Mismatch)

### Symptoms
API Gateway rejects the JWT token with `403 Forbidden: Invalid Audience (aud)`.

### Root Cause
Keycloak defaults to setting `aud: "account"`. If your API Gateway or Spring Resource Server verifies `@Value("${spring.security.oauth2.resourceserver.jwt.issuer-uri}")` and requires audience check, it will reject.

### Solution
Add an **Audience Protocol Mapper** in Keycloak:
- Mapper Type: `Audience`
- Included Client Audience: Target client ID (e.g., `backend-api`).

---

## 🏷️ Tags
#keycloak #troubleshooting #jwt #cors #redirect-uri #audience
