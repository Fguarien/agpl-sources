# agpl-sources — sources correspondantes de nos builds AGPL

Ce dépôt publie, pour chaque logiciel sous **AGPL-3.0** que nous faisons tourner dans une
image **construite localement**, de quoi reconstruire exactement ce qui tourne :

> **source = image/commit amont épinglé + le Dockerfile de ce dépôt.**

Aucun de ces builds ne modifie le code du programme AGPL. Ils partent de l'image publiée par
l'amont, épinglée par digest, et ne changent que l'environnement d'exécution (runtime PHP,
paquets système, dépendances Python). Ce dépôt est publié par prudence, pour que la source
corresponde exactement à la version déployée (AGPL-3.0 § 13).

| Service | Amont (code AGPL, non modifié) | Version / commit amont | Image de base épinglée | Ce que change notre build |
|---|---|---|---|---|
| [`cortendesk/`](cortendesk/) | [marcpope/cortendesk](https://github.com/marcpope/cortendesk) | `v1.14.0` = `2070d598d30cbaec207060a0fbef539ccf5d5d52` | `ghcr.io/marcpope/cortendesk@sha256:3305ea4bca8814e71f6c0dcf49df449cf44f82be4023a0c98e15fdc2ef919545` | remplace les binaires `php`/`php-fpm` et 6 extensions par un PHP 8.4.27 (CVE-2026-109618/109619), construit depuis la recette [docker-library/php](https://github.com/docker-library/php) copiée dans `cortendesk/php-8.4.27/` |
| [`nextcloud-mcp/`](nextcloud-mcp/) | [cbcoutinho/nextcloud-mcp-server](https://github.com/cbcoutinho/nextcloud-mcp-server) | `v0.199.4` = `9d97c248d61ea457d6f3684e370114811822bf5d` | `ghcr.io/cbcoutinho/nextcloud-mcp-server:0.199.4@sha256:4db27920ca54f13965f8a509bcaf8491c2d06a476cc1b77062868ae901c2f0f1` | `apt-get upgrade`, 4 dépendances Python remplacées (PyJWT, cryptography, fsspec, urllib3), `pip` système retiré |

## Reconstruire

```bash
# cortendesk 1.14.0 + PHP 8.4.27
cd cortendesk
docker build -t php-local:8.4.27-fpm-alpine3.24 php-8.4.27/
docker build -t cortendesk-local:1.14.0-php8.4.27 .

# nextcloud-mcp 0.199.4 + couche corrective
cd ../nextcloud-mcp
docker build -t nextcloud-mcp-local:0.199.4-20261020 .
```

Le code source du programme lui-même est celui du commit amont indiqué ci-dessus ; l'image de
base épinglée par digest en est la construction publiée par l'amont.

## Licences

- Les programmes amont sont sous **AGPL-3.0** (voir leurs dépôts).
- Nos Dockerfiles sont publiés sous **AGPL-3.0** (`LICENSE`), comme les programmes qu'ils
  emballent.
- `cortendesk/php-8.4.27/` est une copie de la recette officielle docker-library/php, sous sa
  licence d'origine (MIT, `cortendesk/php-8.4.27/LICENSE`). Voir `NOTICE`.

Ce dépôt est mis à jour à chaque changement de l'un de ces builds.
