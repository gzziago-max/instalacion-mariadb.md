# instalacion-mariadb.md
# Instalación y configuración de MariaDB en Ubuntu Server

## 1. Actualizar Ubuntu Server

Primero actualizamos los repositorios.

```bash
sudo apt update
```

Actualizamos los paquetes instalados.

```bash
sudo apt upgrade -y
```

## 2. Instalar MariaDB

Instalamos el servidor de MariaDB.

```bash
sudo apt install mariadb-server -y
```

Comprobamos que se ha instalado correctamente.

```bash
mariadb --version
```

## 3. Iniciar MariaDB

Iniciamos el servicio.

```bash
sudo systemctl start mariadb
```

Hacemos que se inicie automáticamente al encender Ubuntu.

```bash
sudo systemctl enable mariadb
```

Comprobamos su estado.

```bash
sudo systemctl status mariadb
```

Tiene que aparecer active (running).

Para salir de la pantalla pulsamos Q.

## 4. Configurar la seguridad

Ejecutamos:

```bash
sudo mariadb-secure-installation
```

Seguimos las preguntas que aparecen.

- Contraseña actual de root: pulsamos Enter si utiliza autenticación por socket y no tiene contraseña.
- Cambiar a autenticación unix_socket: podemos mantenerla si ya está configurada.
- Cambiar contraseña de root: no es necesario si usamos autenticación por socket.
- Eliminar usuarios anónimos: Y.
- Desactivar acceso remoto de root: Y.
- Eliminar base de datos test: Y.
- Recargar privilegios: Y.

Las preguntas pueden variar según la versión.

## 5. Entrar en MariaDB

Para entrar ejecutamos:

```bash
sudo mariadb
```

Cuando aparezca:

```text
MariaDB [(none)]>
```

Significa que estamos dentro.

## 6. Ver las bases de datos

```sql
SHOW DATABASES;
```

Nos muestra las bases de datos existentes.

## 7. Crear una base de datos

Creamos una base de datos llamada instituto.

```sql
CREATE DATABASE instituto;
```

Comprobamos que se ha creado.

```sql
SHOW DATABASES;
```

## 8. Seleccionar una base de datos

```sql
USE instituto;
```

A partir de ahora trabajaremos dentro de esta base de datos.

## 9. Crear una tabla

Creamos una tabla llamada alumnos.

```sql
CREATE TABLE alumnos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50),
    apellido VARCHAR(50),
    edad INT
);
```

Comprobamos las tablas existentes.

```sql
SHOW TABLES;
```

Para ver la estructura de la tabla:

```sql
DESCRIBE alumnos;
```

## 10. Insertar datos en la tabla

Añadimos un alumno.

```sql
INSERT INTO alumnos (nombre, apellido, edad)
VALUES ('Juan', 'Garcia', 18);
```

Añadimos varios alumnos.

```sql
INSERT INTO alumnos (nombre, apellido, edad)
VALUES
('Pedro', 'Lopez', 19),
('Maria', 'Perez', 17),
('Carlos', 'Sanchez', 20);
```

## 11. Consultar los datos

Para ver todos los alumnos:

```sql
SELECT * FROM alumnos;
```

Para ver solamente los nombres:

```sql
SELECT nombre FROM alumnos;
```

Para buscar alumnos mayores de 18 años:

```sql
SELECT * FROM alumnos WHERE edad > 18;
```

Para ordenar por edad:

```sql
SELECT * FROM alumnos ORDER BY edad ASC;
```

## 12. Modificar datos

Cambiamos la edad del alumno con ID 1.

```sql
UPDATE alumnos
SET edad = 20
WHERE id = 1;
```

Comprobamos el cambio.

```sql
SELECT * FROM alumnos;
```

## 13. Eliminar datos

Eliminamos un alumno.

```sql
DELETE FROM alumnos WHERE id = 1;
```

Comprobamos que se ha eliminado.

```sql
SELECT * FROM alumnos;
```

Importante: utilizar WHERE para no borrar todos los registros.

## 14. Crear un usuario en MariaDB

Creamos un usuario llamado usuario1.

```sql
CREATE USER 'usuario1'@'localhost'
IDENTIFIED BY 'ClaveSegura123!';
```

