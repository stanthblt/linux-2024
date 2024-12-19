# TP2 dév : packaging et environnement de dév local

## Sommaire

- [TP2 dév : packaging et environnement de dév local](#tp2-dév--packaging-et-environnement-de-dév-local)
  - [Sommaire](#sommaire)
- [I. Packaging](#i-packaging)
  - [1. Calculatrice](#1-calculatrice)
  - [2. Chat room](#2-chat-room)
  - [3. Ur own](#3-ur-own)
  - [4. Un ptit mot](#4-un-ptit-mot)

# I. Packaging

## 1. Calculatrice

➜ **Créez un dépôt git dédié au code de votre calculatrice**

[4. Un ptit mot](#4-un-ptit-mot)

➜ **Packager dans une image Docker l'application de calculatrice réseau**

- packaging du serveur, **pas le client**
- créer un `Dockerfile` qui permet de build une image qui contient votre calculatrice
  - `FROM` ce que vous voulez
  - il faut qu'il y ait Python
  - les librairies s'il y a des libs pour votre calculatrice
  - un `ENTRYPOINT` qui lance automatiquement la calculatrices

➜ **Environnement : adapter le code**

- on doit pouvoir choisir sur quel port écoute la calculatrice au lancement du conteneur
  -  on peut définir la variable d'environnement `CALC_PORT` pour le conteneur
  -  par exemple `CALC_PORT=8888`
- votre code doit donc :
  - récupérer la valeur de la variable d'environnement `CALC_PORT` si elle existe
  - vous devez vérifier que c'est un entier
  - écouter sur ce port là
- ainsi, on peut choisir le port d'écoute comme ça avec `docker run` :

```bash
$ docker run -e CALC_PORT=6767 -d calc
```

➜ **Adapter le `Dockerfile`**

- définir la variable d'environnement `CALC_PORT` à sa valeur par défaut directement dans le `Dockerfile`
- comme ça, y'a une valeur par défaut !

➜ **Ecrire un `docker-compose.yml`**

- créer un `docker-compose.yml` qui permet de lancer votre image calculatrice
- il build automatiquement le `Dockerfile` pour lancer la calculatrice
  - on peut faire ça en ajoutant une clause `build:` dans le `docker-compose.yml`

➜ **Logs : adapter le code si besoin**

- tous les logs de la calculatrice DOIVENT sortir en sortie standard
  - genre si tu le lances à la main ton code, ça print les logs dans le terminal
- en effet, il est courant qu'un conteneur génère tous ses logs en sortie standard
- parce qu'en fait, on peut consulter tout ce qu'a print le conteneur dans son terminal avec la commande `docker logs`

```bash
docker logs <ID_or_name>
```

🌞 **Le lien vers ton dépôt**

- dans le compte-rendu, le lien vers le dépôt git de ta calculatrice, et c'est tout
- il doit donc contenir :
  - le code serveur
  - le code client
  - un `Dockerfile`
  - un `docker-compose.yml`
  - un `README.md`
    - il donne un exemple de commande build
    - il donne un exemple de commande pour up le `docker-compose.yml`
    - il explique qu'on peut modifier le port d'écoute en modifiant une variable d'environnement

## 2. Chat room

➜ Idem, tu crées un dépôt git dédié pour ça

- tu peux aussi te reservir de ton dépôt git déjà existant pour la chat room (normalement)

🌞 **Packager l'application de chat room**

- uniquement le lien du dépôt git dans le compte-rendu
- pareil : on package le serveur (pas le client)
- `Dockerfile` et `docker-compose.yml` à ajouter
- code à adapter :
  - logs en sortie standard
  - variable d'environnement `CHAT_PORT` pour le port d'écoute
  - variable d'environnement `MAX_USERS` pour limiter le nombre de users dans chaque room (ou la room s'il y en a qu'une)
- encore un `README.md` propre qui montre comment build et comment run (en démontrant l'utilisation des variables d'environnement)

## 3. Ur own

T'es un dév, donc tu dév nan ? Sur du code non ? :d

🌞 **Packager une application à vous**

- que vous avez dév en cours, ou en perso
- dans l'idéal, un truc où y'a au moins deux conteneurs, que ce soit rigolo, mais np sinon
- ajoutez une gestion de variable d'environnement pareil ce serait cool et sûrement utile ?
- écrivez un `Dockerfile` qui porte l'environnement de votre application
- écrivez un `docker-compose.yml` qui lance les bails
- écrivez un `README.md` qui donne la commande pour lancer les bails
- (ré)utilisez un dépôt git dédié
  - je veux pouvoir juste le clone, et suivre le `README.md` qui m'indique la commande `docker compose`
