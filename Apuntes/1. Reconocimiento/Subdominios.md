[[Apuntes]]
## Check List

1. Inspeccionar los certificados

2. Datos de los DNS de los subdominios que encontramos

3. Comprobar que con http y https sale lo mismo


# Enumeración pasiva

### Phonebook

[Phonebook.cz - Intelligence X](https://phonebook.cz/)

### Intelx

https://intelx.io/

### CRFR
```
python3 ctfr.py -d example.com
```

https://github.com/UnaPibaGeek/ctfr

### Sublist3r


```
python3 sublist3r.py -d example.com
```

https://github.com/aboul3la/Sublist3r

### https://dnsdumpster.com/

### Hackertarget

https://hackertarget.com/

## Enumeración activa

### ffuf

```
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-top100000.txt -H "Host: FUZZ.example.com" -u http://<IP> -fs {size}
```

|Parámetro|Descripción|
|---|---|
|-w|Wordlist|
|-H|Cabecera del mensaje|
|-u|IP del dominio principal|
|-fs|Ignorar cualquier resultado que tenga X tamaño [Ej: -fs 200,342]|

### Wfuzz


```
wfuzz -c --hc=403 -t 20 -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.example.com" -u https://example.com
```

### Gobuster


```
gobuster vhost -u https://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt
```

#### Con filtros

```
gobuster vhost -u https://example.com -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -t 20 | grep -v "403"
```

### DNSRecon

**DNSRecon** es una herramienta de línea de comando de Linux utilizada para la enumeración y el escaneo de servidores de nombres de dominio (DNS)

```
dnsrecon -brt -d <dominio dns>
```

|Parámetro|Descripción|
|---|---|
|-b|Realiza una búsqueda inversa del servidor de nombres de dominio. Esta opción utiliza la dirección IP del servidor para buscar dominios alojados en ese servidor.|
|-r|Realiza una búsqueda de registros DNS de forma recursiva en el servidor de nombres de dominio.|
|-t|Especifica el tipo de registro DNS que se va a buscar. Si no se especifica, se buscarán todos los tipos de registro.|
|-d|Dominio DNS|