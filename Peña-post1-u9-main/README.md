# Unidad 9 — Seguridad en Aplicaciones Web

Sistema de autenticación con **Spring Security 6**, registro de usuarios con
contraseñas hasheadas con `BCryptPasswordEncoder`, login basado en formulario
con `UserDetailsService` que consulta MySQL y autorización diferenciada por
roles `ADMIN` y `USER`.

---

## 1. Requisitos previos

| Herramienta            | Versión recomendada |
|------------------------|---------------------|
| Java JDK               | 17 o superior       |
| Maven                  | 3.8+ (opcional, puedes usar el wrapper) |
| MySQL Server           | 8.x                 |
| Visual Studio Code     | última              |

### Extensiones de VS Code (instalar antes de abrir el proyecto)

1. **Extension Pack for Java** (Microsoft) — incluye `Language Support for Java`, `Debugger`, `Test Runner`, `Maven`, `Project Manager`.
2. **Spring Boot Extension Pack** (VMware) — incluye `Spring Boot Tools`, `Spring Initializr Java Support`, `Spring Boot Dashboard`.

---

## 2. Configurar MySQL

Abre tu cliente MySQL y ejecuta:

```sql
CREATE DATABASE IF NOT EXISTS estudiantes_db
  CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER IF NOT EXISTS 'appuser'@'localhost' IDENTIFIED BY 'apppass';
GRANT ALL PRIVILEGES ON estudiantes_db.* TO 'appuser'@'localhost';
FLUSH PRIVILEGES;
```

> Si tus credenciales son distintas, edita `src/main/resources/application.properties`.

---

## 3. Abrir el proyecto en VS Code

## 4. Ejecutar la aplicación

La aplicación queda disponible en **http://localhost:8080**.

---

## 5. Usuarios de prueba

| Rol   | Email                        | Contraseña |
|-------|------------------------------|------------|
| ADMIN | `admin@universidad.edu`      | `admin123` |
| USER  | *(el que registres tú)*      | *(la que elijas)* |

---

## 6. Estructura del proyecto

```
seguridad-app/
├── pom.xml
├── insertar_admin.sql
├── README.md
└── src/
    ├── main/
    │   ├── java/com/universidad/seguridad/
    │   │   ├── SeguridadApplication.java
    │   │   ├── config/
    │   │   │   └── SecurityConfig.java
    │   │   ├── controller/
    │   │   │   └── AuthController.java
    │   │   ├── model/
    │   │   │   └── Usuario.java
    │   │   ├── repository/
    │   │   │   └── UsuarioRepository.java
    │   │   └── service/
    │   │       ├── UsuarioService.java
    │   │       └── UsuarioDetailsService.java
    │   └── resources/
    │       ├── application.properties
    │       ├── static/css/styles.css
    │       └── templates/
    │           ├── dashboard.html
    │           ├── admin/panel.html
    │           └── auth/
    │               ├── login.html
    │               └── registro.html
    └── test/java/com/universidad/seguridad/
        └── GenerarHashTest.java
```

---

## 7. Solución de problemas

| Problema | Causa probable | Solución |
|----------|----------------|----------|
| `Access denied for user 'appuser'` | Credenciales MySQL incorrectas | Ajusta `application.properties` |
| `Communications link failure` | MySQL no está corriendo | Inicia el servicio MySQL |
| `Whitelabel Error Page` al iniciar | Tabla `usuarios` aún no existe | Reinicia la app, Hibernate la crea |
| Login da `error=true` siempre | Hash BCrypt mal copiado | Re-genera con `GenerarHashTest` |
| 403 al entrar como ADMIN | Rol guardado sin prefijo `ROLE_` | Verifica que en BD diga `ROLE_ADMIN` |

---

## 8. Capturas (✅ INCLUIDAS en `capturas/`)

Todas las capturas obligatorias de la rúbrica ya están en la carpeta `capturas/`.
Ver `EVIDENCIAS.md` para el detalle de cada una.

| # | Archivo | Demuestra |
|---|---------|-----------|
| 1 | `01_login_personalizado.png` | Formulario de login propio |
| 2 | `02_dashboard_user.png` | Dashboard como USER |
| 3 | `03_error_403_forbidden.png` | USER bloqueado en `/admin` |
| 4 | `04_hash_bcrypt_mysql.png` | Contraseña hasheada con BCrypt en MySQL |
| 5 | `05_panel_admin.png` | ADMIN accede y ve la lista de usuarios |
| 6 | `06_dashboard_admin.png` | *(bonus)* Dashboard ADMIN con botón extra |

