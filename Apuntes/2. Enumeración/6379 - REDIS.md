## ¿Qué es y cómo funciona?

Redis es un motor de base de datos en memoria, basado en el almacenamiento en tablas de hashes pero que opcionalmente puede ser usada como una base de datos durable o persistente. Está escrito en ANSI C por Salvatore Sanfilippo, quien es patrocinado por Redis Labs

## Enumeración

```
nc -vn <IP> <port> # conectarnos mediante netcat
redis-cli -h <IP> # mediante el cliente de redis
```

### Comandos redis-cli

#### Autenticar nos

Por defecto en redis se puede acceder sin credenciales pero obviamente habrá veces que tendremos credenciales, se haría con el comando de abajo pero el espacio es un + `AUTH user+password` .

Normalmente la password la podríamos encontrar en el archivo redis.conf como "requirepass" y el usuario por defecto es `default`. Y el usuario puede estar en la variable "masteruser"

```
AUTH <username> <password>
```

#### Comandos autenticados

```
SELECT 0
[ ... Indicate the database ... ]
KEYS * 
[ ... Get Keys ... ]
GET <KEY>
[ ... Get Key ... ]
```

**Ejemplo**

En este caso el SELECT 0 tiene 4 keys en la bbdd 0 pero podría ser que fuera la 1 en otra ocasión, por defecto redis utiliza la 0.

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FBAfX0OOaYLk5SLLi4jyO%252Fimagen.png%3Falt%3Dmedia%26token%3D77cafb29-47c1-4b33-9d39-380a4d8eb525&width=300&dpr=4&quality=100&sign=6ae10a68fb640fd09d1a2066a25690cc0e3c8e89bc93358225595037028b2fec)

#### Type

Depende del tipo de KEY se tiene que poner TYPE para saber si son un key, una lista o un hash, hay diferentes maneras de abrir dependiendo de el tipo.

```
TYPE <KEY>
[ ... Type of the Key ... ]
LRANGE <KEY> 0 -1
[ ... Get list items ... ]
HGET <KEY> <FIELD>
[ ... Get hash item ... ]
```

**Ejemplo**

Para listar las KEYS.

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FHFsvb42PO0ZMdFawfLlT%252Fimagen.png%3Falt%3Dmedia%26token%3Dd6ef2ebd-e93e-4cd1-bd9d-d3024f1de50c&width=300&dpr=4&quality=100&sign=7c227ea4d284336bc342abbd92a4e8e5c98ffcbad6cd7cf15dafafcb6ae324d5)

Eso sale por que es una lista, y no se puede ver con GET, sino con `lrange <key> 0 -1`

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252F3fyiZgOdpHqI6tKwx4e0%252Fimagen.png%3Falt%3Dmedia%26token%3Df43152c5-a3a8-45a2-b467-1fc1e09b35c4&width=768&dpr=4&quality=100&sign=c774bf4b685c25a6c2b8cb580c4b52e2c42caff836fb498b1e9aafa0acf83924)