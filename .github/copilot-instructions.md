# Instructions

Voici des instructions destinées à l'agent Copilot pour travailler sur ce dépôt "ansible-role-gitlab-runner".

## Objectif

- Aider à développer, corriger et documenter le rôle Ansible pour gitlab-runner.

## Contexte du dépôt

- Structure principale : `tasks/`, `defaults/`, `handlers/`, `vars/`, `molecule/`, `meta/`.
- Ce dépôt contient un rôle Ansible destiné à installer et configurer un template et des scénarios Molecule pour tests locaux.

## Règles générales

- Répondre en français.
- Proposer des modifications minimales et ciblées : petits commits, changement explicite des fichiers, respecter le style YAML/Ansible existant.
- Avant toute modification significative, lister les fichiers à changer et expliquer brièvement pourquoi.
- Respecter les conventions Ansible : utiliser `defaults/main.yml` pour valeurs par défaut, `vars/` pour variables liées à la plateforme, `tasks/` pour logique, `handlers/` pour handlers.

## Pratiques recommandées

- Sécurité : éviter d'exposer des secrets dans le dépôt. Si une tâche a besoin d'un secret, documenter clairement comment fournir la valeur via Vault, variables d'environnement, ou inventory.
- Tests : utiliser Molecule (scénarios dans `molecule/default/`) et ajouter/mettre à jour `converge.yml` et `verify.yml` quand on change le rôle. Fournir une simple commande pour exécuter Molecule localement.

## Compatibilité multi-plateforme

- Ne générer que pour les plateformes : rockylinux 9, ubuntu noble et debian trixie
- multi-plateforme : si une tâche dépend d'une distribution, isoler la logique dans `tasks/setup-<Distro>.yml` et conditionner l'inclusion via `ansible_os_family` ou `ansible_distribution`.

## Fichiers et conventions spécifiques

- `tasks/main.yml` : inclure les fichiers `setup-RedHat.yml`, `setup-Debian.yml`, selon la plateforme. Ne pas dupliquer la logique.
- `defaults/main.yml` : documenter chaque variable par un bref commentaire et fournir des valeurs par défaut raisonnables.
- `molecule/` : s'assurer que `molecule.yml` et `converge.yml` sont cohérents; Si tu modifies des handlers ou des services, adapte les tests de vérification (`verify.yml`).

## Lorsque tu proposes un correctif ou une nouvelle fonctionnalité

- Fournir un court résumé (2-4 lignes) expliquant le problème et la solution.
- Montrer les fichiers modifiés et un extrait des changements clés (bloc YAML/Ansible) dans la PR description.
- Ajouter ou mettre à jour des tests Molecule basiques (vérification d'un service, présence d'un binaire, groupe d'utilisateurs, etc.).
- Expliquer comment exécuter les tests localement (commande Molecule) et noter toute dépendance ou prérequis.

## Comportement attendu pour les réponses de l'agent

- Fournir directement les modifications de fichiers sous forme de patchs ou d'édits (si intégré au repo), pas seulement des instructions textuelles.
- Avant d'appliquer un patch qui change plus de 3 fichiers, lister les fichiers concernés et demander confirmation.
- Après modification, exécuter (ou indiquer comment exécuter) les tests Molecule pertinents et fournir les résultats ou les commandes nécessaires.

## Tests recommandés

- Exécuter molecule : `molecule test`
- Linter YAML/Ansible : `ansible-lint .` et `yamllint .` si disponibles

## Notes finales

- Prioriser la clarté et la maintenabilité. Préfère des changements incrémentaux testés.
- Si tu rencontres des parties ambiguës du rôle, propose deux options de correction avec leurs avantages/inconvénients.

Merci de respecter ces consignes chaque fois que tu contribues au dépôt.
