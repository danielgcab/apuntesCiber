El Protocolo de transferencia de hipertexto es el protocolo de comunicación que permite las transferencias de información a través de archivos en la World Wide Web

## Check List

- Whatweb o Wappalyzer para saber las tecnologías (Wordpress, Apache, Tomcat etc)
    
- Comprobar si podemos ver las siguientes rutas
    
    - robots.txt
        
    - sitemap.xml
        
    - web.config
        
    - crossdomain.xml
        
    
- Fuerza bruta directorios
    
    - También directorios ocultos
        


## Enumeración manual

### Favicon

A veces cuando se crean las páginas y el favicon no lo cambian, puede que si está el por defecto de la instalación podamos saber que tecnología está usando descargando el favicon.ico y pasandolo a hash con md5sum.

#### Linux

```
curl https://example.com/path/to/favicon.ico | md5sum
```

#### Powershell

```
curl https://example.com/path/to/favicon.ico -UseBasicParsing -o favicon.ico 
Get-FileHash .\favicon.ico -Algorithm MD5
```

Una vez obtenido el md5sum cogemos el resultado y vamos a la siguiente página: [https://wiki.owasp.org/index.php/OWASP_favicon_database](https://wiki.owasp.org/index.php/OWASP_favicon_database) una vez dentro filtramos por el md5sum.

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FHDZ8LFaZI6Er2nCmKs1x%252Fimagen.png%3Falt%3Dmedia%26token%3D543afd80-b17b-453b-90df-a5ce4f0525b3&width=768&dpr=4&quality=100&sign=9b80fb140e9a8d059a523dc4cfeba85d20c3e83c8c0f81cfe2dfdb73b48fb918)

Ejemplo que el md5sum coincide con el framework cgiirc

### Sitemap.xml

```
http://example.com/sitemap.xml
```

### HTTP Headers


```
curl http://example.com -v # ver cabezera de la web
```

### Conectarnos mediante telnet o Ncat

```
nc <IP> 80
telnet <IP> 80
```

Una vez que hayas establecido la conexión, podrías enviar solicitudes HTTP utilizando comandos como `GET`, `POST`, `PUT`, `DELETE`, `HEAD`, `OPTIONS`, entre otros. Aquí te muestro algunos ejemplos:

- Realizar una solicitud GET:

```
GET / HTTP/1.1
Host: www.example.com
```

Este comando solicita la página principal del sitio web `www.example.com` a través de HTTP.

- Realizar una solicitud POST:

```
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 25

username=john&password=doe
```

Este comando envía datos de formulario a través de una solicitud HTTP POST a la página `/login` del sitio web `www.example.com`.

Ten en cuenta que, dependiendo del servidor web con el que te conectes, es posible que necesites autenticarte o proporcionar alguna otra información adicional para realizar ciertas acciones. Además, ten en cuenta que la interacción directa con un servidor web a través de `ncat` puede no ser segura ni práctica para la mayoría de los casos de uso.

### Comandos básicos telnet/nc

|Comando|Descripción|
|---|---|
|`GET /url HTTP/1.1`|Obtener la página web correspondiente a la URL especificada.|
|`HEAD /url HTTP/1.1`|Obtener los encabezados de la respuesta HTTP correspondiente a la URL especificada.|
|`POST /url HTTP/1.1`|Enviar datos a un servidor web utilizando el método HTTP POST.|
|`PUT /url HTTP/1.1`|Enviar datos a un servidor web utilizando el método HTTP PUT.|
|`DELETE /url HTTP/1.1`|Eliminar un recurso de un servidor web utilizando el método HTTP DELETE.|
|`OPTIONS /url HTTP/1.1`|Obtener una lista de los métodos HTTP que son compatibles con el recurso especificado en la URL.|
|`TRACE /url HTTP/1.1`|Realizar una solicitud de seguimiento de una solicitud a un servidor web.|
|`CONNECT host:port HTTP/1.1`|Establecer una conexión TCP/IP segura a un servidor web utilizando el protocolo HTTPS.|
|`HOST: hostname`|Especificar el nombre de host para la solicitud HTTP.|
|`User-Agent: user-agent-string`|Especificar el tipo de navegador o agente de usuario utilizado para la solicitud HTTP.|
|`Referer: referring-url`|Especificar la URL de la página que enlaza a la solicitud HTTP.|
|`Content-Type: mime-type`|Especificar el tipo de contenido enviado con la solicitud H|

