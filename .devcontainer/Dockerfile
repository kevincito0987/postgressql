# devcontainer/Dockerfile

FROM mcr.microsoft.com/devcontainers/php:1-8.2-bullseye

# Instala el cliente de PostgreSQL
RUN apt-get update && apt-get install -y \
    postgresql-client

# Limpiar para reducir el tamaño de la imagen
RUN apt-get clean && rm -rf /var/lib/apt/lists/*