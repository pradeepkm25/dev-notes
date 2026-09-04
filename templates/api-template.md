# API Name

## Purpose
[Describe what this endpoint does]

## Module
[e.g., Tenant Management / Data Catalog / Notifications]

## Method
GET / POST / PUT / PATCH / DELETE

## Endpoint
`/api/example`

## Authentication
Bearer Token (JWT) - Describe authentication without exposing secrets.

## Headers
```text
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

## Request Body
```json
{}
```

## Curl
```bash
curl --request GET \
  --url "https://api.example.com/api/example" \
  --header "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## Response Example
```json
{}
```

## Important Notes
- [Special rate limits, caching behaviors, or validation rules]

## Common Errors
- `401 Unauthorized`: Token expired or invalid signature.
- `403 Forbidden`: Missing required permission.

## Related Documentation
- [Link to other docs or flows]

## Tags
#api #module-name #rest
