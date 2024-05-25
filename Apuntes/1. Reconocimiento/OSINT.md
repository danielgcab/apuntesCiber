#recon #osint

-------------
## Google Hacking/Dorking
#osint #dorking #google

| Filtro   | Ejemplo         | Descripcion                                          |
| -------- | --------------- | ---------------------------------------------------- |
| site     | site:j4ckie.com | Mostrará solo el sitio j4ckie.com                    |
| inurl    | inurl:admin     | Si en el url hay un admin lo mostrará                |
| filetype | filetype:pdf    | Si hay un archivo en google que sea .pdf lo mostrará |
| intitle  | intitle:admin   | Devuelve donde en el titulo ponga admin              |

18 Google Dorking / Hacking más famosos:
https://pentest-tools.com/information-gathering/google-hacking

## Wayback Machine
#osint #waybackMachine

Sirve para poder ver versiones antiguas de páginas web, guarda el histórico de las webs, puede servir para antiguas paginas que pueden tener contenido que podamos recuperar y/o ver información relevante y puede incluso seguir existiendo.

https://web.archive.org/

## Sublist3r
#osint #Sublist3r

Es una herramienta de Python diseñada para enumerar subdominios de sitios web que usan OSINT. Ayuda a recopilar y recopilar subdominios para el dominio al que se dirigen.

https://github.com/aboul3la/Sublist3r


```
sublist3r -d <dominio>
```

|Short Form|Long Form|Description|
|---|---|---|
|-d|--domain|Domain name to enumerate subdomains of|
|-b|--bruteforce|Enable the subbrute bruteforce module|
|-p|--ports|Scan the found subdomains against specific tcp ports|
|-v|--verbose|Enable the verbose mode and display results in realtime|
|-t|--threads|Number of threads to use for subbrute bruteforce|
|-e|--engines|Specify a comma-separated list of search engines|
|-o|--output|Save the results to text file|
|-h|--help|show the help message and exit|

## SSL/TLS Certificates
#osint #ssl/tls

Podemos consultar si el certificado SSL/TLS es parte del **Certificate Transparency logs (CT)** buscándolo en las siguientes webs y ver si es malicioso o no.

https://crt.sh/

https://ui.ctsearch.entrust.com/ui/ctsearchui

## Imágenes
#osint #image


Para hacer un reconocimiento de imágenes podemos acceder a las siguiente web

https://pimeyes.com/en

## Credenciales y brechas de seguridad
#osint #credentials

Aquí podemos encontrar información que está expuesta a internet, cuando realicemos reconocimiento a una auditoría es una buena práctica buscar el dominio en esta página, aunque es de pago.

https://www.dehashed.com/

## Correos electrónicos
#osint #mail

A la hora de realizar una auditoría podemos buscar el dominio de la empresa a la que estemos haciendo para ver si hay expuesto sus correos y poder sacar ventaja.

- Hunter: [https://hunter.io/](https://hunter.io/)

- Intelligence X: [https://intelx.io/](https://intelx.io/)

- Phonebook.cz: [https://phonebook.cz/](https://phonebook.cz/)

- Clearbit Connect: [Chrome Extension](https://chrome.google.com/webstore/detail/clearbit-connect-free-ver/pmnhcgfcafcnkbengdcanjablaabjplo)


Para confirmar que el correo es correcto podemos validarlo en las siguientes webs:
https://email-checker.net/validate
https://verifyemailaddress.org/email-validation

## Spiderfoot

**SpiderFoot** is an open source intelligence (OSINT) automation tool. It integrates with just about every data source available and utilises a range of methods for data analysis, making that data easy to navigate.

SpiderFoot has an embedded web-server for providing a clean and intuitive web-based interface but can also be used completely via the command-line. It's written in **Python 3** and **MIT-licensed**.