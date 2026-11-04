# Laboratorio: Despliegue de Gestor de Incidencias con Docker Compose

## 9. Entrega final
## Respuestas al cuestionario del laboratorio

### 1. ¿Por qué el frontend no necesita conectarse a PostgreSQL?
Porque el frontend es la interfaz de usuario (Nginx + HTML/JS) que se ejecuta en el cliente/navegador. Esta, no realiza consultas directas a la base de datos por seguridad y arquitectura, toda la manipulación y persistencia de datos se realiza a través de las peticiones HTTP que envía a la API del backend.

### 2. ¿Por qué el backend pertenece a dos redes?
Porque el backend actúa como intermediario entre la capa de presentación y la capa de datos. Pertenece a la red del `frontend` para recibir las peticiones HTTP de Nginx/cliente y a la red de los `datos` para comunicarse mediante SQL con PostgreSQL.

### 3. ¿Qué servicio necesita publicar un puerto en el anfitrión?
Únicamente el servicio `frontend`, que necesita publicar un puerto (`8080`), ya que es el único punto de entrada que es accesible para todos los usuarios finales desde el navegador web de la máquina anfitrion.

### 4. ¿Por qué publicar PostgreSQL hacia fuera no es la solución adecuada para un fallo de comunicación interno?
Publicar el puerto de PostgreSQL en el anfitrión no resuelve un problema de red dentro de Docker, e introduce un riesgo de seguridad al dejar la base de datos expuesta a redes externas no autorizadas. Los fallos de comunicación interna se deben resolver mediante la configuración de redes en Compose.

### 5. ¿Qué ventaja ofrece Docker Compose respecto a ejecutar los tres contenedores manualmente?
Se puede definir toda la arquitectura en un solo archivo `compose.yaml`. Así no hace falta ejecutar múltiples comandos `docker run`, `docker network` y `docker volume` a mano, facilitando el arranque con `docker compose up`.

### 6. ¿Por qué hemos dividido la arquitectura en dos redes?
Principalmente por seguridad. Al separar la red `frontend` de la red `datos`, nos aseguramos que si el frontend se ve comprometido, un atacante no pueda acceder directamente a la base de datos PostgreSQL, ya que no comparten segmento de red.

### 7. ¿Por qué el backend utiliza `db` como dirección de PostgreSQL?
Porque al asignar el nombre lógico de servicio `db` al contenedor de PostgreSQL, Compose resuelve automáticamente ese nombre a la IP interna del contenedor, evitando tener que gestionar direcciones IP dinámicas.

### 8. ¿Qué diferencia existe entre detener un proyecto y eliminar sus volúmenes?
- `docker compose stop`: Detiene la ejecución de los contenedores manteniendo sus capas, redes y volúmenes intactos.
- `docker compose down -v`: Elimina los contenedores, las redes del proyecto y también borra los volúmenes con nombre declarados (`postgres_data`), destruyendo permanentemente todos los datos almacenados en la base de datos.

### 9. ¿Qué consecuencias tendría para la empresa perder el volumen de PostgreSQL?
Implicaría la pérdida la información sobre las incidencias registradas en la empresa, causando interrupción del servicio, posibles pérdidas económicas y problemas operativos hasta restaurar una copia de seguridad.