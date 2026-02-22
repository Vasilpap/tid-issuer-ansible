# Public Release Security Cleanup Report

Date: 2026-02-22

This report summarizes the pre-public hardening across the TID Issuer repos.

## Completed

- Moved runtime secret handling toward Ansible vault-driven variables.
- Rewrote git history to remove previously exposed sensitive artifacts.
- Removed tracked Vue `.env` history.
- Tightened `.gitignore` patterns for logs/backups/local machine state.
- Replaced default secret values with placeholders and required env guards.

## Key Outcomes

- `tid-issuer-quarkus`
  - Removed `quarkus.log*` from history.
  - Removed leaked OIDC secret string from history.
  - Hardened MinIO defaults and log ignore rules.
- `tid-issuer-vue`
  - Removed `.env` from history and tracking.
- `tid-issuer-infra`
  - Removed Keycloak key-provider secrets/private keys from `realm-export.json` history.
  - Kept two test users as requested.
  - Enforced required secret env vars in `docker-compose.yml`.

## Published Cleanup Branches

- `https://github.com/Vasilpap/tid-issuer-quarkus/tree/public-release-security-cleanup-2026-02-22`
- `https://github.com/Vasilpap/tid-issuer-vue/tree/public-release-security-cleanup-2026-02-22`
- `https://github.com/Vasilpap/tid-issuer-infra/tree/public-release-security-cleanup-2026-02-22`

## Main Branch Update

After approval, cleaned history and hardening commits were force-updated to `main`:

- `tid-issuer-quarkus` main -> `984d102`
- `tid-issuer-vue` main -> `6bb0dbf`
- `tid-issuer-infra` main -> `9398bfe`

## Newly Published Repositories

- `https://github.com/Vasilpap/tid-issuer-ansible` (`main`: `3f71044`)
- `https://github.com/Vasilpap/tid-issuer-vagrant` (`main`: `904e59e`)

## Required Manual Follow-up

1. Rotate live credentials (DB, Keycloak admin, MinIO, any client secrets).
2. Create and encrypt `group_vars/secrets.yml` from `group_vars/secrets.yml.example`.
