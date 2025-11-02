# Notes de version

Toutes les modifications notables de ce projet sont documentées dans ce fichier.

## [1.0.1] - 2025-11-02
### Ajoutés
- Ajout de la configuration du fichier config.toml

## [1.0.0] - 2025-11-01
### Ajoutés
- Implémentation initiale du rôle `ansible-role-gitlab-runner` :
  - `defaults/main.yml` avec les variables documentées
  - `tasks/main.yml`, `tasks/setup-Debian.yml`, `tasks/setup-RedHat.yml`
  - `handlers/main.yml` pour le redémarrage et la mise à jour du cache apt
  - Scénario Molecule `molecule/default` avec `converge.yml` et `verify.yml`
  - `README.md` avec les instructions d'utilisation et de test
  - Exemple `playbook.yml`

### Correction / Améliorations
- Linting et corrections des YAML et des FQCN pour satisfaire `ansible-lint`.
- Ajustement du scénario Molecule et des handlers pour s'exécuter dans des conteneurs Docker (sans sudo).

