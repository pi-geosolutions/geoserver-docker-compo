# Compo docker **GeoServer** avec support HTTPS

## Utilisation en local
Cette compo est prête à l'usage pour une exécution en local. Caddy configure quand même un service en HTTPS, utilisant un nom de domaine aotu-magiquement résolu vers l'IP locale.
L'accès se fait alors sur https://geoserver-127-0-0-1.nip.io.

## Support HTTPS
Caddy sert de reverse-proxy avec support d'un certificat SSL auto-généré.
De base avec cette config, Caddy va utilisé un certificat non secure, vous aurez donc des alertes de sécurité dans le navigateur. C'est acceptable pour une utilisation locale. Cf supra.

Pour un déploiement en production, deux actions sont requises : 
- remplacer le FQDN dans le fichier .env par le nom de domaine du serveur
- commenter la ligne `tls internal` dans caddy/etc/Caddyfile

Puis lancez la compo, Caddy devrait automatiquement générer un certificat SSL adapté à votre nom de domaine.

## Postgis

**Postgis** est configuré avec un port 5432 ouvert mais seulement sur localhost. Ce qui protège postgis d'une attaque extérieure, et contraint à utiliser un tunnel SSH pour s'y connecter.

