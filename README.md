# TID Issuer Ansible

Infrastructure and application deployment automation for the TID Issuer platform.

## What It Deploys

- Infrastructure services on `infra_hosts` (PostgreSQL, Keycloak, MinIO)
- Quarkus API on `api_hosts`
- Vue frontend and Nginx on `web_hosts`

## Repository Layout

- `hosts.yaml`: inventory
- `group_vars/`: host-group configuration and secret placeholders
- `playbooks/`: `infra.yml`, `api.yml`, `web.yml`, `site.yml`
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
```

## Notes

- Repositories are synced from `main`.
- Inventory hostnames are aligned with the Vagrant environment by default.
