[[Apuntes]]

## Enumerar con nmap


```
nmap -p25 --script=smtp* <IP>
```

## Enumerar usuario

```
nmap <IP> --script=smtp* -p 25
nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 $ip
```

## Conectarnos con telnet o ncat

```
telnet <IP> 25
nc <IP> 25
```

### Comandos básicos

|Comando|Descripción|
|---|---|
|`HELO domain_name`|Identificarse con el servidor SMTP y proporcionar el nombre de dominio del cliente.|
|`MAIL FROM: sender_address`|Especificar la dirección de correo electrónico del remitente del mensaje.|
|`RCPT TO: recipient_address`|Especificar la dirección de correo electrónico del destinatario del mensaje. Puedes especificar múltiples destinatarios utilizando varios comandos `RCPT TO:`.|
|`DATA`|Iniciar la sección de datos del mensaje. Puedes proporcionar el cuerpo del mensaje después de este comando, terminando con una línea que contenga solo un punto (`.`).|
|`QUIT`|Terminar la sesión SMTP y cerrar la conexión.|

Artículo más extenso sobre los comandos y respuestas: [https://mailtrap.io/blog/smtp-commands-and-responses/](https://mailtrap.io/blog/smtp-commands-and-responses/)

### Rutas importantes


```
/var/mail/$user
```

## Fuerza bruta

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt smpt
```