El Protocolo de transferencia de archivos es un protocolo de red para la transferencia de archivos entre sistemas conectados a una red TCP, basado en la arquitectura cliente-servidor.

## Check List

- Comprobar si el usuario anonymous o ftp existen.
    
    - Si es así conectarnos y ver que nos deja ver.
        
    - Comprobar si nos deja subir archivos al ftp (READ/WRITE)
        
    
- Ejecutar algunos scripts de nmap de reconocimiento/enumeración.
    

```
ftp <IP> # conectarte al FTP
ls, get, put, mget # comandos comunes
anonymous:anonymous o anonymous: # probar credenciales por si el usuario anon esta habilitado
ftp:ftp
```

Wordlist fuerza bruta: [https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt)

## Nmap scripts


```
nmap --script ftp-* -p 21 <IP> # todos los scripts de reconocimiento
ls -al /usr/share/nmap/scripts/ | grep -e “ftp” # para ver los scripts posibles de ftp
## Opciones
ftp-anon.nse
ftp-bounce.nse
ftp-brute.nse
ftp-libopie.nse
ftp-proftpd-backdoor.nse
ftp-syst.nse
ftp-vsftpd-backdoor.nse
ftp-vuln-cve2010–4221.nse
tftp-enum.nse
```

### Descargar los archivos del FTP


```
wget -m ftp://anonymous:anonymous@<IP> #Download
wget -m --no-passive ftp://anonymous:anonymous@<IP> #Download
```

## Conectarse con telnet o ncat

```
nc <ip> 21
telnet <ip> 21
```

Una vez que hayas establecido la conexión, puedes interactuar con el servidor FTP utilizando comandos específicos del protocolo. Aquí te muestro algunos ejemplos

### Comandos básicos telnet - ncat

|Comando|Descripción|
|---|---|
|`USER username`|Iniciar sesión en el servidor FTP proporcionando un nombre de usuario.|
|`PASS password`|Proporcionar la contraseña correspondiente al nombre de usuario proporcionado anteriormente.|
|`LIST` o `NLST`|Mostrar una lista de los archivos disponibles en el servidor, con información sobre su tamaño y fecha de modificación. `NLST` sólo muestra los nombres de los archivos.|
|`RETR file_name`|Descargar el archivo correspondiente al nombre especificado desde el servidor FTP al cliente.|
|`STOR file_name`|Subir un archivo desde el cliente al servidor FTP con el nombre especificado.|
|`DELE file_name`|Eliminar el archivo correspondiente al nombre especificado del servidor FTP.|
|`PWD`|Mostrar el directorio actual en el servidor FTP.|
|`CWD directory_name`|Cambiar el directorio actual en el servidor FTP al directorio especificado.|
|`CDUP`|Subir un nivel en la jerarquía de directorios en el servidor FTP.|
|`MKD directory_name`|Crear un nuevo directorio en el servidor FTP con el nombre especificado.|
|`RMD directory_name`|Eliminar el directorio correspondiente al nombre especificado del servidor FTP.|
|`QUIT`|Cerrar la sesión actual en el servidor FTP y terminar la conexión.|

## Fuerza bruta

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt ftp
```