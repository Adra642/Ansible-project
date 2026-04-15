# K3s Academic Lab with Ansible

This project provisions a small K3s cluster using Ansible, organized with a maintainable structure commonly used in real projects.

## Project layout

```text
.
├── ansible.cfg
├── inventories/
│   └── lab/
│       ├── group_vars/
│       │   └── all/
│       │       └── config.yml
│       └── hosts.ini
├── playbooks/
│   └── site.yml
└── roles/
    ├── base/
    │   └── tasks/main.yml
    ├── k3s_agent/
    │   └── tasks/main.yml
    ├── k3s_server/
    │   └── tasks/main.yml
    └── jellyfin/
        ├── tasks/main.yml
        └── templates/jellyfin.yaml.j2

```

## Requirements

- Ansible installed on the control machine
- SSH access to all nodes with sudo privileges
- Network connectivity between control plane and agents

## Quick start

1. Edit inventory values in `inventories/lab/hosts.ini`.
2. Optionally set `ansible_user` per host and tune variables in `inventories/lab/group_vars/all/config.yml`.
3. Run:

```bash
ansible-playbook playbooks/site.yml
```

The final play deploys Jellyfin into the `media` namespace with a NodePort service on `30096` and persistent volumes for both `/config` and `/media`.

## Academic best-practice notes

- Roles split responsibilities (`base`, `k3s_server`, `k3s_agent`, `jellyfin`)
- Environment variables and settings are centralized in `group_vars`
- Idempotency is preserved with `creates` checks
- Token handling remains in memory and is not persisted to files
- `ansible.cfg` keeps project defaults local to this repository
