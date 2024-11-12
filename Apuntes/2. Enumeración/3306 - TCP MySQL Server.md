[[Apuntes]]
MySQL es un sistema de gestión de bases de datos relacional desarrollado bajo licencia dual: Licencia pública general/Licencia comercial por Oracle Corporation

## Nmap scripts


```
ls -al /usr/share/nmap/scripts/ | grep -e “mysql” # para ver los scripts posibles del mysql
nmap --script=mysql-* target-1 # lanzar todos los scripts
mysql-enum
mysql-brute
mysql-info
Esto puede ser a veces muy ruidoso y bloquear el puerto, así que mejor otras formas mejores.
```

## Conectarnos

```
mysql -h <IP> -u <USER> -p <PASS>
```

### Comandos básicos

|Comando|Descripción|
|---|---|
|`SHOW DATABASES;`|Muestra todas las bases de datos disponibles en el servidor MySQL.|
|`USE database_name;`|Selecciona la base de datos especificada para su uso en la conexión actual.|
|`SHOW TABLES;`|Muestra todas las tablas disponibles en la base de datos seleccionada.|
|`DESCRIBE table_name;`|Muestra la estructura de la tabla especificada, incluyendo nombres y tipos de columna.|
|`SELECT * FROM table_name;`|Recupera todos los registros de la tabla especificada.|
|`INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...);`|Inserta un nuevo registro en la tabla especificada, proporcionando valores para cada columna.|
|`UPDATE table_name SET column1 = value1, column2 = value2, ... WHERE condition;`|Actualiza uno o más registros en la tabla especificada, estableciendo nuevos valores para cada columna. Puedes especificar una condición que determine qué registros actualizar.|
|`DELETE FROM table_name WHERE condition;`|Elimina uno o más registros de la tabla especificada, basándose en una condición que determine qué registros eliminar.|
|`QUIT`|Cierra la conexión Telnet con el servidor MySQL.|

Artículo más extenso sobre comandos: [https://phoenixnap.com/kb/mysql-commands-cheat-sheet](https://phoenixnap.com/kb/mysql-commands-cheat-sheet)

## Fuerza bruta

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt mysql
``````