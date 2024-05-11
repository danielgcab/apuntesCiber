## ¿Qué es y cómo funciona?

WordPress es un sistema de gestión de contenidos (CMS, por sus siglas en inglés) de código abierto que se utiliza para crear y gestionar sitios web y blogs. Es una de las plataformas de CMS más populares en el mundo, y se utiliza para todo tipo de sitios web, desde blogs personales hasta sitios web empresariales y tiendas en línea.

WordPress funciona utilizando una combinación de un software de servidor web, una base de datos y un conjunto de archivos de software que se descargan e instalan en un servidor web. Los usuarios pueden descargar e instalar el software de WordPress de forma gratuita desde el sitio web oficial de WordPress, o a través de un proveedor de alojamiento web que ofrezca WordPress como parte de sus servicios.

Una vez que se ha instalado el software de WordPress en un servidor web, los usuarios pueden empezar a crear y gestionar su sitio web utilizando la interfaz de usuario de WordPress. Esto incluye la creación de páginas y publicaciones, la personalización de la apariencia y la funcionalidad del sitio web a través de temas y plugins, la gestión de usuarios y permisos, la creación de formularios de contacto y mucho más.

La funcionalidad de WordPress se puede extender mediante el uso de plugins, que son piezas de software que se agregan al núcleo de WordPress para proporcionar funcionalidades adicionales. Los usuarios pueden descargar e instalar plugins desde el directorio de plugins de WordPress o desde otros sitios web de terceros.

WordPress es altamente personalizable y escalable, lo que lo hace adecuado para sitios web de cualquier tamaño y complejidad. Además, es fácil de usar y cuenta con una amplia comunidad de desarrolladores y usuarios que contribuyen con la mejora y el mantenimiento del software.

## Enumeración manual

Comprobar los siguientes puntos a la hora de enfrentarnos a un Wordpress -->

- index.php

- Revisar posibles rutas del login

    - /wp-admin/login.php

    - /wp-admin/wp-login.php

    - /login.php

    - /wp-login.php


- En /wp-content es la ruta principal donde estan los /plugins y /themes, cuando estás enumerando plugins o temas cuando pones X plugin y la pantalla sale blanco significa que existe, sino saldría un código de respuesta 404.

- En /wp-includes están los certificados, fuentes, java script y widgets.

- Comprobar si tenemos acceso al archivo xmlrpc.php, si es así consulta el siguiente [artículo](https://nitesculucian.github.io/2019/07/01/exploiting-the-xmlrpc-php-on-all-wordpress-versions/).

- Comprobar si hay expuesto el archivo /wp-config.php.txt

- Si nos encontramos que la web no se ve bien, es posible que la web este apuntando a un virtual hosting routing, cosa que podemos ver si vemos el código fuente de la web, si es así tenemos que añadir en el /etc/hosts de nuestra máquina apuntando a la IP + dominio


**Ejemplo**

```
192.168.138.132    site.example
```

### Enumerar plugins

```
curl -s -X GET "http://url/" | grep -oP 'plugins/\K[^/]+' | sort -u
```

## Herramientas automatizadas

### Wpscan

```
# Comando básico
wpscan --url <URL> --enumerate <ap,at,tt,cb,dbe,u,m>
# Parametros importantes
vp   Vulnerable plugins
ap   All plugins
p    Popular plugins
vt   Vulnerable themes
at   All themes
t    Popular themes
tt   Timthumbs
cb   Config backups
dbe  Db exports
u    User IDs range. e.g: u1-5
    Range separator to use: '-'
    Value if no argument supplied: 1-10
m    Media IDs range. e.g m1-15
    Note: Permalink setting must be set to "Plain" for those to be detected
    Range separator to use: '-'
    Value if no argument supplied: 1-100
vt -> vulnerables themes
--password-attack
```

### Fuerza bruta

Creamos el archivo users.txt y ponemos dentro los usuarios enumerados.

```
pepito
ejemplo
tjeck
```

```
wpscan -U users.txt -P /usr/share/wordlists/rockyou.txt --url http://IP
```

### Wpseku


```
python3 wpseku.py -url <URL> # instalar --> https://github.com/andripwn/WPSeku
```

## Post-explotación

Revisar archivo **wp-config** en la carpeta **/var/www/html/wordpress** ya que suele tener las passwords del sistema o del Wordpress y puede ayudar para escalar privilegios.