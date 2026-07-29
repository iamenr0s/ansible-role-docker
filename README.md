[![Molecule](https://github.com/iamenr0s/ansible-role-docker/actions/workflows/molecule.yml/badge.svg)](https://github.com/iamenr0s/ansible-role-docker/actions/workflows/molecule.yml) ![Ansible Role](https://img.shields.io/ansible/role/d/iamenr0s/ansible_role_docker) [![CodeFactor](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-docker/badge)](https://www.codefactor.io/repository/github/iamenr0s/ansible-role-docker)

Ansible Role: Docker
=====================

This role installs Docker CE on Debian/Ubuntu and RHEL-family (Fedora/AlmaLinux/RockyLinux) hosts, amd64/arm64. It adds the official Docker package repository (apt or dnf, depending on the host), installs the Docker packages, enables the service, adds users to the `docker` group, and templates a custom `daemon.json`.

Features
--------
- Adds the official Docker apt or dnf repository, dispatched by `ansible_facts['os_family']`.
- Installs `docker-ce`, `docker-ce-cli`, and `containerd.io`.
- Enables and starts the `docker` service.
- Adds configured users to the `docker` group.
- Templates `/etc/docker/daemon.json` with a `json-file` log driver and capped log size.

Requirements
------------
- Python 3 available on the managed hosts (Ansible modules require Python).
- `apt` (Debian/Ubuntu) or `dnf` (Fedora/AlmaLinux/RockyLinux) present.
- Run with privilege escalation on real hosts: `become: true` is recommended.

Supported Platforms
--------------------
- AlmaLinux 8, 9, 10
- RockyLinux 8, 9, 10
- Fedora 42, 43, 44
- Debian 12 (bookworm), 13 (trixie)
- Ubuntu 22.04 (jammy), 24.04 (noble)

Note: `meta/main.yml` lists the same set in Galaxy's own format (numeric versions for EL/Fedora, codenames for Debian/Ubuntu).

Role Variables
--------------
Defined in `defaults/main.yml`:

- `docker_edition` (str): Docker edition to install, `ce` or `ee` (default: `ce`).
- `docker_packages` (list): Docker packages to install (default: `docker-{{ docker_edition }}`, `docker-{{ docker_edition }}-cli`, `containerd.io`).
- `docker_apt_prerequisite_packages` (list): Prerequisite apt packages installed before adding the Docker repository (Debian/Ubuntu).
- `docker_apt_gpg_key` (str): URL of the Docker apt GPG key (Debian/Ubuntu).
- `docker_apt_keyring_path` (str): Path where the Docker apt GPG key is stored (default: `/etc/apt/keyrings/docker.asc`).
- `docker_apt_release_channel` (str): Docker apt release channel (default: `stable`).
- `docker_apt_repository` (str): Docker apt repository line (Debian/Ubuntu).
- `docker_redhat_prerequisite_packages` (list): Prerequisite dnf packages installed before adding the Docker repository (Fedora/AlmaLinux/RockyLinux).
- `docker_redhat_repo_distro` (str): Docker repository path segment for RHEL-family hosts, `fedora` or `centos` (auto-detected).
- `docker_redhat_gpg_key` (str): URL of the Docker RPM GPG key (Fedora/AlmaLinux/RockyLinux).
- `docker_redhat_repo_url` (str): URL of the Docker dnf repository file (Fedora/AlmaLinux/RockyLinux).
- `docker_users` (list): Users to add to the `docker` group (default: `[]`).
- `docker_run_not_in_container` (bool): Enables the `overlay2` storage driver in `daemon.json`; only needed when the role runs outside a container (default: `false`).

Dependencies
------------
- Collections: none.
- Role dependencies: none.

Example Playbook
-----------------
Basic run on supported hosts:

```yaml
- hosts: all
  become: true
  roles:
    - role: iamenr0s.ansible_role_docker
```

Add real users to the `docker` group:

```yaml
- hosts: all
  become: true
  vars:
    docker_users:
      - alice
      - bob
  roles:
    - role: iamenr0s.ansible_role_docker
```

Running outside a container (enables `overlay2`):

```yaml
- hosts: all
  become: true
  vars:
    docker_run_not_in_container: true
  roles:
    - role: iamenr0s.ansible_role_docker
```

CI & Release (maintainers)
----------------------------
A single workflow (`.github/workflows/molecule.yml`) runs lint and the full Molecule distro matrix on pushes to `main`, PRs, and `v*` tags. On `v*` tags, a `release` job publishes to Ansible Galaxy after all tests pass.

The Galaxy API key lives in the `galaxy` GitHub environment, which only `v*` tags may target. One-time setup:

```bash
# Galaxy publishing key (environment-scoped, get it from galaxy.ansible.com/ui/token)
gh secret set GALAXY_API_KEY --env galaxy --repo iamenr0s/ansible-role-docker

# Code scanning notifications (Slack webhook URL; for Discord append /slack to the webhook URL)
gh secret set SECURITY_ALERT_WEBHOOK --env galaxy --repo iamenr0s/ansible-role-docker
```

`.github/workflows/code-scanning-notify.yml` polls the code-scanning API every 6 hours and posts new or updated open alerts to that webhook (GitHub Actions cannot trigger on `code_scanning_alert` directly).

To release: tag a commit `vX.Y.Z` and push the tag — CI gates the Galaxy publish.

## Contributing

Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for the
local pipeline commands and pull request checklist. This project follows the
[Contributor Covenant Code of Conduct](CODE_OF_CONDUCT.md).

## Security

See [SECURITY.md](SECURITY.md) — GitHub private vulnerability reporting, no
public issues for security bugs.

## License

This project is licensed under the [MIT License](LICENSE).

## Author Information

Author: iamenr0s

Galaxy: `iamenr0s.ansible_role_docker`
