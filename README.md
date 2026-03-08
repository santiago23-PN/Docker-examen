# Moodle + MariaDB (Docker Compose) - Actividad 2.03

Implementación de **Moodle** (plataforma de aprendizaje) usando **Docker Compose** con base de datos **MariaDB**.



## 📁 Estructura del proyecto
```text
docker-examen/
├── docker-compose.yml     # Archivo principal de configuración
├── Dockerfile             # Receta para construir la imagen de Moodle
└── README.md              # Documentación del proyecto
```
## ⚙️ 1. Configuración del archivo `docker-compose.yml`

Copia y pega lo siguiente dentro de tu archivo `docker-compose.yml`:

```yaml
version: '3.8'

services:
  moodleapp:
    image: moodleapp:1.0
    container_name: moodleapp
    ports:
      - "80:80"
    volumes:
      - moodledata:/var/www/moodledata
    depends_on:
      - moodledb
    networks:
      - moodle-net

  moodledb:
    image: mariadb:10.11
    container_name: moodledb
    environment:
      MYSQL_ROOT_PASSWORD: mysql-rootpass
      MYSQL_DATABASE: moodlelms
      MYSQL_USER: moodle-admin
      MYSQL_PASSWORD: moodle-dbpass
    volumes:
      - clouddbdata:/var/lib/mysql
    networks:
      - moodle-net

  phpmyadmin:
    image: phpmyadmin
    container_name: phpmyadmin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: moodledb
      PMA_USER: moodle-admin
      PMA_PASSWORD: moodle-dbpass
    depends_on:
      - moodledb
    networks:
      - moodle-net

volumes:
  clouddbdata:
  moodledata:

networks:
  moodle-net:
```

## 🚀 2. Levantar el entorno

Ejecuta los siguientes comandos desde tu terminal para descargar y levantar el proyecto:

```bash
git clone https://github.com/santiago23-PN/Docker-examen.git
cd Docker-examen
docker compose up -d
docker ps
```
Detener los contenedores:
```bash
docker compose stop
```
Iniciar los contenedores (si ya fueron creados):
```bash
docker compose start
```
## 🌐 Acceso a la plataforma

Cuando ambos contenedores estén activos, abre en tu navegador:

* **Moodle:** `http://localhost:80` (o simplemente `http://localhost`)
* **phpMyAdmin:** `http://localhost:8081`
* o http://<IP_del_servidor>

---

## 🛠️ 3. Configuración de la Base de Datos en Moodle
Cuando el asistente de Moodle te solicite los "Ajustes de base de datos", utiliza los siguientes valores exactos:

* **host de la Base de Datos:** `moodledb`
* **Nombre de la base de datos:** `moodlelms`
* **Usuario de la base de datos:** `moodle-admin`
* **Contraseña de la base de datos:** `moodle-dbpass`
* **Prefijo de tablas:** `mdl_`
---

## 🎓 4. Cuenta de Administrador
* Usuario: **admin**
* Contraseña: **S45yuk#56**
---
## 🎓 4. Configuración inicial y acceso a Moodle

Para comenzar a gestionar tu plataforma, primero debes iniciar sesión con tu cuenta de administrador.

1. Abre `http://localhost` en tu navegador.
2. Haz clic en el botón **"Acceder"** (Login) en la esquina superior derecha.
3. Ingresa tu usuario y contraseña de administrador que definiste durante la instalación.

![Pantalla de inicio de sesión](imagenes/login-moodle.png)

Una vez dentro, verás el **Panel de control** (Dashboard), lo que indica que tienes acceso total para configurar cursos y usuarios.

![Panel de control principal](imagenes/dashboard-moodle.png)

### 1) Creación de un curso
Para crear un curso, dirígete a la opción **“Mis cursos”** y selecciona **“Create course”**.

![Acceso a Mis cursos](imagenes/figura-7.png)

Completa el formulario con los datos necesarios (nombre, fechas, etc.) y selecciona **“Guardar cambios y mostrar”**.

![Formulario de curso](imagenes/figura-8.png)

Al finalizar, verás la confirmación de que el curso se creó correctamente en la plataforma.

![Curso creado](imagenes/figura-9.png)

---

## 👥 5. Gestión de usuarios y roles

### Creación de usuarios
Para registrar un nuevo usuario, ve a la pestaña **“Administración del sitio”**, luego a la opción **“Usuarios”** y haz clic en **“Añadir un nuevo usuario”**.

![Administración de usuarios](imagenes/figura-10.png)

Completa el formulario con la información requerida y selecciona **“Crear usuario”**.

![Formulario de usuario](imagenes/figura-11.png)

Podrás visualizar la lista de todos los usuarios registrados en la plataforma.

![Lista de usuarios](imagenes/figura-12.png)

### Asignación de roles e inscripción
Para inscribir usuarios y asignarles roles (estudiante, profesor, etc.):

1. **Inscribir usuarios:** Ve a **“Mis cursos”**, selecciona el curso, entra a **“Participantes”** y haz clic en **“Inscribir usuarios”**.
2. **Asignar roles:** En la lista de participantes, haz clic en el ícono del lápiz junto al usuario para seleccionar su rol y guarda los cambios.

![Inscripción de usuarios](imagenes/figura-14.png)
![Visualización de participantes](imagenes/figura-15.png)
![Asignación de roles](imagenes/figura-13.png)

---


