# 🛡️ Microservices Auth Auronix

**Enterprise Multi-Tenant Authorization Server & Identity Broker**

Este microservicio es el núcleo central de autenticación y autorización (IAM) del ecosistema Auronix. Diseñado con una Arquitectura Hexagonal y principios de *Zero Trust*, soporta concurrencia masiva, múltiples flujos de autenticación y aislamiento estricto de datos por inquilino (Multi-Tenant RLS).

© 2026 Auronix / Jhonny Miñan. Todos los derechos reservados. Código Propietario y Cerrado.

---

## 🏗️ Stack Tecnológico Base

* **Lenguaje:** Java 21 LTS (Virtual Threads / Project Loom)
* **Framework:** Spring Boot 4.0.5
* **Seguridad:** Spring Security 7.x + Spring Authorization Server (OAuth2.1 / OIDC)
* **Persistencia:** PostgreSQL (con Row-Level Security por Tenant) + Hibernate
* **Caché & Rate Limiting:** Redis + Bucket4j
* **Eventos & Auditoría:** Apache Kafka
* **Criptografía:** BouncyCastle (Soporte X.509 y ECDSA)
* **Compilación:** GraalVM Native Image
* **Observabilidad:** OpenTelemetry (Micrometer Tracing)

## ⚙️ Arquitectura Hexagonal

El proyecto está diseñado bajo una estricta separación de responsabilidades (Ports and Adapters):
* `identity-core`: Reglas de negocio puras, entidades y validación.
* `auth-orchestrator`: Interfaz única de entrada (`/auth/login`) resolviendo estrategias dinámicas (Strategy Pattern).
* `oauth2-server`: Emisión y validación de JSON Web Tokens (JWS/JWE).
* `risk-engine`: Evaluación asíncrona de anomalías y control de acceso.

## 🔐 Características de Seguridad Críticas

1. **Aislamiento Multi-Tenant (RLS):** Garantía a nivel de motor de base de datos de que ningún cliente puede acceder a los datos de otro.
2. **Criptografía Avanzada:**
   * Algoritmo de Hashing: **Argon2id** (Resistencia contra ataques por GPU).
   * Firmas de JWT: **RS256 / ES256**.
   * Validación de Firmas Electrónicas Locales (Truststore Propio).
3. **Defensa Activa:**
   * Limitación de peticiones (Rate Limiting) por IP, Usuario y Tenant utilizando Redis.
   * Bloqueo progresivo exponencial ante ataques de fuerza bruta.
4. **Tokens Condicionales:** Personalización de *claims* y *Refresh Tokens* emitidos únicamente según las políticas de seguridad del Tenant y el nivel de confianza del cliente.

## 🚀 Despliegue y Compilación

Este proyecto está preparado para ejecutarse en contenedores Docker y orquestarse en Kubernetes, optimizado para compilación nativa (GraalVM).

### Pre-requisitos
* Java 21 (GraalVM CE)
* Maven 3.9+
* Docker & Docker Compose (Para levantar PostgreSQL, Redis y Kafka localmente)

### Compilación JVM Tradicional
```bash
mvn clean install -DskipTests