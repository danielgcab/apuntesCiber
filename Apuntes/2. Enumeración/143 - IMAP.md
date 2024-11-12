[[Apuntes]]

## Conectarse con ncat o telnet

```
nc <ip> 143
telnet <ip> 143
```

### Comandos básicos

|Comando|Descripción|
|---|---|
|`LOGIN username password`|Iniciar sesión en el servidor IMAP proporcionando un nombre de usuario y contraseña.|
|`SELECT mailbox_name`|Seleccionar un buzón de correo con el nombre especificado.|
|`FETCH message_number data_item`|Recuperar un mensaje de correo con el número de mensaje y el elemento de datos especificados. Por ejemplo, puedes recuperar la fecha de un mensaje utilizando `FETCH message_number BODY[HEADER.FIELDS (DATE)]`.|
|`UID FETCH unique_id data_item`|Recuperar un mensaje de correo utilizando su identificador único (UID) en lugar de su número de mensaje.|
|`STORE message_number flag`|Establecer o borrar una bandera en un mensaje de correo con el número de mensaje especificado. Por ejemplo, puedes marcar un mensaje como leído utilizando `STORE message_number +FLAGS (\Seen)`.|
|`UID STORE unique_id flag`|Establecer o borrar una bandera en un mensaje de correo utilizando su UID en lugar de su número de mensaje.|
|`SEARCH search_criteria`|Buscar mensajes de correo que coincidan con los criterios de búsqueda especificados. Por ejemplo, puedes buscar mensajes de correo no leídos utilizando `SEARCH UNSEEN`.|
|`UID SEARCH search_criteria`|Buscar mensajes de correo utilizando los criterios de búsqueda especificados, pero devolviendo una lista de UID en lugar de números de mensaje.|
|`APPEND mailbox_name flags date_time message`|Añadir un mensaje a un buzón de correo con el nombre especificado. Puedes proporcionar una lista de banderas para el mensaje (como `\Seen` o `\Flagged`) y una fecha y hora para el mensaje (en formato IMAP).|
|`LOGOUT`|Cerrar la sesión actual en el servidor IMAP y terminar la conexión.|

## Fuerza bruta

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt imap
```