# TID Issuer Ansible

Infrastructure and application deployment automation for the TID Issuer platform.

## What It Deploys

- Infrastructure services on `infra_hosts` (PostgreSQL, Keycloak, MinIO)
- Quarkus API on `api_hosts`
- Vue frontend and Nginx on `web_hosts`
- Full stack on `docker_env_hosts` via Docker Compose (infra + API + web)

## Repository Layout

- `hosts.yaml`: inventory
- `group_vars/`: host-group configuration and secret placeholders
- `playbooks/`: `infra.yml`, `api.yml`, `web.yml`, `docker-env.yml`, `site.yml`
- `files/`: templates for env files and service configs

## Secrets

This repo supports encrypted secrets with Ansible Vault.

```bash
ansible-vault edit group_vars/secrets.vault.yml
```

Deploy with the vault file:

```bash
ansible-playbook -i hosts.yaml playbooks/site.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
```

## Common Runs

```bash
ansible-playbook -i hosts.yaml playbooks/infra.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
ansible-playbook -i hosts.yaml playbooks/api.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
ansible-playbook -i hosts.yaml playbooks/web.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
ansible-playbook -i hosts.yaml playbooks/docker-env.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
```

## Docker-Env Scenario

- Target host group: `docker_env_hosts` (host: `docker-env`)
- Deploys infra services, API, and frontend from container images in a single Compose stack
- Default image refs:
  - `ghcr.io/vasilpap/tid-issuer-quarkus:latest`
  - `ghcr.io/vasilpap/tid-issuer-vue:latest`
- Web endpoint is exposed on HTTPS (`443`) in this scenario
- GHCR login is optional; configure `vault_tid_docker_env_ghcr_username` and `vault_tid_docker_env_ghcr_token` only for private images

Quick run:

```bash
ansible-playbook -i hosts.yaml playbooks/docker-env.yml --ask-vault-pass -e @group_vars/secrets.vault.yml
```

Note: `ansible.cfg` already sets `inventory = ./hosts.yaml`, so `-i hosts.yaml` is optional when running from this repository.

## Notes

- Repositories are synced from `main`.
- Inventory hostnames are aligned with the Vagrant environment by default.

## Architecture and Diagrams

Cross-repository diagrams (API DTO class diagram, system architecture, deployment diagram) and the complete technology inventory are documented in `../WORKSPACE_ARCHITECTURE.md`.
