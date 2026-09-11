# Keycloak Realm Brute Force Detection Configuration

**Module:** Keycloak / Security Defenses  
**Category:** Security & Tenant Provisioning  
**Status:** Task Logged (Pending implementation in `akashic-tenant-provisioning-api`)  

---

## 🎯 Goal
Enable automated Brute Force Detection across all provisioned Keycloak tenant realms to temporarily lock out users upon repeated authentication failures, mitigating password-guessing and credential-stuffing attacks.

---

## ⚙️ Target Configuration (Keycloak Admin UI)

**Navigation**: **Realm settings** → **Security defenses** tab → **Brute force detection**

| Keycloak UI Field | Value | RealmRepresentation Property | Purpose / Description |
| :--- | :--- | :--- | :--- |
| **Enabled** | `ON` | `bruteForceProtected: true` | Master toggle to enable brute force protection |
| **Brute Force Mode** | `Lockout temporarily` | `permanentLockout: false` | Locks out user temporarily rather than requiring manual admin unlock |
| **Max login failures** | `10` | `failureFactor: 10` | Number of failed attempts before temporary lockout kicks in |
| **Strategy to increase wait time** | `Multiple` | *(Standard exponential backoff)* | Increment wait time per additional failed attempt |
| **Wait increment** | `1 Minute` (60s) | `waitIncrementSeconds: 60` | Initial wait duration added per failure beyond threshold |
| **Max wait** | `15 Minutes` (900s) | `maxFailureWaitSeconds: 900` | Upper ceiling on temporary lockout wait time |
| **Failure reset time** | `12 Hours` (43200s) | `maxDeltaTimeSeconds: 43200` | Window after which failed attempt counter resets to 0 |
| **Quick login check** | `1000 ms` | `quickLoginCheckMilliSeconds: 1000` | Time limit to detect rapid-fire scripted login attempts |
| **Minimum quick login wait** | `1 Minute` (60s) | `minimumQuickLoginWaitSeconds: 60` | Minimum wait enforced if quick login check is triggered |

---

## 💻 Future Implementation Blueprint (`akashic-tenant-provisioning-api`)

### 1. Define Constant in `src/common/constants/keycloak/keycloak.constants.ts`
```typescript
import type RealmRepresentation from '@keycloak/keycloak-admin-client/lib/defs/realmRepresentation';

export const REALM_BRUTE_FORCE_CONFIG: Partial<RealmRepresentation> = {
  bruteForceProtected: true,
  permanentLockout: false,
  failureFactor: 10,
  waitIncrementSeconds: 60,
  maxFailureWaitSeconds: 900,
  maxDeltaTimeSeconds: 43200,
  quickLoginCheckMilliSeconds: 1000,
  minimumQuickLoginWaitSeconds: 60,
};
```

### 2. Apply in `PlatformRealmService.createRealm` (`src/modules/platform/services/platform-realm.service.ts`)
```typescript
// Include during realm creation:
await kcAdminClient.realms.create({
  realm: realmName,
  enabled: true,
  displayName: prettifyRealmName(realmName),
  ...REALM_SESSION_CONFIG,
  ...REALM_BRUTE_FORCE_CONFIG, // <-- Add brute force config here
  passwordPolicy: REALM_PASSWORD_POLICY,
  // ...
});
```

---

## 🏷️ Tags
#keycloak #security #brute-force #realm-settings #tenant-provisioning #auth
