
# Microservicios de Gestión de Matrículas

Arquitectura backend distribuida para la gestión académica de estudiantes, cursos y matrículas. Este proyecto implementa patrones de diseño de microservicios, seguridad perimetral con OAuth2/OpenID Connect y despliegue contenedorizado.

> **Nota:** Proyecto de portafolio enfocado en la comunicación entre APIs, Service Discovery, Configuración Centralizada y Seguridad.

![Java](https://img.shields.io/badge/Java-21-orange?logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-6DB33F?logo=springboot&logoColor=white)
![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-Microservices-brightgreen)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?logo=docker&logoColor=white)
![Keycloak](https://img.shields.io/badge/Keycloak-OAuth2-purple)

## Tecnologías

- **Backend:** Java 21, Spring Boot 3
- **Arquitectura:** Microservicios, Spring Cloud (Eureka, Config Server)
- **Base de datos:** PostgreSQL 16
- **Seguridad:** Keycloak (OAuth2 / OpenID Connect), JWT
- **Infraestructura:** Docker, Docker Compose
- **Herramientas:** Maven, Postman, IntelliJ IDEA

## Arquitectura del Sistema

El sistema está dividido en servicios independientes que se comunican a través de la red interna de Docker:

| **Servicio** | **Descripción** | **Puerto** |
|---|---|---|
| **ms-estudiante** | Gestión de la información de los estudiantes. | `8082` |
| **ms-curso** | Administración de cursos y créditos académicos. | `8081` |
| **ms-matricula** | Registro de matrículas (comunica estudiante y curso). | `8080` |
| **ms-eureka** | Servidor de descubrimiento de servicios. | `8761` |
| **config-server** | Servidor de configuración centralizada. | `8888` |
| **Keycloak** | Servidor de identidad y autorización (OIDC). | `8083` |
| **PostgreSQL** | Base de datos relacional compartida. | `5432` |

### Flujo de Seguridad (Keycloak + JWT)

1. El cliente (Postman/Frontend) solicita un token a Keycloak (`/token`).
2. Keycloak valida las credenciales y emite un JWT (Access Token).
3. El cliente envía el token en el header `Authorization: Bearer <token>` a los microservicios.
4. Spring Security en cada microservicio valida la firma criptográfica del token usando las llaves públicas de Keycloak.

## ¿Cómo ejecutar el proyecto?

### Requisitos previos

- Tener Docker Desktop instalado y corriendo.
- (Opcional) Tener Postman para probar las APIs.

### Pasos

1. Clona este repositorio:

   ```bash
   git clone https://github.com/adrivng/gestion-matriculas-microservices.git
   cd gestion-matriculas-microservices
   ```

2. Levanta toda la infraestructura con un solo comando:

   ```bash
   docker compose up -d --build
   ```

3. Espera unos 30-40 segundos a que todos los servicios se registren en Eureka.

## Prueba de la API (Postman)

Para consumir los endpoints protegidos, sigue estos pasos:

### 1. Obtener el Token en Keycloak

- **Método:** `POST`
- **URL:** `http://localhost:8083/realms/gestion-matriculas/protocol/openid-connect/token`
- **Body (x-www-form-urlencoded):**
  - `grant_type`: `password`
  - `client_id`: `ms-client`
  - `client_secret`: `ms-client-secret-123`
  - `username`: `testuser`
  - `password`: `123456`

### 2. Consumir un Endpoint (Ejemplo: Crear Curso)

- **Método:** `POST`
- **URL:** `http://localhost:8081/api/cursos`
- **Authorization:** Bearer Token (pega el `access_token` obtenido en el paso 1).
- **Body (raw JSON):**

  ```json
  {
    "nombre": "Matematicas Avanzadas",
    "codigo": "MAT-101",
    "creditos": 4
  }
  ```


## Autor

**Adrian Gabriel Cardenas Lamas**

Desarrollo de Sistemas de Información

[GitHub: @adrivng](https://github.com/adrivng)