## Enumeración 443

## Connect 443

Este comando nos dará información de la págona como el certificado SSL web, el código fuente, verisones de TLS, SSLv, emails etc..


```
openssl s_client -connect <ip>/domain:443 #GET 
```

### SSLscan

```
sslscan https://ip/domain
```

## Bypass Status Codes

### 403 (Forbidden)

1. Usando la cabezera "X-Original-URL" (Web Cache Poisoning)

```
GET /admin HTTP/1.1
Host: example.com
```

Bypass con:

```
GET /admin HTTP/1.1
Host: example.com
X-Original-URL: /admin
```

1. Añadiendo un %2e después del primer /

```
example.com/admin --> (403)
```

Bypass con:


```
http://example.com/%2e/admin
```

1. Añadiendo un (.) punto y barra / y un ; en la URL

```
http://example.com/admin --> (403)
```

Bypass con:


```
http://example.com/secret/. # un punto
http://example.com/secret// # añadiendo 2 barras
http://example.com/./secret/.. # añadiendo un . y dos al final
http://example.com/;/secret # un ; entre medio
http://example.com/.;/secret # un . y ; 
http://example.com//;//secret # un ; y doble barra
```

1. Añadiendo un ";/" después del nombre del directorio

```
http://example.com/admin
```

Bypass con:

```
http://example.com/admin..;/
```

1. Cambiando los caracteres en mayusculas y minusculas

```
http://example.com/admin
```

Bypass con:

```
http://example.com/aDmIn
```

## CMS - Tecnologías web

- [Wordpress](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/wordpress)
    
- [Tiny File Manager](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/tiny-file-manager)
    
- [Joomla](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/joomla)
    
- [Magento](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/magento)
    
- [Drupal](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/drupal)
    
- [TeamCity](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/teamcity)
    
- [WebDav](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/webdav)
    
