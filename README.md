# Compo docker **GeoServer** avec support HTTPS

## Utilisation en local
Cette compo est prête à l'usage pour une exécution en local. Caddy configure quand même un service en HTTPS, utilisant un nom de domaine aotu-magiquement résolu vers l'IP locale.
L'accès se fait alors sur https://geoserver-127-0-0-1.nip.io.


## Support HTTPS

Caddy sert de reverse-proxy avec support d'un certificat SSL auto-généré.
De base avec cette config, Caddy va utiliser un certificat non secure, vous aurez donc des alertes de sécurité dans le navigateur. C'est acceptable pour une utilisation locale. Cf supra.

**Pour un déploiement en production**, trois actions sont requises : 
- remplacer le FQDN dans le fichier .env par le nom de domaine du serveur
- commenter la ligne `tls internal` dans caddy/etc/Caddyfile
- ***changer tous les mots de passe écrits en clair dans le docker-compose.yml***, cf infra

Puis lancez la compo, Caddy devrait automatiquement générer un certificat SSL adapté à votre nom de domaine.

## Identifiants / mots de passe

Vous pouvez changer les identifiants et mot de passe par défaut pour Geoserver et Postgis, via l'emploi de variables d'environnement:

- soit en copiant le fichier .env-sample en .env et en ajustant son contenu
- soit en exportant les variables d'environnement souhaitées directement dans votre terminal avant de lancer la compo. 

_Attention, les identifiants PostgreSQL sont initialisés au premier lancement_. Pour qu'un changement d'identifiants soit pris en compte, il faudra détruire et recréer la compo (enfin, les volumes) via `docker compose down -v`, ce qui va aussi vous faire perdre le contenu. Ou bien les changer de façon classique, à la main, dans la base.

## Postgis

**Postgis** est configuré avec un port 5432 ouvert mais seulement sur localhost. Ce qui protège postgis d'une attaque extérieure, et contraint à ***utiliser un tunnel SSH*** pour s'y connecter.
```
# Open localhost port 15432, for an SSH tunnel through the machine (my_username@my_server) running the compo
ssh -L 15432:127.0.0.1:5432 my_username@my_server
```