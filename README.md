# TID Issuer Ansible

This repo deploys the TID Issuer stack with monolithic inline playbooks.

It follows the same practical structure as your `../dev-ops-course/ansible-devops` work (inventory, `group_vars`, playbooks), adapted for this workspace where each app lives in its own repository.

## Structure

- `hosts.yaml`: inventory (control, `infra_hosts`, `api_hosts`, `web_hosts`)
- `group_vars/`: environment and deployment variables per host group
- `playbooks/`: entry points (`infra.yml`, `api.yml`, `web.yml`, `site.yml`)
- `files/`: Jinja2 templates for configuration files rendered on targets

## Run

```bash
ansible-playbook playbooks/site.yml
```

Or per component:

```bash
ansible-playbook playbooks/infra.yml
ansible-playbook playbooks/api.yml
ansible-playbook playbooks/web.yml
```

## Notes

- Repositories are deployed from `main`.
- Default hostnames are set in `group_vars/all.yml` under `tid_hosts`.
- Production secrets are expected in `group_vars/secrets.yml` (ignored by git).
- Bootstrap flow:
  - `cp group_vars/secrets.yml.example group_vars/secrets.yml`
  - `ansible-vault encrypt group_vars/secrets.yml`
  - Fill encrypted values for DB, Keycloak admin, OIDC client secret, and MinIO credentials.
