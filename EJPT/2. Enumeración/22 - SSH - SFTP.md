SSH es el nombre de un protocolo y del programa que lo implementa cuya principal función es el acceso remoto a un servidor por medio de un canal seguro en el que toda la información está cifrada.

### Check List


```
ssh <username>@<IP> # comando básico
ssh <username>@<IP> -p <password> # con password
ssh <username>@<IP> -i <privatekey> # conectarte con private key (un archivo con la private key dentro>
```

### Windows RDP login


```
xfreerdp /dynamic-resolution +clipboard /cert:ignore /v:MACHINE_IP /u:Administrator /p:'<passowrd'
```

### Nmap scripts

```
ls -al /usr/share/nmap/scripts/ | grep -e “ssh” # para ver los scripts posibles del ssh
nmap -p22 <ip> -sC # Script por defecto de nmap
nmap -p22 <ip> -sV # Version del servicio
nmap -p22 <ip> --script ssh2-enum-algos # Recuperar algoritmos soportados
nmap -p22 <ip> --script ssh-hostkey --script-args ssh_hostkey=full # Recuperar keys debiles
nmap -p22 <ip> --script ssh-auth-methods --script-args="ssh.user=root" # Revisar metodos de autenticarnos
```

## Transferencia de archivos

### SCP (Secure Copy Protocol)

El Secure Copy Protocol, también conocido como Secure Copy, es un protocolo de sistemas informáticos que se utiliza para la transferencia de archivos de forma segura.

#### Máquina local --> Remote SERVER (SSH)

```
scp <archivo> usuario@<IP>:/ruta/donde/lo/pondras
```

#### Remote SERVER (SSH) --> Máquina local

```
scp usuario@<IP>:file.txt /ruta/local/
```

## Fuerza bruta

### Hydra

```
hydra -l <user> -P /usr/share/wordlists/rockyou.txt ssh://<IP> # SSH brute force
```

### Ssh2John


```
ssh2john <private_key> > hash_key # convertir la private key en hash para john
```

### John

```
john <hash_key> --wordlist=/usr/share/wordlists/rockyou.txt
```