# Iam-Microservice

Este proyecto es un microservicio de Gestión de Identidad y Acceso (IAM) construido con Spring Boot. Proporciona funcionalidades de registro y autenticación de usuarios, utilizando JSON Web Tokens (JWT) para asegurar los endpoints.

## Funcionalidad

El microservicio expone dos endpoints principales para la gestión de usuarios:

-   `POST /auth/register`: Permite a los nuevos usuarios registrarse proporcionando un nombre de usuario, correo electrónico y contraseña.
-   `POST /auth/login`: Autentica a los usuarios existentes y, si las credenciales son correctas, devuelve un JWT.

### Autenticación Basada en Token (JWT)

Una vez que un usuario se autentica correctamente, el servicio genera un JWT. Este token contiene información (claims) sobre el usuario, como su nombre de usuario y los roles que tiene asignados (por ejemplo, `ROLE_USER`, `ROLE_ADMIN`).

El token debe ser incluido en la cabecera `Authorization` de las solicitudes a los endpoints protegidos, de la siguiente manera:

`Authorization: Bearer <token>`

El microservicio valida este token en cada solicitud para asegurar que el usuario tiene los permisos necesarios para acceder al recurso.

### Roles de Usuario

El sistema define los siguientes roles:

-   `ROLE_USER`: Rol por defecto para los usuarios registrados.
-   `ROLE_ADMIN`: Rol para los administradores del sistema, con permisos elevados.

Cuando se genera un token, los roles del usuario se incluyen en los claims, permitiendo una autorización basada en roles en otros microservicios que consuman este servicio de IAM.

## Endpoints de la API

-   `POST /auth/register`
    -   **Descripción**: Registra un nuevo usuario.
    -   **Request Body**:
        ```json
        {
          "username": "testuser",
          "email": "test@example.com",
          "password": "password123"
        }
        ```
-   `POST /auth/login`
    -   **Descripción**: Autentica un usuario y devuelve un JWT.
    -   **Request Body**:
        ```json
        {
          "username": "testuser",
          "password": "password123"
        }
        ```
    -   **Success Response**:
        ```json
        {
          "token": "ey..."
        }
        ```

## Configuración de Seguridad

La configuración de seguridad permite el acceso público a los endpoints bajo `/auth/**`. Cualquier otra ruta requiere que la solicitud esté autenticada con un JWT válido.

## Cómo Ejecutar el Proyecto

1.  **Base de Datos**: Asegúrate de tener una instancia de MySQL en ejecución.
2.  **Configuración**: Actualiza el archivo `src/main/resources/application.properties` con las credenciales de tu base de datos.
3.  **Ejecutar**: Inicia la aplicación usando tu IDE o a través de la línea de comandos con Maven:
    ```bash
    mvn spring-boot:run
    ```
La aplicación se ejecutará en el puerto `8081` por defecto.
