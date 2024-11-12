[[Apuntes]]

#recon #active 

**¿Qué es Reconocimiento Activo?**

El reconocimiento activo implica la interacción directa con un sistema o red objetivo para obtener información valiosa sobre su configuración, vulnerabilidades y posibles puntos de entrada para un ataque.

Las siguientes herramientas son parte del reconocimiento activo, entra dentro de cada una de ellas para saber como se utilizan

# **Ping**
#ping #active

Ping es un comando que envía un paquete de tipo ICMP Echo a un sistema remoto. Si el sistema remoto está en línea y el paquete "ping" fue correctamente enrutado y no bloqueado por ningún cortafuegos, el sistema remoto debería enviar un paquete ICMP Echo Reply en respuesta. De manera similar, la respuesta al "ping" debería llegar al primer sistema si es correctamente enrutada y no es bloqueada por ningún cortafuegos.
```bash
ping -c 1 <IP target>  # linux
ping -n 1 <IP target> # windows
```

|Parámetro|Descricpión|
|---|---|
|-c|Numero de paquetes|
|-n|Numero de paquetes|
El tamaño de la cabecera de un paquete ICMP es de 8 bytes.

# **Web Browser**

Podemos utilizar la herramienta Devolper Tools (se abre con F12 o Ctrl + Shift + I) para hacer un reconocimiento de la página web a la que nos estemos enfrentando, viendo como se transimen los datos, las cookies, java script etc.

