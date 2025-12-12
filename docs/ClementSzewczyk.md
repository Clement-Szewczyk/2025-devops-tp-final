# Explication Choix Clément Szewczyk

## Dockerfile

### Backend (Go)

**Multi-stage build** : Le premier stage compile l'application Go avec `golang:1.24-alpine`, le second copie uniquement le binaire dans une image alpine légère. Cela réduit la taille de l'image finale en excluant le compilateur et le code source.

### Frontend (React/Vite)

**Multi-stage build** : Le premier stage build l'application avec `node:24-alpine`, le second utilise `nginx:alpine` pour servir les fichiers statiques. Node.js est absent de l'image finale, seul Nginx reste.

**Configuration dynamique** : Template Nginx avec `envsubst` pour injecter l'URL du backend via variable d'environnement, permettant de changer de backend sans rebuilder l'image.

## Déploiement

### Frontend et Backend

**Render.com** : Choisi pour sa simplicité d'utilisation, son intégration facile avec DockerHub. Possibilé de redéployer automatiquement via une github action.

### Base de données

**Neon.tech** : Offre une base de données PostgreSQL managée avec un plan gratuit.

### Storybook

**GitHub Pages** : Simple à configurer via une action GitHub pour déployer automatiquement la documentation Storybook.


## CI/CD

J'ai choisi une architecture avec un fichier par service : 

- `cd.yaml` : pipeline permettant de gérer le déploiement continu du frontend et backend sur Render.com à chaque push et pull request sur la branche main.

- `ci-frontend.yaml` : pipeline de pour le frontend (lint et tests)
- `ci-backend.yaml` : pipeline pour le backend (vet et tests)
- `ci-e2e.yaml` : pipeline pour les tests end-to-end 
- `ci-docker.yaml` : pipeline pour builder et push les images Docker sur DockerHub
- `ci-storybook.yaml` : pipeline pour builder et déployer Storybook sur GitHub Pages
- `ci-all.yaml` : pipeline principal orchestrant les autres pipelines avec des dépendances et conditions d'exécution (ex: ne builder les images Docker que sur la branche main, ne déployer Storybook qu'après le build Docker).
- `release.yaml` : pipeline pour créer des releases GitHub avec les changelogs basés sur les messages de commit.

Grâce à cette architecture, chaque pipeline est indépendant et peut être modifié ou étendu sans impacter les autres. Le fichier `ci-all.yaml` permet de coordonner l'exécution des pipelines dans le bon ordre avec les bonnes conditions.

## Stratégie Git

J'ai mis en place une stratégie Git qui permet de faire d'éxécuter les test sur la branch develop à chaque push ou pull request. Une fois que tout est validé, on peut merger sur main qui déclenche le déploiement automatique, le build des images Docker, le déploiement de Storybook et la création de release (si besoin).


## Problèmes rencontrés

### Reverse Proxy Nginx

**1. Problème avec le header Host**

Le reverse proxy Nginx envoyait un header Host au service Render. Ce header ne correspondait pas à ce que le load balancer de Render attendait. *Résultat* : Render rejetait la requête et renvoyait une erreur 502 Bad Gateway.

**Solution** : Il suffit de ne plus forcer ce header dans la configuration Nginx. En le supprimant, Nginx utilise sa valeur par défaut et Render accepte correctement la requête. Les erreurs 502 disparaissent.

**2. Problème de configuration SSL**

Lors de la connexion en HTTPS entre Nginx et Render, Nginx n’arrivait pas à gérer correctement la vérification du certificat SSL du backend. Cela provoquait des erreurs de type “SSL handshake” ou “certificat invalide”.

**Solution** : Ajouter la bonne directive SSL dans la config Nginx, par exemple `proxy_ssl_server_name on;`, afin que Nginx gère correctement la validation du certificat du backend. Cela rétablit une connexion HTTPS propre avec Render.

*Merci Anthony pour l'aide sur ce point !*

### Github Release

J'ai beaucoup eu de mal à configurer la génération automatique des releases GitHub. Le problème venait du fait que les messages de commit n'étaient pas formatés correctement pour être interprétés par l'action GitHub. 

Maintenant j'ai compris que pour que ça fonctionne, il faut utiliser des messages de commit suivant la convention. Par exemple :

- feat: ajout d'une nouvelle fonctionnalité
- fix: correction d'un bug
- docs: mise à jour de la documentation
- chore: tâches diverses (mise à jour des dépendances, nettoyage du code, etc.)

Avec cette convention, la Github Action peut générer automatiquement des changelogs pertinents et créer des releases basées sur les changements réels apportés au code.
