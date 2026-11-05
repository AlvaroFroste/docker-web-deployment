

# Proyecto Docker - WordPress

## 1. Descripción y objetivo

Este proyecto consiste en crear una página web utilizando Wordpress y MariaDB como base de datos

El objetivo es como instalar en dos contenedores distintos por un lado Wordpress y por el otro MariaDB que es donde se guardan los datos de la página
## 2. Tecnologías utilizadas

-Docker.
-Docker Compose.
-WordPress.
-MariaDB.
-Git y GitHub.
-Ubuntu (WSL 2).
-Windows 11.

## 3. Requisitos previos

-Windows 11.
-Docker Desktop.
-WSL 2 con Ubuntu.
-Git.
-GitHub.
-Conexión a internet (al no ser una máquina virtual se usará la red real de mi ordenador)

## 4. Estructura del proyecto

```text
docker-web-deployment/
│
├── docker-compose.yml    # Configuración de los contenedores
├── .env                  # Datos y contraseñas
├── .env.example          # Ejemplo de configuración
├── .gitignore            # Archivos que no se subirán a GitHub
├── README.md             # Información del proyecto
│
├── database/
│   └── README.md         # Información de la base de datos
│
└── backups/
    └── README.md         # Información de las copias de seguridad
```

## 5. Configuración del archivo .env

El .env contiene los datos necesarios para que el WordPress pueda conectarse a la base de datos. Dentro se configuran por ejemplo las contraseñas y el puerto que se usará (8080)
El fichero .env.example sirve como plantilla para rellenar el fichero .env

## 6. Iniciar el proyecto

Primero, abrir Ubuntu y entrar en la carpeta del proyecto que habremos creado previamente:

mkdir -p /proyectos/docker-web-deployment
cd /proyectos/docker-web-deployment

Para escargar las imágenes:

docker compose pull

Para ejecutar los 2 contenedores:

docker compose up -d


Para comprobar que funcionan correctamente:

docker compose ps


## 7. Acceder a la página web

Abrir el navegador  y entrar en el siguiente enlace. Ojo,la primera vez aparecerá la instalación de WordPress, donde habrá que configurar el idioma, crear el usuario admin y su contraseña. Como en cualquier instalacion nueva de wordpress

http://localhost:8080


## 8. Cómo detener y reiniciar los servicios

Para detener los contenedores:

docker compose stop

Para volver a iniciarlos:


docker compose start

Para detenerlos y eliminarlos:

docker compose down


## 9. Cómo consultar los registros

Para ver los logs de los contenedores:

docker compose logs

Para ver solamente los logs de WordPress:

docker compose logs wordpress

Para ver solamente los de MariaDB:

docker compose logs db

## 10. Backups y restauración

Para guardar un backup de la base de datos:

docker compose exec -T db sh -c 'mariadb-dump -u"$MARIADB_USER" -p"$MARIADB_PASSWORD" "$MARIADB_DATABASE"' > backups/wordpress.sql

Esto crea un archivo llamado wordpress.sql dentro del directorio backups.
Para comprobar que se ha creado y a su vez tener mas información del mismo como cuanto espacio ocupa se usa el comando:

ls -lh backups/wordpress.sql

Para recuperar los datos de dicho backup:

docker compose exec -T db sh -c 'mariadb -u"$MARIADB_USER" -p"$MARIADB_PASSWORD" "$MARIADB_DATABASE"'
