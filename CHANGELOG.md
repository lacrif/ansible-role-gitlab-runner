# Changelog

All notable changes to this project will be documented in this file.

## [0.1.0] - 2025-11-01
### Added
- Initial implementation of `ansible-role-gitlab-runner` role:
  - `defaults/main.yml` with documented variables
  - `tasks/main.yml`, `tasks/setup-Debian.yml`, `tasks/setup-RedHat.yml`
  - `handlers/main.yml` for restart and apt cache update
  - Molecule scenario `molecule/default` with `converge.yml` and `verify.yml`
  - `README.md` with usage and testing instructions
  - `playbook.yml` example

### Fixed / Improved
- Linted and fixed YAML and FQCN issues to satisfy `ansible-lint`.
- Adjusted Molecule scenario and handlers to run in Docker-based containers (no sudo).