- [Tomcat](https://j4ckie0x17.gitbook.io/notes-pentesting/enumeracion-de-servicios/80-433-http-https/tomcat)
    
- Adobe ColdFusion
    

## Enumeración automatizada

### Web Discovery

#### Wordlists

https://github.com/danielmiessler/SecLists
https://github.com/kkrypt0nn/wordlists

### Gobuster

#### Comando básico

```
gobuster dir -e -u <IP> -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t <threads>
```

#### Modo DIR

|Parámetro|Descripción|
|---|---|
|-e|Modo expandido, imprime URL completas|
|-u|URL|
|-w|Wordlist|
|-t|Número de subprocesos simultáneos|
|-r|Sigue los redirects|
|-x|Extensión(es) de archivo para buscar (solo modo dir) [.php,.json,.xml,.doc,.sh,.js,.txt]|
|-f|Fuerzas a ponerle una / al final de la ruta, a veces si no encuentra nada, prueba de poner este parámetro a ver si te aparece|

[![Logo](https://www.kali.org/images/favicon.png)gobuster | Kali Linux ToolsKali Linux](https://www.kali.org/tools/gobuster/)

### Dirsearch

#### Comando básico

```
dirsearch -u  <url> -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -E <extensiones (php,xml,json etc)
```

|Parámetro|Descripción|
|---|---|
|-u|URL|
|-w|Wordlist|
|-E|Extensiones|
|-t|Número de subprocesos simultáneos|
|-i|Incluir solo códigos de estado que quieras (-i 200,301)|
|-x|Excluir solo códigos de estado que quieras (-i 404,403)|

[![Logo](https://www.kali.org/images/favicon.png)dirsearch | Kali Linux ToolsKali Linux](https://www.kali.org/tools/dirsearch/)

### Wfuzz

#### Comando básico

```
wfuzz -c --hc=404 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -t 400 <IP/FUZZ> # básico
```

#### Comando buscar archivos

```
wfuzz -c --hc=404 -z files,/usr/share/wordlists/SecLists/Discovery/Web-Content/raft-large-files.txt -u http://IP/FUZZ
```

|Parámetro|Descripción|
|---|---|
|-c|Formato coloreado|
|--hc=|Ocultar código de estado (404,403)|
|-w|Wordlist|
|-t|Número de subprocesos simultáneos|
|-w|Extensiones (normalmente incluirlas en un archivo txt de arriba a abajo)|
|--hl/hw/hh=|Ocultar líneas,palabras, caracteres indicados|
|--sc/sl/sw/sh|Mostrar respuestas con el código,líneas,palabras, caracteres indicados|
|-b|Especificar cookie|

Si no te reporta nada, puede que sea por que no le has puesto una / al final de FUZZ Ej:<IP/FUZZ>/

[![Logo](https://www.kali.org/images/favicon.png)wfuzz | Kali Linux ToolsKali Linux](https://www.kali.org/tools/wfuzz/)

#### Links de apoyo

- [https://www.pinguytaz.net/index.php/2019/10/18/wfuzz-navaja-suiza-del-pentesting-web-1-3/](https://www.pinguytaz.net/index.php/2019/10/18/wfuzz-navaja-suiza-del-pentesting-web-1-3/)

- [https://www.pinguytaz.net/index.php/2019/10/22/wfuzz-navaja-suiza-del-pentesting-web-2-3/](https://www.pinguytaz.net/index.php/2019/10/22/wfuzz-navaja-suiza-del-pentesting-web-2-3/)

- [https://www.pinguytaz.net/index.php/2019/10/28/wfuzz-navaja-suiza-del-pentesting-web-3-3/](https://www.pinguytaz.net/index.php/2019/10/28/wfuzz-navaja-suiza-del-pentesting-web-3-3/)


### Ffuf

#### Comando básico

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -u https://target/FUZZ -mc 200,301 -fs {size}
```

#### Web Discovery

|Parametro|Descripción|
|---|---|
|-w|Wordlist|
|-H|Añade o edita la cabezera de la URL donde hará FUZZ (buscar subdominios)|
|-fs|Filtrar por tamaño de respuesta|
|-X|Especificar que queremos que haga fuzzing de parámetros POST [Ejemplo: -X POST -d "username=admin\&password=FUZZ""|
|-d|Datos POST (-d "username=admin\&password=FUZZ")|
|-H|Poner cabecera [-H "Host: FUZZ.example-com"] o [-H 'Content-Type: application/x-www-form-urlencoded']|
|-mc|Que solamente muestre estos códigos de respuesta (-mc 200,301)|
|-e|Para extensiones, separadas por comas (php,xml,txt,asp)|


https://linuxcommandlibrary.com/man/ffuf

#### DNS Bruteforce

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.example-com" -u http://<IP>
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.example.com" -u http://<IP> -fs {size}
```

### Nikto

```
nikto -h <IP>
```

## Fuerza bruta

### HTTP Auth

```
hydra -L /usr/share/brutex/wordlists/simple-users.txt -P /usr/share/brutex/wordlists/password.lst sizzle.htb.local http-get /certsrv/ # Use https-get mode for httpS
```

### Medusa

```
medusa -h <IP> -u <usuario> -P <pass.txt> -M http -m DIR:/path/to/auth -T 10
```

### Hydra

#### HTTP - POST

```
hydra -L /usr/share/brutex/wordlists/simple-users.txt -P /usr/share/brutex/wordlists/passw
```