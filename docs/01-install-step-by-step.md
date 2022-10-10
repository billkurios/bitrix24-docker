# Instructions pas à pas pour installer le portail Bitrix24

L'instruction décrit le processus de lancement de l'image Bitrix24 Docker sur une machine locale.

## Étape 1 : Vérification des programmes installés

Assurez-vous que Docker est installé et exécuté localement. Pour les utilisateurs Linux, en plus [nécessite l'installation de Docker Compose](https://docs.docker.com/compose/install/).

```shell
$ docker --version
Docker version 18.09.2, build 6247962

$ docker-compose --version
docker-compose version 1.23.2, build 1110ad01
```

Si vous n'avez pas installé Docker, suivez le guide officiel.

- [Windows](https://docs.docker.com/docker-for-windows/install/)
- [Linux (Ubuntu, CentOS, Debian)](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
- [Mac](https://docs.docker.com/docker-for-mac/install/)

> Pour les utilisateurs de Windows 10, avant l'installation, vous devez faire attention à la version de votre système d'exploitation.
> 
> Donc, si vous avez installé `Windows 10 Pro`, la version standard de Docker CE fera l'affaire, si vous avez `Windows 10 Home`, vous aurez besoin d'une installation supplémentaire de `Docker Toolbox`.
> 
> Suivez les instructions du manuel officiel.

## Étape 2 : Initialisation du projet

Créez un nouveau dossier pour héberger notre projet et accédez-y.

Dans la console, cela ressemblera à ceci :

```shell
mkdir myproject
cd myproject
```

## Étape 3 : Créer un fichier de configuration

Dans votre dossier de projet, créez `docker-compose.yml` avec le contenu suivant :


```yml
version: '3'
services:
  web:
    image: "akopkesheshyan/bitrix24:latest"
    ports:
      - "80:80"
      - "443:443"
    cap_add:
      - SYS_ADMIN 
    security_opt:
      - seccomp:unconfined
    privileged: true
    volumes:
      - ./:/home/bitrix/www/local
    depends_on:
      - mysql
  mysql:
    image: mariadb
    healthcheck:
      test: "/usr/bin/mysql --user=root --password=+Tr+()8]!szl[HQIsoT5 --execute \"SHOW DATABASES;\""
      interval: 2s
      timeout: 20s
      retries: 10
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: +Tr+()8]!szl[HQIsoT5
      MYSQL_DATABASE: sitemanager
      MYSQL_USER: bitrix
      MYSQL_PASSWORD: +Tr+()8]!szl[HQIsoT5
    command: ['--character-set-server=utf8', '--collation-server=utf8_unicode_ci', '--skip-character-set-client-handshake', '--sql-mode=']   
```

## Étape 4 : Lancez Bitrix24 Docker

Dans la console, exécutez la commande :

```shell
docker-compose up -d
```

Un message apparaîtra à l'écran indiquant que les conteneurs ont été lancés avec succès.

```shell
$ docker-compose up -d
Starting myproject_mysql_1 ... done
Starting myproject_tools_1 ... done
Starting myproject_web_1   ... done
```

En cas d'erreur, assurez-vous que vous êtes dans le répertoire du projet (voir étape 2) et que Docker Compose est installé.

## Étape 4 : Installation du portail

Ouvrez un navigateur et accédez à http://localhost, vous verrez le programme d'installation standard de Bitrix. Grâce à son travail, vous recevrez un portail entièrement fonctionnel.

Tous les fichiers placés dans le répertoire du projet seront disponibles dans le dossier `/local`. Si vous n'utilisez pas encore cet annuaire pour développer vos propres solutions, il est fortement recommandé de vous familiariser avec le cours officiel [Bitrix Framework Developer](https://dev.1c-bitrix.ru/learning/course/ index.php?COURSE_ID=43&LESSON_ID= 2705&LESSON_PATH=3913.4776.2483.2705)