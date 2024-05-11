
#recon #pasive
## ¿Qué es Reconocimiento Pasivo?

El reconocimiento pasivo se refiere a la identificación de información sensible o valiosa sobre un objetivo sin la necesidad de interactuar directamente con él. El objetivo puede ser un sistema informático, una red, una organización, una persona o cualquier otra entidad que pueda ser un objetivo para un atacante.

El reconocimiento pasivo se lleva a cabo utilizando técnicas no invasivas que no generan ningún tráfico en la red o actividad sospechosa. Estas técnicas incluyen la recopilación de información de Whois, la exploración de servicios públicos, la revisión de información pública en sitios web y redes sociales, entre otros.

- Whois
- Nslookup & Dig
- DNSDumpster
- Shodan.io

Los atacantes utilizan el reconocimiento pasivo para recopilar información valiosa sobre el objetivo antes de llevar a cabo un ataque. Esta información puede incluir direcciones IP, nombres de dominio, información de contacto, sistemas operativos y aplicaciones utilizadas, vulnerabilidades conocidas y otra información que pueda ser útil para planear un ataque exitoso.

## Whois
#pasive #whois

WHOIS es un protocolo de solicitud y respuesta que sigue la especificación RFC 3912. Un servidor WHOIS escucha en el puerto TCP 43 las solicitudes entrantes. El registrador de dominios es responsable de mantener los registros de WHOIS para los nombres de dominio que arrienda. El servidor de WHOIS responde con diversa información relacionada con el dominio solicitado. De particular interés, podemos aprender:

- Registrador: ¿A través de qué registrador se registró el nombre de dominio?
    
- Información de contacto del registrante: Nombre, organización, dirección, teléfono, entre otras cosas. (a menos que se oculte a través de un servicio de privacidad)
    
- Fechas de creación, actualización y vencimiento: ¿Cuándo se registró por primera vez el nombre de dominio? ¿Cuándo se actualizó por última vez? ¿Y cuándo hay que renovarlo?
    
- Servidor de nombres: ¿Qué servidor pedir para resolver el nombre de dominio?


```
whois exmaple.com
```

## Nslookup & Dig
#pasive #nslookup #dig

Encontrar la dirección IP de un nombre de dominio utilizando nslookup, que significa búsqueda de servidor de nombres. Debe ejecutar el comando nslookup DOMAIN_NAME, por ejemplo, nslookup example.com. O, de manera más general, puede usar nslookup OPTIONS DOMAIN_NAME SERVER. Estos tres parámetros principales son:

- OPCIONES contiene el tipo de consulta como se muestra en la siguiente tabla. Por ejemplo, puede usar A para direcciones IPv4 y AAAA para direcciones IPv6.

- DOMAIN_NAME es el nombre de dominio que está buscando.

- SERVIDOR es el servidor DNS que desea consultar. Puede elegir cualquier servidor DNS local o público para consultar. Cloudflare ofrece 1.1.1.1 y 1.0.0.1, Google ofrece 8.8.8.8 y 8.8.4.4 y Quad9 ofrece 9.9.9.9 y 149.112.112.112. Hay muchos más servidores DNS públicos entre los que puede elegir si desea alternativas a los servidores DNS de su ISP.


|Comando|Descripción|
|---|---|
|nslookup -type=A example.com|Busca el registro de DNS tipo A (dirección IP) para el dominio example.com|
|nslookup -type=MX example.com 1.1.1.1|Busca el registro de DNS tipo MX (servidor de correo) para el dominio example.com en el servidor DNS especificado (1.1.1.1)|
|nslookup -type=TXT example.com|Busca el registro de DNS tipo TXT (texto arbitrario) para el dominio example.com|
|dig example.com A|Busca el registro de DNS tipo A (dirección IP) para el dominio example.com utilizando el comando 'dig'|
|dig @1.1.1.1 example.com MX|Busca el registro de DNS tipo MX (servidor de correo) para el dominio example.com en el servidor DNS especificado (1.1.1.1) utilizando el comando 'dig'|
|dig example.com TXT|Busca el registro de DNS tipo TXT (texto arbitrario) para el dominio example.com utilizando el comando 'dig'|

## DNSDumpster
#pasive #dnsDumpster

Dnsdumpster es una herramienta de investigación de dominios para encontrar información relacionada con el host. Es el proyecto example.com. No solo el subdominio, sino que le brinda información sobre el servidor DNS, el registro MX, el registro TXT y un excelente mapeo de su dominio.

https://dnsdumpster.com/dnsdumpster.com

## Shodan.io
#pasive #shodan

Shodan es un motor de búsqueda que le permite al usuario encontrar iguales o diferentes tipos específicos de equipos conectados a Internet a través de una variedad de filtros.