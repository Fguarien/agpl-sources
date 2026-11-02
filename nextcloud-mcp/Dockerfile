# nextcloud-mcp — image dérivée de l'amont 0.199.4, couche corrective CVE (ticket 1034).
# Hôte : vm-addons, /opt/docker/nextcloud-mcp. Build local, tag immuable.
# Retirer quand l'amont publiera (hors quarantaine 7 j) une version qui embarque ces correctifs.
FROM ghcr.io/cbcoutinho/nextcloud-mcp-server:0.199.4@sha256:4db27920ca54f13965f8a509bcaf8491c2d06a476cc1b77062868ae901c2f0f1

USER root
RUN apt-get update && apt-get -y upgrade && rm -rf /var/lib/apt/lists/*
# Versions toutes publiées depuis plus de 7 jours au 2026-10-20. --no-deps puis `uv pip check` :
# on remplace des versions, le résolveur n'en choisit aucune.
# Non corrigés, faute de version compatible : pillow (amont et fastembed : <12, correctif en
# 12.1.1+) et starlette (amont : <1.0, correctif en 1.1.0+) — 2 HIGH, attendre l'amont.
RUN uv pip install --no-cache --python /opt/venv/bin/python --no-deps \
        PyJWT==2.15.0 cryptography==50.0.0 fsspec==2026.6.0 urllib3==2.8.0 \
    && uv pip check --python /opt/venv/bin/python \
    && pip uninstall -y pip
# pip système retiré : il vendore urllib3, msgpack, setuptools vulnérables ; l'image tourne
# depuis /opt/venv et ne s'en sert pas.
USER appuser
