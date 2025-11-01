# Rôle Ansible: Gitlab Runner

[![CI](https://github.com/lacrif/ansible-role-temaplate/actions/workflows/ci.yml/badge.svg)](https://github.com/lacrif/ansible-role-template/actions/workflows/ci.yml)

Rôle Ansible minimal pour installer et configurer GitLab Runner sur
Debian/Ubuntu et RHEL/Rocky.

Ce dépôt contient :

- `defaults/main.yml` : variables par défaut et documentation.
- `tasks/` : logique d'installation (fichiers spécifiques par distribution).
- `handlers/main.yml` : handlers pour redémarrer le service / mettre à jour le cache.
- `molecule/` : scénario `default` pour tests locaux avec Docker.

## Prérequis

- Ansible >= 2.15
- Docker (pour exécuter les tests Molecule localement)

## Variables principales (voir `defaults/main.yml`)

- `gitlab_runner_version` : Version du paquet ("" = dernière disponible).
- `gitlab_runner_register` : Booléen. Si `true`, active l'étape d'enregistrement (désactivée par défaut).
- `gitlab_runner_registration_token` : Token d'enregistrement (NE PAS COMMITTER).
- `gitlab_runner_registration_url` : URL de l'instance GitLab (par défaut `https://gitlab.com/`).
- `gitlab_runner_executor` : Executor pour l'enregistrement (par défaut `shell`).
- `gitlab_runner_apt_repo` / `gitlab_runner_yum_repo` : URLs des scripts officiels d'ajout des dépôts.
- `gitlab_runner_service_name` : Nom du service (`gitlab-runner`).

Ne stockez jamais `gitlab_runner_registration_token` en clair dans le dépôt. Utilisez :

- Ansible Vault
- Variables d'inventaire chiffrées
- Passer via `--extra-vars` au moment de l'exécution

## Utilisation

Exemple d'utilisation dans un playbook :

```yaml
- name: Install GitLab Runner
  hosts: runners
  become: true
  vars:
    # By default registration is disabled; set to true and provide a token to register
    gitlab_runner_register: false
  roles:
    - lacrif.gitlab_runner
```

Pour installer la dernière version sans enregistrer le runner :

```bash
ansible-playbook -i inventory playbook.yml
```

Pour activer l'enregistrement lors de la convergence (ex : CI ou run manuel) :

```bash
ansible-playbook -i inventory playbook.yml --extra-vars "gitlab_runner_register=true gitlab_runner_registration_token=MYTOKEN"
```

## Tests avec Molecule

Prérequis pour tests : Docker installé et accessible par l'utilisateur courant.

Installer les dépendances Python (optionnel) :

```bash
pip install "molecule[docker]" ansible-lint yamllint
```

Exécuter les tests Molecule :

```bash
molecule test
```

Pour changer le système d'exploitation

```bash
export MOLECULE_DISTRO_NAME=rockylinux && export MOLECULE_DISTRO_VERSION=9 && molecule test
export MOLECULE_DISTRO_NAME=ubuntu && export MOLECULE_DISTRO_VERSION=noble && molecule test
export MOLECULE_DISTRO_NAME=debian && export MOLECULE_DISTRO_VERSION=trixie && molecule test
```

## Limitations et améliorations possibles

- Ajouter la logique idempotente d'enregistrement du runner (avec gestion du token via Vault).
- Remplacer les scripts `curl | bash` par une gestion explicite des sources (fichiers `.list` / `.repo`) si nécessaire pour des environnements sans accès internet.
- Ajouter des scénarios Molecule par distribution pour tester plus précisément (Rocky, Ubuntu, Debian).

## Licence

Voir le fichier `LICENSE`.

## Auteur

Lacrif
