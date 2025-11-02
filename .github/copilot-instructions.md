# INSTRUCTIONS POUR COPILOT

Voici des instructions destinées à l'agent Copilot pour travailler sur ce dépôt.

## OBJECTIF DU RÔLE

Aider à développer, corriger et documenter le rôle Ansible.

## CONVENTIONS DU DÉPÔT

Ce dépôt contient un rôle Ansible destiné à installer et configurer un template et des scénarios Molecule pour tests locaux.

### STRUCTURE DU DÉPÔT

- `.devcontainer/` : configuration pour les environnements de développement dans des conteneurs.
- `.github/` : workflows GitHub Actions pour CI/CD, templates de PR/issue.
- `defaults/` : valeurs par défaut des variables.
- `handlers/` : handlers utilisés par le rôle.
- `tasks/` : contient la logique principale du rôle.
- `meta/` : métadonnées du rôle.
- `molecule/` : scénarios de test Molecule.
- `vars/` : variables spécifiques à la plateforme.

### FICHIERS CLÉS

- `defaults/main.yml` : documenter chaque variable par un bref commentaire et fournir des valeurs par défaut raisonnables.
- `handlers/main.yml` : définir les handlers utilisés par le rôle.
- `meta/main.yml` : documenter les dépendances du rôle et les métadonnées.
- `molecule/default/converge.yml` : scénario principal pour déployer le rôle dans un conteneur de test.
- `molecule/default/molecule.yml` : configuration de Molecule, y compris les plateformes de test et les images Docker
- `molecule/default/verify.yml` : assertions pour vérifier que le rôle a été appliqué correctement.
- `tasks/main.yml` : point d'entrée principal, inclure des fichiers spécifiques à la plateforme si nécessaire.
- `vars/main.yml` : définir des variables spécifiques à la plateforme si nécessaire.
- `.ansible-lint` : règles personnalisées pour ansible-lint.
- `.yamllint` : configuration pour yamllint.
- `.gitignore` : ignorer les fichiers temporaires, logs, et autres artefacts non pertinents.
- `CHANGELOG.md` : tenir à jour avec les changements significatifs, corrections de bugs et nouvelles fonctionnalités.
- `CODE_OF_CONDUCT.md` : code de conduite pour les contributeurs.
- `CONTRIBUTING.md` : lignes directrices pour contribuer au dépôt.
- `LICENSE` : inclure une licence open source appropriée (ex: MIT, Apache 2.0).
- `README.md` : documenter l'objectif du rôle, les variables configurables, les exemples d'utilisation et les instructions de test.
- `SECURITY.md` : instructions pour signaler des vulnérabilités de sécurité.

## RÈGLES GENÉRALES

- Répondre en français.
- Prioriser la clarté et la maintenabilité. Préfère des changements incrémentaux testés.
- Si tu rencontres des parties ambiguës du rôle, propose deux options de correction avec leurs avantages/inconvénients.
- Respecter le style YAML/Ansible existant.
- Respecter les conventions Ansible et les meilleures pratiques (idempotence, gestion des erreurs, etc.).
- Idempotence : écrire des tâches idempotentes et vérifier que state: present/absent ou creates/removes sont utilisés lorsque c'est pertinent.
- Performance : éviter les boucles inutiles et optimiser les tâches pour réduire le temps d'exécution.
- Effectuer des tests locaux avant de proposer des modifications.

## COMPATIBILITÉ PLATEFORME

- Générer pour les plateformes : rockylinux 9, ubuntu noble et debian trixie
- Si une tâche dépend d'une distribution, isoler la logique dans `tasks/setup-<Distro>.yml` et conditionner l'inclusion via `ansible_os_family` ou `ansible_distribution`.
- Les images utilisées dans `molecule/default/molecule.yml` doivent refléter les plateformes supportées.

## COMMITS ET MESSAGES

- Utiliser des messages de commit clairs et descriptifs.
- Préfixer les messages de commit avec le type de changement (fix, feat, docs, style, refactor, test, chore).
- Inclure des références aux issues ou PRs pertinentes dans les messages de commit.

## CONTRIBUTIONS ET PULL REQUESTS

- Fournir directement les modifications de fichiers sous forme de patchs ou d'édits (si intégré au repo), pas seulement des instructions textuelles.
- Avant d'appliquer un patch qui change plus de 3 fichiers, lister les fichiers concernés et demander confirmation.
- Proposer des modifications minimales et ciblées : petits commits, changement explicite des fichiers
- Fournir un court résumé (2-4 lignes) expliquant le problème et la solution.
- Montrer les fichiers modifiés et un extrait des changements clés (bloc YAML/Ansible) dans la PR description.

## DOCUMENTATION

- Mettre à jour `README.md` et autres documents pertinents pour refléter les changements.
- Chaque variable doit être documentée avec un commentaire expliquant son usage.

## CHANGELOG

- Suivre le versionnage sémantique dans `CHANGELOG.md` et les tags Git.
- Mettre à jour le changelog pour chaque version avec les changements majeurs, mineurs et corrections de bugs.
- Documenter les nouvelles fonctionnalités, corrections de bugs et changements importants dans le changelog.
- Ordrer chronologiquement avec la version la plus récente en haut.

## LOGGING ET DEBUGGING

- Gestion des erreurs : inclure des vérifications et des messages d'erreur utiles.
- Logging : utiliser le module debug pour fournir des informations utiles lors de l'exécution.

## QUALITE DU CODE 

- Lisibilité : utiliser des noms de variables et de tâches clairs et descriptifs.
- Modularité : diviser les tâches complexes en sous-tâches dans des fichiers séparés si nécessaire.
- Exécuter `ansible-lint .` pour vérifier la syntaxe et les bonnes pratiques Ansible.
- Exécuter `yamllint .` pour vérifier la syntaxe YAML.

## SECURITE

- Eviter d'exposer des secrets dans le dépôt. 
- Si une tâche a besoin d'un secret, documenter clairement comment fournir la valeur via Vault, variables d'environnement, ou inventory.

## TESTS MOLECULE

- Exécuter molecule : `molecule test` pour valider les modifications.
- Génnérer des tests Molecule basiques (vérification d'un service, présence d'un binaire, groupe d'utilisateurs, etc.).
- Expliquer comment exécuter les tests Molecule.
- Noter toute dépendance ou prérequis.
- S'assurer que `molecule.yml` et `converge.yml` sont cohérents.
- Si tu modifies des handlers ou des services, adapte les tests de vérification dans `verify.yml`.
- Vérifier que les images Docker dans `molecule/default/molecule.yml` correspondent aux plateformes supportées.
- Utiliser des images personnalisés :
    - `lacrif/rockylinux-systemd:9` pour Rocky Linux 9 avec systemd,
    - `lacrif/ubuntu-systemd:noble` pour Ubuntu 24.04 avec systemd,
    - `lacrif/debian-systemd:trixie` pour Debian 13 avec systemd.
- Utiliser la collection Ansible `hpe.monkeyble` pour mocker des modules non compatibles avec le mode check.

## NOTES FINALES

Merci de respecter ces consignes chaque fois que tu contribues au dépôt.
