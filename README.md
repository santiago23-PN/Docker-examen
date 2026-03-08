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

## 🛠️ 3. Configuración de la Base de Datos en Moodle
Cuando el asistente de Moodle te solicite los "Ajustes de base de datos", utiliza los siguientes valores exactos:

* **host de la Base de Datos:** `moodledb`
* **Nombre de la base de datos:** `moodlelms`
* **Usuario de la base de datos:** `moodle-admin`
* **Contraseña de la base de datos:** `moodle-dbpass`
* **Prefijo de tablas:** `mdl_`
