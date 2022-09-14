# Dépendances

Maven (Java), NPM (Javascript), gem (Ruby), Pypi (Python), images Docker, Ansible Galaxy, Terraform, Git submodules, etc.

## Stratégies de mise à jour des dépendances

### Versions *fuzzy*, ranges, latest, etc.

Les dépendances sont mises à jour automatiquement à des étapes clés du cycle de vie de l'application (au build, à l'install, au runtime).

Différentes stratégies de mise à jour possibles (patch, mineure, majeure, toutes, etc.)

**Avantages :**

* mises à jour fréquentes
* automatique

**Inconvénients :**

* pas de maitrise
* pas de traçabilité
* diagnostic difficile en cas de régression
* granularité aléatoire
* effet de surprise

Cette stratégie peut s'avérer chaotique et dangereuse.

Certains gestionnaires de dépendances adressent ce problème avec un fichier de lock (`package-lock.json`, `yarn.lock`, `Gemfile.lock`, etc.), ce qui revient, à peu de choses près, à avoir des versions strictes.

## Versions strictes et mises à jour manuelles

Les versions des dépendances sont déclarées de façon stricte.

Les mises à jour nécessitent l'édition manuelle des versions dans les fichiers de configuration (`pom.xml`, `package.json`, `Dockerfile`, etc.)

La plupart des gestionnaires de dépendances proposent des commandes spécialisées (`mvn versions:use-next-releases`, `npm outdated`, etc.).

**Avantages :**

* 100% maitrisé
* bonne traçabilité (historique Git)
* testable (création d'une branche, lancement de la CI)
* granularité à la carte
* pas de surprise

**Inconvénients :**

* à l'initiative du développeur
* chronophage
* souvent oublié ou mis de côté au profit d'activités plus *urgentes*

Se traduit souvent par l'absence de mise à jour ou des mises à jour trop rares.

## Bot de mise à jour

Un automate inspecte le code, identifie les dépendances utilisées et propose des mises à jour sous forme de *Pull Requests*.

**Avantages :**

* automatique
* testable (création d'une branche, lancement de la CI)
* maitrisé (la *Pull Request* peut être acceptée ou non)
* bonne traçabilité (historique Git)
* pas de surprise

**Inconvénients :**

* temps de mise en place du bot (effort mutualisé)

## Renovate

Sa configuration est co-localisée avec le code à scanner (fichier `renovate.json` à la racine du dépôt Git). Si elle n'existe pas, Renovate créé une *PR* d'*on-boarding* pour proposer une configuration de base.

Pour chaque dépendance à mettre à jour, Renovate créé une *PR* dédiée.

Exemple de PR sur Github : https://github.com/ericcitaire/motif/pull/44

Renovate rebase la branche si nécessaire, ferme la PR si elle n'est plus d'actualité, etc.

Possibilité de configurer des règles de mise à jour : regroupement de dépendances, politique de mise à jour (version majeure, mineure, patch).

Exemple de configuration : https://github.com/ericcitaire/motif/blob/main/renovate.json

Supporte de nombreuse plateformes de gestion de sources : Bitbucket, Github, GitLab, etc.
https://docs.renovatebot.com/modules/platform/

Supporte de nombreux gestionnaires de dépendances et sémantiques de versionning associées : Maven, npm, Docker, etc.
https://docs.renovatebot.com/modules/manager/

Supporte de nombreuses sources de données : Maven Central, npm, Docker Hub, etc. et permet également de les surcharger pour utiliser des miroirs (Artifactory, Nexus, etc.).

Permet la mise à jour de dépendances internes.

## Autres solutions

**Similaire à Renovate :**

* Dependabot (Github)

**Scan devulnérabilités :**

* XRay (JFrog)
* Snyk
