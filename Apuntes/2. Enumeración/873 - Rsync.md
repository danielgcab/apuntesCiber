[[Apuntes]]
## ¿Qué es y cómo funciona?

Rsync es una aplicación libre para sistemas de tipo Unix y Microsoft Windows que ofrece transmisión eficiente de datos incrementales, que opera también con datos comprimidos y cifrados.

## Enumeración

```
nmap -sV --script "rsync-list-modules" -p <PORT> <IP>
msf> use auxiliary/scanner/rsync/modules_list

#Example using IPv6 and a different port
rsync -av --list-only rsync://[dead:beef::250:56ff:feb9:e90a]:8730
```

### Listar una carpeta

```
rsync -av --list-only rsync://<IP>/shared_name # listar los archivos
rsync -av rsync://<IP>:873/shared_name ./rsyn_shared -p "password" # copiarlo localmente, 
```

### Subir archivos

```
rsync -a <file> rsync://<IP>/path/where/you/want/to/putit
```