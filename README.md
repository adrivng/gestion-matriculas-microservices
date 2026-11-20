
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

## Capturas del Proyecto

*(Aquí debes subir tus capturas a una carpeta llamada `/docs` o `/assets` en tu repo y enlazarlas usando la ruta relativa).*

**1. Descubrimiento de Servicios (Eureka Server)**

<img width="886" height="634" alt="image" src="https://github.com/user-attachments/assets/1080afc5-3c35-48c5-961d-1dbc9c4bb0e3" />

**2. Configuración de Seguridad (Keycloak)**

<img width="886" height="444" alt="image" src="https://github.com/user-attachments/assets/be4abf63-9435-414f-b7c0-a54c18e111cd" />

<img width="886" height="481" alt="image" src="https://github.com/user-attachments/assets/841bc0ae-ae67-4919-97b5-b86612961c02" />

<img width="1920" height="927" alt="screencapture-localhost-8083-admin-master-console-2026-11-20-17_10_50" src="https://github.com/user-attachments/assets/bd1380dd-cb3c-46c7-9738-b70d199c71d0" />

<img width="886" height="418" alt="image" src="https://github.com/user-attachments/assets/43e7d015-9587-4472-a5c9-114b1fffe167" />

**3. Prueba de Endpoints Protegidos (Postman)**

<img width="886" height="640" alt="image" src="https://github.com/user-attachments/assets/bfa5bc0d-e391-41a7-a5ff-3e1ea36bff46" />

<img width="886" height="623" alt="image" src="https://github.com/user-attachments/assets/bd521537-a46e-4893-b14a-ca346ec53aa8" />

<img width="886" height="620" alt="image" src="https://github.com/user-attachments/assets/9d4914ba-bdd4-4048-b96c-41299e82e4d9" />

**4. Esquema Tablas (PostgreSQL)**

<img width="886" height="498" alt="image" src="https://github.com/user-attachments/assets/ede3c954-d3be-4226-ba0a-10d58ef68f2f" />


**5. Despliegue de Contenedores (Docker)**

<img width="886" height="470" alt="image" src="https://github.com/user-attachments/assets/2e281210-4844-4370-82ac-f272b4d6d662" />


## Autor

**Adrian Gabriel Cardenas Lamas**

Desarrollo de Sistemas de Información

[GitHub: @adrivng](https://github.com/adrivng)
