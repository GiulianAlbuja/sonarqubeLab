# Análisis de Proyecto con SonarQube Usando Docker

Este documento describe los pasos para configurar y usar SonarQube en un contenedor Docker para analizar un proyecto web.

## Requisitos Previos

- **Docker Desktop**: Asegúrate de tener Docker Desktop instalado en tu máquina. Esto te permitirá usar contenedores Docker sin necesidad de configurar manualmente bases de datos u otros componentes.

## Pasos para Configurar y Usar SonarQube

### 1. Descargar Docker Desktop

- Descarga e instala Docker Desktop desde [la página oficial de Docker](https://www.docker.com/products/docker-desktop). Esto nos permitirá ejecutar SonarQube en un contenedor, simplificando la configuración.

### 2. Configurar el Archivo `docker-compose.yaml`

- Crea un archivo llamado `docker-compose.yaml` con el siguiente contenido:

```yaml
version: '2'

services:
  sonarqube:
    image: sonarqube
    ports:
      - "9000:9000"
    networks:
      - sonarnet
    environment:
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonar
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres
    networks:
      - sonarnet
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data

networks:
  sonarnet:
    driver: bridge

volumes:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_bundled-plugins:
  postgresql:
  postgresql_data:
```
Este archivo configura dos servicios: SonarQube y una base de datos PostgreSQL necesaria para SonarQube. Utiliza volúmenes para persistir datos y redes para conectar los servicios.

### 3. Iniciar los Contenedores
- Navega a la carpeta donde se encuentra el archivo docker-compose.yaml y ejecuta el siguiente comando para iniciar los contenedores:
`docker-compose up -d`

	Esto iniciará SonarQube y la base de datos en segundo plano.

### 4. Acceder a SonarQube
- Abre un navegador web y dirígete a `http://localhost:9000`. Al acceder por primera vez, te pedirá credenciales:

	Usuario: `admin`
	Contraseña: `admin`

	![image](https://github.com/user-attachments/assets/1487ff46-5693-40da-ab84-d9ece569f6ed)

	Después de iniciar sesión, se te pedirá que cambies la contraseña.

### 5. Crear un Nuevo Proyecto
- Una vez dentro, podrás crear un nuevo proyecto. Elige la opción para hacerlo local.

  ![image](https://github.com/user-attachments/assets/de8e4344-b185-4c7b-b190-9d4b7293c5af)


	Nombre del Proyecto: Por ejemplo, `PRUEBA`.

	![image](https://github.com/user-attachments/assets/b7054218-dad2-474d-a7aa-a450a1eeb474)


- Selecciona las opciones globales y continúa.

  ![image](https://github.com/user-attachments/assets/3eed20f0-2d9b-484b-abb2-a9e55defd24a)

	
### 6. Configurar el Análisis del Proyecto
- SonarQube te preguntará cómo deseas analizar tu proyecto. Puedes elegir entre varias opciones como Jenkins, GitHub Actions, etc. Para análisis local, selecciona localmente.

  ![image](https://github.com/user-attachments/assets/2234fc6d-f809-4c87-b29b-bf73096b53a3)


### 7. Generar un Token de Autenticación
- En el primer paso del análisis local, necesitarás generar un token. Haz clic en generar y el servicio de mostrará el token generado.

  ![image](https://github.com/user-attachments/assets/da836e77-3e0b-42f4-a968-510ed85f8af9)


### 8. Descargar y Configurar el Escáner
- Selecciona el tipo de análisis adecuado para tu proyecto. Si es un proyecto web, selecciona otros y luego el sistema operativo.

- Visita el enlace proporcionado para descargar el escáner. Una vez descargado, agrega la carpeta bin del escáner a las variables de entorno del sistema (PATH).

![image](https://github.com/user-attachments/assets/68f57f42-78ff-4b26-967a-7b32b4ae53b0)


### 9. Ejecutar el Análisis
- Abre una terminal en la carpeta raíz del proyecto que deseas analizar y ejecuta el comando proporcionado por SonarQube para iniciar el análisis.

![image](https://github.com/user-attachments/assets/2cdf4ba7-0da6-45c3-9b4e-a453eb80917a)

### 10. Verificar el Informe
- Una vez finalizado el análisis, actualiza la página de SonarQube. El informe del análisis se reflejará en la interfaz web.

## Secciones del Informe en SonarQube
SonarQube proporciona varias secciones en el informe:

- Overview: Ofrece una vista general del estado del proyecto, incluyendo el número de problemas encontrados y el progreso general.
- Issues: Muestra los problemas encontrados en el código, clasificados por severidad (bloqueantes, críticos, mayores, menores).
- Security Hotspots: Identifica posibles problemas de seguridad en el código.
- Measures: Presenta métricas de calidad del código como la cobertura de pruebas y la complejidad.
- Code: Muestra el código fuente y los problemas encontrados en él.
- Activity: Proporciona un historial de los análisis y los cambios en los problemas a lo largo del tiempo.

