## ¿Qué es y cómo funciona?

Drupal es un sistema de gestión de contenidos (CMS, por sus siglas en inglés) de código abierto y gratuito que se utiliza para construir sitios web y aplicaciones en línea. Es una de las plataformas CMS más populares en el mundo, junto con WordPress y Joomla, y se utiliza en todo tipo de proyectos, desde sitios web de pequeñas empresas hasta grandes portales gubernamentales y corporativos.

Drupal está construido sobre el lenguaje de programación PHP y se basa en una arquitectura modular que permite a los usuarios agregar y personalizar funcionalidades y características según sea necesario. Además, cuenta con una amplia comunidad de desarrolladores y usuarios que contribuyen con la mejora y el mantenimiento del software.

Entre las características y funcionalidades que ofrece Drupal se encuentran:

- Gestión de contenidos: permite la creación y organización de contenidos y recursos multimedia.

- Personalización: ofrece un amplio conjunto de herramientas y módulos para personalizar la apariencia y la funcionalidad de un sitio web.

- Seguridad: cuenta con un sistema de seguridad robusto y actualizaciones regulares para proteger los sitios web de posibles vulnerabilidades.

- Escalabilidad: puede manejar sitios web de gran tamaño y tráfico.

- Multilingüismo: permite la creación de sitios web multilingües.

En resumen, Drupal es una plataforma CMS popular y versátil que ofrece muchas funcionalidades para la creación y gestión de sitios web y aplicaciones en línea.

## Comprobar usuarios

Para comprobar si existen los usarios solamente hay que crear una nueva cuenta e intentar ingresar el usuario que creemos que puede existir, si existe saldrá lo siguiente

![](https://j4ckie0x17.gitbook.io/~gitbook/image?url=https%3A%2F%2F1367155054-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252FqCnBDYTntMpZLlwqTWcg%252Fuploads%252FT0aZXrdQOqWp3TprVwhT%252Fimagen.png%3Falt%3Dmedia%26token%3D2cbf756d-ac95-43f6-839d-d1dd8c16ca69&width=768&dpr=4&quality=100&sign=a1da01019d9745582356a3b325357dda52ff303400df8d223b83fc96e55443c3)

## Comprobar versión

La versión del Drupal se puede saber de varias maneras pero la más efectiva es si tienes acceso al archivo CHANGELOG.TXT ahí te saldrá la última version.

## Enumeración automática

```
droopescan scan drupal --url https://example.com
```

[SamJoan/droopescan: A plugin-based scanner that aids security researchers in identifying issues with several CMSs, mainly Drupal & Silverstripe.GitHub](https://github.com/SamJoan/droopescan)

### Exploits

Para versiones más antiguas de Drupal v8.x < v8.3.9 / v8.4.x < v8.4.6 / v8.5.x < v8.5.1 podemos utilizar Drupalgeddon2

[[Drupalgeddon 2 / CVE-2018-7600 / SA-CORE-2018-002)GitHub](https://github.com/dreadlocked/Drupalgeddon2|GitHub - dreadlocked/Drupalgeddon2: Exploit for Drupal v7.x + v8.x (Drupalgeddon 2 / CVE-2018-7600 / SA-CORE-2018-002)GitHub]]

Y para versiones de más antiguas que 7.58 el Drupalgeddon3, pero necesitas estar autenticado en el dashboard

[[Metasploit)GitHub](https://github.com/rithchard/Drupalgeddon3|GitHub - rithchard/Drupalgeddon3: Drupal < 7.58 - Drupalgeddon 3 Authenticated Remote Code Execution (Metasploit)GitHub]]

## Post Explotación

Una vez explotado si queremos obtener credenciales de las bases de datos de drupal, tenemos que buscar el archivo **settings.php.**

Para buscarlo podemos utilizar este comando:

```
find / -name settings.php -exec grep "drupal_hash_salt\|'database'\|'username'\|'password'\|'host'\|'port'\|'driver'\|'prefix'" {} \; 2>/dev/null
```