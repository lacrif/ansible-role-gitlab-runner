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

### Installation et enregistrement

- `gitlab_runner_version` : Version du paquet ("" = dernière disponible).
- `gitlab_runner_register` : Booléen. Si `true`, active l'étape d'enregistrement (désactivée par défaut).
- `gitlab_runner_registration_token` : Token d'enregistrement (NE PAS COMMITTER).
- `gitlab_runner_registration_url` : URL de l'instance GitLab (par défaut `https://gitlab.com/`).
- `gitlab_runner_executor` : Executor pour l'enregistrement (par défaut `shell`).
- `gitlab_runner_apt_repo` / `gitlab_runner_yum_repo` : URLs des scripts officiels d'ajout des dépôts.
- `gitlab_runner_service_name` : Nom du service (`gitlab-runner`).

### Gestion du fichier de configuration

- `gitlab_runner_manage_config` : Booléen. Si `true`, gère le fichier `/etc/gitlab-runner/config.toml` via template (par défaut `false`).
- `gitlab_runner_concurrent` : Nombre de jobs concurrents (par défaut `1`).
- `gitlab_runner_check_interval` : Fréquence de vérification de nouveaux jobs en secondes (par défaut `0`).
- `gitlab_runner_log_level` : Niveau de logs: `debug`, `info`, `warn`, `error`, `fatal`, `panic` (par défaut `info`).
- `gitlab_runner_log_format` : Format des logs: `runner`, `text`, `json` (par défaut `runner`).
- `gitlab_runner_shutdown_timeout` : Délai de fermeture gracieuse en secondes (par défaut `0`).
- `gitlab_runner_runners` : Liste des runners à configurer (par défaut `[]`).

Exemple de configuration d'un runner :

```yaml
gitlab_runner_runners:
  - name: "my-shell-runner"
    url: "https://gitlab.com/"
    token: "RUNNER_TOKEN"
    executor: "shell"
    description: "Shell executor runner"
    tag_list: ["shell", "linux"]
    run_untagged: true
    locked: false
  - name: "my-docker-runner"
    url: "https://gitlab.com/"
    token: "RUNNER_TOKEN_2"
    executor: "docker"
    description: "Docker executor runner"
    tag_list: ["docker"]
    docker:
      image: "alpine:latest"
      privileged: false
      volumes: ["/cache"]
```

Ne stockez jamais `gitlab_runner_registration_token` ou les tokens des runners en clair dans le dépôt. Utilisez :

- Ansible Vault
- Variables d'inventaire chiffrées
- Passer via `--extra-vars` au moment de l'exécution

## Utilisation

### Exemple d'utilisation basique

Installer GitLab Runner sans configuration du fichier config.toml :

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

### Exemple avec gestion du fichier de configuration

Installer et configurer GitLab Runner avec gestion du fichier config.toml :

```yaml
- name: Install and configure GitLab Runner
  hosts: runners
  become: true
  vars:
    gitlab_runner_manage_config: true
    gitlab_runner_concurrent: 4
    gitlab_runner_log_level: "info"
    gitlab_runner_runners:
      - name: "production-shell-runner"
        url: "https://gitlab.example.com/"
        token: "{{ vault_runner_token_1 }}"
        executor: "shell"
        tag_list: ["shell", "production"]
        run_untagged: false
      - name: "docker-runner"
        url: "https://gitlab.example.com/"
        token: "{{ vault_runner_token_2 }}"
        executor: "docker"
        tag_list: ["docker"]
        docker:
          image: "alpine:latest"
          privileged: false
          volumes: ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
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
