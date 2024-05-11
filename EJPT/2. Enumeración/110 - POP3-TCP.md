## Conectarnos mediante nc o telnet

```
nc <ip> 110
telnet <ip> 110
```

### Comandos básicos

|Comando|Descripción|
|---|---|
|`USER username`|Iniciar sesión en el servidor POP3 proporcionando un nombre de usuario.|
|`PASS password`|Proporcionar la contraseña correspondiente al nombre de usuario proporcionado anteriormente.|
|`LIST`|Mostrar una lista de los correos electrónicos disponibles en el servidor, con información sobre su tamaño y número de mensaje.|
|`RETR message_number`|Mostrar el contenido del correo electrónico correspondiente al número de mensaje especificado.|
|`DELE message_number`|Marcar el correo electrónico correspondiente al número de mensaje especificado para ser eliminado del servidor.|
|`NOOP`|No realizar ninguna operación. Puede utilizarse para mantener la conexión activa.|
|`RSET`|Restablecer la conexión a su estado inicial. Puede utilizarse para cancelar operaciones pendientes.|
|`QUIT`|Cerrar la sesión actual en el servidor POP3 y terminar la conexión.|

## Fuerza bruta

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt pop3
```