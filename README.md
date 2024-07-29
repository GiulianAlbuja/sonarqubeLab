# Guía de Configuración y Uso de SonarQube con Docker Compose

## Introducción

SonarQube es una plataforma de análisis de código que proporciona informes sobre la calidad del código, detectando problemas de mantenimiento, seguridad y más. En esta guía, veremos cómo configurar SonarQube usando Docker Compose y cómo utilizarlo una vez que el contenedor esté en funcionamiento.

## Requisitos Previos

Antes de comenzar, asegúrate de tener los siguientes elementos instalados en tu máquina:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Paso 1: Crear el Archivo `docker-compose.yml`

Para configurar SonarQube con Docker Compose, necesitas crear un archivo llamado `docker-compose.yml`. Este archivo define los servicios, redes y volúmenes necesarios para ejecutar SonarQube.

Aquí tienes un ejemplo de archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  sonarqube:
    image: sonarqube:lts
    container_name: sonarqube
    ports:
      - "9000:9000"
    volumes:
      - sonarqube_data:/opt sonarqube/data
      - sonarqube_extensions:/opt sonarqube/extensions
    networks:
      - sonarnet

volumes:
  sonarqube_data:
  sonarqube_extensions:

networks:
  sonarnet:
