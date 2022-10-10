# Comment se connecter à phpMyAdmin

Si vous utilisez phpMyAdmin pour votre base de données, vous pouvez le connecter via Docker Compose. 

## Étape 1 : Créer un fichier de configuration

Dans votre dossier de projet, créez `docker-compose.yml` avec le contenu suivant:

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
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    links:
      - mysql:mysql
    ports:
      - 8181:80
    environment:
      PMA_HOST: mysql
      MYSQL_USERNAME: bitrix
      MYSQL_PASSWORD: +Tr+()8]!szl[HQIsoT5
```

## Étape 2 : Lancez Bitrix24 Docker

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
Starting myproject_phpmyadmin_1 ... done
```

## Étape 3 : Connexion à phpMyAdmin

Ouvrez un navigateur et accédez à http://localhost:8181, vous verrez la fenêtre de connexion standard `phpMyAdmin`. 

Utilisez les paramètres de connexion du fichier de configuration:

- `Username`: bitrix
- `Password`: +Tr+()8]!szl[HQIsoT5

## Informations Complémentaires

- [Build officiel de phpMyAdmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin/)