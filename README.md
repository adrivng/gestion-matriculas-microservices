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

| Servicio | Descripción | Puerto |
|----------|-------------|--------|
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

