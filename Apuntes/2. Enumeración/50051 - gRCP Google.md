RPC es un sistema de llamada a procedimiento remoto de código abierto desarrollado inicialmente en Google. Utiliza como transporte HTTP/2 y Protocol Buffers como lenguaje de descripción de interfaz.

Para poder conectarte a este servicio necesitaras una herramienta de github la cuál te permite interactuar con los servidores de gRPC a través del navegador. Es algo así como [Postman](https://www.getpostman.com/) , pero para las API de gRPC en lugar de REST.

[GitHub - fullstorydev/grpcui: An interactive web UI for gRPC, along the lines of postmanGitHub](https://github.com/fullstorydev/grpcui)

Link a la aplicación UI

- Necesitas tener instalado go.

## Conectarte al servidor gRCP

Tienes que instalar la aplicación e utilizar el siguiente comando:

```
grpcui -plaintext localhost:50051
```

Pero puedes acceder sin instalar la herramienta utilizando el siguiente comando:

```
go run github.com/fullstorydev/grpcui/cmd/grpcui@latest -plaintext 10.10.11.214:50051
```