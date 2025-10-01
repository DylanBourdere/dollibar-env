# dollibar-env

Ensemble de services Docker Compose pour travailler simultanément sur les versions 16 à 22 de Dolibarr.

## Prérequis

- Docker et Docker Compose
- Fork Git de Dolibarr pour chaque version que vous souhaitez modifier
- Entrées DNS/hosts pointant vers votre machine pour `dylan.local` et `dolibarr16.local` … `dolibarr22.local`

## Structure du projet

```
.
├── dashboard/               # Interface web servie à dylan.local
│   ├── index.html
│   └── nginx.conf
├── docker-compose.yml        # Stack complète Traefik + Dolibarr + MariaDB
└── instances/
    ├── dolibarr-16/
    ├── dolibarr-17/
    ├── dolibarr-18/
    ├── dolibarr-19/
    ├── dolibarr-20/
    ├── dolibarr-21/
    └── dolibarr-22/
```

Chaque dossier `instances/dolibarr-XX` est monté dans le conteneur correspondant, ce qui vous permet d'éditer le code localement et de committer directement sur votre fork.

## Mise en place

1. Clonez votre fork Dolibarr dans le dossier prévu pour chaque version. Exemple :
   ```bash
   git clone git@github.com:mon-fork/dolibarr.git instances/dolibarr-16
   git -C instances/dolibarr-16 checkout 16.x
   ```
   Répétez pour les autres versions souhaitées.

2. Vérifiez/ajoutez les entrées dans votre fichier hosts (ou votre DNS interne) :
   ```
   127.0.0.1 dylan.local
   127.0.0.1 dolibarr16.local
   127.0.0.1 dolibarr17.local
   127.0.0.1 dolibarr18.local
   127.0.0.1 dolibarr19.local
   127.0.0.1 dolibarr20.local
   127.0.0.1 dolibarr21.local
   127.0.0.1 dolibarr22.local
   ```

3. Lancez la stack :
   ```bash
   docker compose up -d
   ```

4. Accédez au dashboard : http://dylan.local

5. Ouvrez les instances directement via les liens sur le dashboard ou via les URL dédiées (`http://dolibarr16.local`, etc.).

## Notes

- Les bases de données MariaDB sont persistées via des volumes Docker (`dbXX_data`).
- Le tableau de bord Traefik est disponible sur http://localhost:8080 (protégé par défaut par Traefik, configurez selon vos besoins).
- Adaptez les variables d'environnement (utilisateur/mot de passe) dans `docker-compose.yml` si besoin.
