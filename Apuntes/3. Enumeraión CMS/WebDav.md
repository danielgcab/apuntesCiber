[[Apuntes]]
## ¿Qué es y cómo funciona?

WebDAV es un protocolo que nos permite **guardar archivos, editarlos, moverlos y compartirlos** en un servidor web, no necesitaremos utilizar otros protocolos de intercambio de archivos en red local o Internet, como Samba, FTP o NFS.

WebDav se suele encontrar en distintas tecnologias:

- **Microsoft IIS:** el cual cuenta con un módulo WebDAV propio.

- **Apache HTTP:** Cuenta con diferentes módulos basados en Linux davfs2, y la herramientas de control Apache Subversión.

- **SabreDAV:** Es una aplicación PHP que se puede instalar en Apache o NGINX, y funciona a modo de complemento en lugar de los módulos de estas.

- **Nestcloud:** Se trata de un servicio en la nube que cuenta con soporte para WebDAV.

- **ownCloud:** Otro servicio en la nube, donde cuenta con un soporte completo de WebDAV.


### Puntos importantes

Normalmente necesitas credenciales validas para poder tener acceso al WebDav, suele ser un panel de autentificación básico de HTTP.

El ataque básico que se tendría que hacer es subir una webshell y ejecutar comandos desde la webshell para enviarnos una shell a nuestra máquina para comprometer el servidor.

- Sube archivos con extensiones ejecutables {.exe,.asp,.aspx etc} (tal vez no esté prohibido).

- Cargue archivos sin extensiones ejecutables (como .txt) e intente cambiar el nombre del archivo (MOVE) con una extensión ejecutable.

- Cargue archivos sin extensiones ejecutables (como .txt) e intente copiar el archivo (MOVER) con extensión ejecutable.

## DevTest

Esta herramienta prueba a subir archivos que tiene por defecto la propia con diferentes extensiones para testear si se suben y se ejcutan.

```
devtest -move -sendbd auto -url http://<IP> #Sube archivos .txt y los cambia a otras extensiones
```

Si tienes credenciales básica añade `-auth user:password` al comando.

Ejemplo de DevTest:

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FaNnQB2rdBWjmn3PctBbK%252Fimagen.png%3Falt%3Dmedia%26token%3D6d7cd227-e3f0-4e96-8086-2e3a7d415029&width=768&dpr=4&quality=100&sign=3e500083f5f86f440282b2b033a411439ad4385465c1da699aaa544b8090934e)

Output del comando

## Cadaver

Para conectarte al WebDav se puede utilizar esta herramienta y subir archivos (subirlos, moverlos o borrarlos).

Copy

```
cadaver <IP>
```

|Comando|Descripción|
|---|---|
|ls|Path|
|put local \| put|Subir archivo local al servidor|
|get remote|Descargar archivo remote|
[WebDAV client for Unix - Linux man page](https://linux.die.net/man/1/cadaver)

Puedes hacer estas acciones de forma manual con CURL, te pongo varios ejemplos para ver como sería

### Manualmente con curl

```
# MOVE REQUEST
curl -s -X MOVE -H "Destination:http://<ip>/archivo-que-modificas" http://<ip>/archivo-normal
# PUT REQUEST
curl -T 'pwned.txt' 'http://<ip>/'
```

#### Referencias
[WebDav: Qué es, para qué sirve y cómo configurarloRedesZone](https://www.redeszone.net/tutoriales/internet/webdav-que-es-configuracion/)
[WebDavHackTricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/put-method-webdav)
