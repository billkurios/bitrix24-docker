![Bitrix24 Docker](/docs/assets/bitrix24-docker-logo.png)

# Bitrix24 Docker: Environnement Web 1C-Portail d'entreprise Bitrix24

Vous permet d'exécuter rapidement et facilement Bitrix24 sur Docker pour le développement local et l'automatisation du processus de test.

## Introduction

Bitrix24 Docker fournit un environnement virtuel prêt à l'emploi optimisé pour le développement et le test des solutions de portail Bitrix24.

### Utilisez Bitrix24 Docker si vous avez besoin:

- Déployez rapidement un environnement Web pour développer des composants et des applications pour Bitrix24
- Débarrassez-vous de nombreux clones de machines virtuelles pour chaque projet
- Exécutez une copie propre du portail sans problèmes techniques complexes
- Automatisez l'exécution des tests dans le cloud (intégration continue)

## Avantages de cette construction

- Disponibilité des services spécifiques à Bitrix24 qui ne sont pas disponibles dans d'autres versions (serveur Push & Pull)
- Compatibilité totale avec bitrix-env, réussissant tous les tests de portail intégrés
- La base de données n'est pas incluse dans l'image principale et est connectée via Docker Compose
- Possibilité d'étendre et de connecter des services supplémentaires (phpMyAdmin, Codeception, etc.)
- Utilisation de variables d'environnement (pour exécuter un conteneur avec différents paramètres)

## Début des travaux

Pour travailler avec Bitrix24 Docker, il est recommandé d'utiliser Docker Compose. 

Ci-dessous se trouve le fichier de configuration `docker-compose.yml` avec MariaDB connecté, où le répertoire de lancement sera monté dans le dossier /local à l'intérieur du conteneur.

Vous pouvez changer la version de la base de données ou connecter plusieurs bases de données différentes en même temps en complétant ce fichier avec les instructions appropriées.

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

Bitrix24 Docker inclut les fichiers d'installation principaux, donc après le démarrage des conteneurs, vous verrez immédiatement la page pour installer une nouvelle copie du portail à l'adresse http://localhost. 

Si vous connectez Bitrix24 Docker à un projet existant, modifiez la valeur des `volumes` de la section `web` en `./:/home/bitrix/www`. 

### Autres scripts de lancement

- [Instructions pas à pas pour l'installation du portail](/docs/01-install-step-by-step.md)
- [Comment se connecter à phpMyAdmin](/docs/02-phpmyadmin-setup.md)
- Exécution de tests de détection de code

## Noter

Ceci est une version non officielle et est uniquement destinée au développement local. N'utilisez pas cette image dans un environnement de production.