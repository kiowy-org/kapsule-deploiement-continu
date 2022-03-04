# Kapsule "Déploiement continu"

Bienvenue dans le lab du webinaire "Déploiement continu" :rocket:
Dans ce dépôt vous retrouverez les instructions et fichiers qui vous permettront d'effectuer la partie pratique de ce webinaire.

## Prerequis

Pour réaliser les expérimentations pratiques de ce webinaire, vous aurez besoin d'une stack Cloud/Kubernetes de base comprenant `Docker`, `KinD`, `Kubectl`.

- [Installation de Docker](https://docs.docker.com/get-docker/)
- [Installation de KinD](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)
- [Installation de Kubectl](https://kubernetes.io/fr/docs/tasks/tools/install-kubectl/)

## Mise en place d'un cluster de test

Pour tester les points que nous allons aborder aujourd'hui, nous allons donc utiliser un cluster Kubernetes dans Docker via KinD.
Pour ce faire, exécutez le script de construction `00_Prerequis/bootstrap.sh` :

```shell
    ./bootstrap.sh
```

Ce script met en place le cluster, met en place l'ingress nginx et installe l'outil de déploiement Argo Rollout dans le cluster. C'est sur Argo rollout que nous allons expérimenter les différents types de déploiement.

## Appliquer un type de déploiement

Pour chacun des types de déploiement présentés au cours de ce webinaire, vous trouverez un dossier correspondant dans le dossier `01_argo-rollout/`.
Appliquez les fichiers contenus via `kubectl` en utilisant l'option `-k` (exemple pour le type de déploiement `canary`) :

```shell
    kubectl apply -k canary/
```

## Argo Rollout

### Installation

Pour utiliser `Argo Rollout`, vous devez en installer le plugin kubectl correspondant. Pour ce faire, référez-vous à la section "Kubectl Plugin Installation" du tutoriel d'installation de `Argo Rollout`.

[Tutoriel d'installation de Argo Rollout](https://argoproj.github.io/argo-rollouts/installation/)

### Acceder au Dashboard

Une fois l'outil installé, vous pouvez lancer l'interface du tableau de board

```shell
    kubectl argo rollouts dashboard
```

Et y accéder à l'adresse [http://localhost:3100/](http://localhost:3100/)

> **Tips - Rien ne s'affiche ? :rocket:**
> Si vous pouvez accéder au site mais que rien ne s'affiche (page blanche), essayez d'autoriser les cookies :wink:

### Suivre le déploiement

Lors que vous expérimenterez les différents types de déploiement, vous pourrez suivre l'execution de la stratégie sur une page dédiée.
Pour

Ou l'observer directement dans votre terminal via la commande.

```shell
    kubectl argo rollouts get rollout <nom_du_rollout> -w
```

## Nettoyage

Une fois ce webinaire terminé vous pouvez supprimer le cluster `KinD` créé précédemment via le script de destruction `00_Prerequis/teardown.sh` :

```shell
    ./teardown.sh
```
