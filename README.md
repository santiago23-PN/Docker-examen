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