Le damos permisos sobre la base de datos instituto.

```sql
GRANT ALL PRIVILEGES ON instituto.* TO 'usuario1'@'localhost';
```

Comprobamos los permisos.

```sql
SHOW GRANTS FOR 'usuario1'@'localhost';
```

## 15. Salir de MariaDB

```sql
EXIT;
```

## 16. Entrar con el usuario creado

Desde la terminal de Ubuntu ejecutamos:

```bash
mariadb -u usuario1 -p
```

Introducimos la contraseña.

Seleccionamos la base de datos.

```sql
USE instituto;
```

Comprobamos que podemos consultar los datos.

```sql
SELECT * FROM alumnos;
```

## 17. Cambiar la contraseña de un usuario

Entramos como administrador:

```bash
sudo mariadb
```

Cambiamos la contraseña:

```sql
ALTER USER 'usuario1'@'localhost'
IDENTIFIED BY 'NuevaClave123!';
```

## 18. Eliminar una tabla

```sql
DROP TABLE alumnos;
```

Este comando elimina la tabla y todos sus datos.

## 19. Eliminar una base de datos

```sql
DROP DATABASE instituto;
```

Cuidado porque elimina toda la base de datos y sus tablas.

## 20. Hacer una copia de seguridad

Salimos de MariaDB:

```sql
EXIT;
```

Desde la terminal de Ubuntu ejecutamos:

```bash
sudo mariadb-dump instituto > copia_instituto.sql
```

Esto crea una copia de seguridad de la base de datos.

## 21. Restaurar una copia de seguridad

Si la base de datos no existe, primero la creamos.

```bash
sudo mariadb -e "CREATE DATABASE instituto;"
```

Restauramos la copia:

```bash
sudo mariadb instituto < copia_instituto.sql
```

Si la base de datos ya existe, no hace falta volver a crearla.

## 22. Reiniciar MariaDB

```bash
sudo systemctl restart mariadb
```

Para detener el servicio:

```bash
sudo systemctl stop mariadb
```

Para volver a iniciarlo:

```bash
sudo systemctl start mariadb
```

## 23. Instalar PHP para conectar con MariaDB

Si necesitamos utilizar MariaDB con Apache y PHP:

```bash
sudo apt install php php-mysql libapache2-mod-php -y
```

Reiniciamos Apache.

```bash
sudo systemctl restart apache2
```

Comprobamos la versión de PHP.

```bash
php -v
```

## 24. Instalar phpMyAdmin (opcional)

Si el examen pide administrar MariaDB desde el navegador:

```bash
sudo apt install phpmyadmin -y
```

Durante la instalación seleccionamos apache2 si aparece la opción y seguimos el asistente de configuración.

Si phpMyAdmin queda correctamente configurado, podemos acceder desde el navegador:

```text
http://IP_DEL_SERVIDOR/phpmyadmin
```

Entramos con el usuario y contraseña de MariaDB.

Si la página no aparece, tendremos que revisar la integración de phpMyAdmin con Apache.

---

# Resumen de comandos importantes

| Comando | Función |
|---|---|
| sudo apt install mariadb-server -y | Instalar MariaDB |
| sudo systemctl status mariadb | Ver estado |
| sudo mariadb | Entrar como administrador |
| SHOW DATABASES; | Ver bases de datos |
| CREATE DATABASE nombre; | Crear base de datos |
| USE nombre; | Seleccionar base de datos |
| SHOW TABLES; | Ver tablas |
| DESCRIBE tabla; | Ver estructura |
| SELECT * FROM tabla; | Consultar datos |
| INSERT INTO | Insertar datos |
| UPDATE | Modificar datos |
| DELETE FROM | Eliminar registros |
| CREATE USER | Crear usuario |
| GRANT | Dar permisos |
| DROP TABLE | Eliminar tabla |
| DROP DATABASE | Eliminar base de datos |
| EXIT; | Salir de MariaDB |

**Importante:** los comandos que empiezan por `sudo` se ejecutan en la terminal de Ubuntu. Los comandos SQL se ejecutan dentro de MariaDB y normalmente terminan en punto y coma.