También podemos utilzar el [Wappalyzer](https://www.wappalyzer.com/) el cuál nos ayudará a que tecnologías nos estamos enfrentando y versiones.


# **Traceroute**
#traceroute #active 

Supongamos que queremos trazar la ruta que sigue un paquete desde nuestro equipo local hasta el sitio web de Google. El resultado de la traza podría ser algo así:

``` bash
> traceroute google.com
```
```
1  192.168.1.1 (192.168.1.1)  1 ms  1 ms  1 ms
2  10.0.0.1 (10.0.0.1)  10 ms  10 ms  10 ms
3  68.86.225.1 (68.86.225.1)  15 ms  15 ms  15 ms
4  68.85.205.173 (68.85.205.173)  20 ms  20 ms  20 ms
5  68.86.90.18 (68.86.90.18)  25 ms  25 ms  25 ms
6  68.86.85.46 (68.86.85.46)  30 ms  30 ms  30 ms
7  68.86.84.13 (68.86.84.13)  35 ms  35 ms  35 ms
8  72.14.209.108 (72.14.209.108)  40 ms  40 ms  40 ms
9  172.253.66.103 (172.253.66.103)  45 ms  45 ms  45 ms
10  216.239.48.44 (216.239.48.44)  50 ms  50 ms  50 ms
11  172.253.50.209 (172.253.50.209)  55 ms  55 ms  55 ms
12  lga34s19-in-f14.1e100.net (172.217.6.14)  60 ms  60 ms  60 ms
```

Cada línea representa un salto o "hop" de la traza, mostrando la dirección IP del router que gestionó ese salto, el tiempo que tardó el paquete en recorrer ese salto y el nombre del servidor si está disponible.

En este ejemplo, la traza muestra que el paquete realizó 12 saltos antes de llegar a su destino final en el sitio web de Google. La dirección IP y el nombre del servidor final aparecen en la última línea de la traza.

# **Telnet**
#telnet #active

El protocolo TELNET (Teletype Network) fue desarrollado en 1969 para comunicarse con un sistema remoto a través de una interfaz de línea de comandos (CLI). Por lo tanto, el comando telnet utiliza el protocolo TELNET para la administración remota. El puerto predeterminado utilizado por telnet es el 23. Desde una perspectiva de seguridad, telnet envía todos los datos, incluyendo nombres de usuario y contraseñas, en texto claro. Enviar en texto claro hace que sea fácil para cualquier persona que tenga acceso al canal de comunicación, robar las credenciales de inicio de sesión. La alternativa segura es el protocolo SSH (Secure SHell).

```
telnet <target> 80
```


# **Nc** 
#netcat #active 

Netcat o simplemente nc tiene diferentes aplicaciones que pueden ser de gran valor para un pentester. Netcat admite tanto los protocolos TCP como UDP. Puede funcionar como un cliente que se conecta a un puerto en escucha; alternativamente, puede actuar como un servidor que escucha en un puerto de su elección. Por lo tanto, es una herramienta conveniente que puede usar como un cliente o servidor simple sobre TCP o UDP.

Ncat como cliente

```
nc <IP target> <puerto>
```

#### 

[](https://j4ckie0x17.gitbook.io/notes-pentesting/reconocimiento/activo#ncat-como-servidor)

Ncat como servidor

```
nc -lvnp 1234
```

|Option|Significado|
|---|---|
|-l|Modo de escucha|
|-p|Especificar el número de puerto|
|-n|Solo numérico; no resuelve nombres de host a través de DNS|
|-v|Salida detallada (opcional, pero útil para descubrir cualquier error)|
|-vv|Muy detallado (opcional)|
|-k|Mantener la escucha después de que el cliente se desconecte|

# **Nmap**
#nmap #active 
Escaneo típico CTF

``` bash
nmap -sSCV -p- --open --min-rate 5000 -n -Pn <IP> -oA scanTarget
nmap -sSCV -p- --open -T<1-5> -n -Pn <IP> -oA scanTarget
nmap -sSCV --top-ports --open --min-rate 5000 -n -Pn <IP> -oA scanTarget
```

|Parámetro|Descripción|
|---|---|
|`-sS`|Escaneo TCP SYN, que no hace el 3-way handshake (el escaneo es más rápido)|
|`-sU`|Escaneo de puertos UDP|
|`-sC`|Serie de scripts de reconocimiento básicos|
|`-sV`|Serie de scripts para determinar las versiones de los puertos abiertos|
|`-p-`|Escaneo de todos los puertos (del 1 al 65535)|
|`--open`|Muestra solamente los puertos abiertos|
|`--min-rate`|Define la tasa de paquetes mínima a enviar durante el escaneo|
|`-n`|Deshabilita la resolución DNS de los nombres de host|
|`-Pn`|Deshabilita el reconocimiento de host y asume que todos los hosts son activos|
|`-oA`|Exporta los resultados del escaneo en tres formatos diferentes (XML, JSON y Nmap)|
|`--top-ports`|Escanea solamente los puertos más frecuentes|
|`-T<0-5>`|Define la velocidad del escaneo, desde 0 (sigiloso) hasta 5 (agresivo)|


# **Escaneo descubrimiento de hosts**
#active #hostDiscovery

| Tipo de Escaneo           | Comando de Ejemplo                        |
| ------------------------- | ----------------------------------------- |
| Escaneo ARP               | sudo nmap -PR -sn MACHINE_IP/24           |
| Escaneo ICMP Echo         | sudo nmap -PE -sn MACHINE_IP/24           |
| Escaneo ICMP Timestamp    | sudo nmap -PP -sn MACHINE_IP/24           |
| Escaneo ICMP Address Mask | sudo nmap -PM -sn MACHINE_IP/24           |
| Escaneo TCP SYN Ping      | sudo nmap -PS22,80,443 -sn MACHINE_IP/30  |
| Escaneo TCP ACK Ping      | sudo nmap -PA22,80,443 -sn MACHINE_IP/30  |
| Escaneo UDP Ping          | sudo nmap -PU53,161,162 -sn MACHINE_IP/30 |

Recuerda agregar la opción -sn si solo estás interesado en descubrir los hosts sin escanear los puertos. Omitir -sn hará que Nmap use la opción de escaneo de puertos para los hosts activos.

|Parámetro|Descripción|
|---|---|
|-n|no hacer una búsqueda de DNS|
|-R|búsqueda de DNS inversa para todos los hosts|
|-sn|solo descubrimiento de hosts|

# **Escaneo básico**
#scanBasic #nmap 

|Tipo de Escaneo de Puertos|Comando de Ejemplo|
|---|---|
|Escaneo TCP de Conexión|nmap -sT 10.10.170.21|
|Escaneo TCP SYN|sudo nmap -sS 10.10.170.21|
|Escaneo UDP|sudo nmap -sU 10.10.170.21|

| Parámetro             | Descripción                                                      |
| --------------------- | ---------------------------------------------------------------- |
| -p-                   | Escanear todos los puertos                                       |
| -p1-1023              | Escanear los puertos del 1 al 1023                               |
| -F                    | Escanear los 100 puertos más comunes                             |
| -r                    | Escanear los puertos en orden consecutivo                        |
| -T<0-5>               | Establecer la plantilla de tiempo (0 es el más lento)            |
| --max-rate 50         | Establecer la tasa máxima de envío de paquetes a 50/seg          |
| --min-rate 15         | Establecer la tasa mínima de envío de paquetes a 15/seg          |
| --min-parallelism 100 | Establecer el número mínimo de sondas a enviar en paralelo a 100 |

# **Escaneo avanzado**
#scanAdvanced #nmap 

|Tipo de Escaneo|Comando de Ejemplo|
|---|---|
|Escaneo TCP Null|sudo nmap -sN 10.10.59.16|
|Escaneo TCP FIN|sudo nmap -sF 10.10.59.16|
|Escaneo TCP Xmas|sudo nmap -sX 10.10.59.16|
|Escaneo TCP Maimon|sudo nmap -sM 10.10.59.16|
|Escaneo TCP ACK|sudo nmap -sA 10.10.59.16|
|Escaneo TCP Window|sudo nmap -sW 10.10.59.16|
|Escaneo TCP Personalizado|sudo nmap --scanflags URGACKPSHRSTSYNFIN 10.10.59.16|
|IP de Origen Falsificado|sudo nmap -S SPOOFED_IP 10.10.59.16|
|Dirección MAC Falsificada|--spoof-mac SPOOFED_MAC|
|Escaneo de Señuelos|nmap -D DECOY_IP,ME 10.10.59.16|
|Escaneo Zombie|sudo nmap -sI ZOMBIE_IP 10.10.59.16|

Estos tipos de escaneo se basan en establecer banderas TCP de manera inesperada para solicitar una respuesta de los puertos. Los escaneos Null, FIN y Xmas provocan una respuesta de los puertos cerrados, mientras que los escaneos Maimon, ACK y Window provocan una respuesta de los puertos abiertos y cerrados.

|Parámetro|Descripción|
|---|---|
|--source-port PORT_NUM|especifica el número de puerto de origen|
|--data-length NUM|agrega datos aleatorios para alcanzar la longitud dada|

|Parámetro|Descripción|
|---|---|
|--reason|explica cómo Nmap llegó a su conclusión|
|-v|modo detallado|
|-vv|modo muy detallado|
|-d|depuración|
|-dd|más detalles para la depuración|

# **Escaneo Versiones - OS**
#nmap #scanOS
 
|Parámetro|Descripción|
|---|---|
|-sV|Determina la información de servicio / versión en los puertos abiertos.|
|-sV --version-light|Intenta las sondas más probables (2).|
|-sV --version-all|Intenta todas las sondas disponibles (9).|
|-O|Detecta el sistema operativo.|
|--traceroute|Ejecuta un traceroute al objetivo.|
|--script=SCRIPTS|Ejecuta scripts de Nmap.|
|-sC o --script=default|Ejecuta scripts predeterminados.|
|-A|Equivalente a -sV -O -sC --traceroute.|
|-oN|Guarda la salida en formato normal.|
|-oG|Guarda la salida en formato legible por grep.|
|-oX|Guarda la salida en formato XML.|
|-oA|Guarda la salida en formatos normal, XML y legible por grep.|

# **Evasión de firewall**
#nmap #scanFirewallEvasion

```bash
nmap -p22 <ip> --mtu 8 | --mtu 16
```

|Parámetro|Descripción|
|---|---|
|-f; --mtu <valor>|Fragmentar los paquetes (tiene que ser el valor multiplicable por 8)|
|-D <clon1,clon2,clon3>|Ocultar escaneo con señuelos|
|-S <IP>|Cambiar IP de origen falseándola para ocultar nuestra IP original|
|-e|Utilizar IP especifica|
|-g / --source-port|Darle el puerto origen por el que se hará el escaneo|
|--data-length|Agregar datos aleatorios a los paquetes que enviamos|
|--ip-options|Enviar paquetes con opciones especificas de la IP|
|--ttl|Establecer un TTL (Time to Live) especifico|
|--spoof-mac|Cambiar nuestra MAC, a veces filtran las MAC y cambiándola puede que hagamos bypass al firewall [Ej: --spoof-mac Dell,Vmware]|
|--badsum|Envía paquetes con una suma de verificación TCP/UDP/SCTP falsa|