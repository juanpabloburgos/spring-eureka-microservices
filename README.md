# Microservicios con Eureka

Tres servicios Spring Boot que se registran en Eureka y se comunican por un API Gateway.

| Servicio | Puerto | Rol |
|---|---|---|
| `eurekaserverservice` | 9099 | Servidor de descubrimiento (Eureka Server) |
| `userservice` | 8086 | Microservicio de usuarios (Eureka Client) |
| `apigateway` | 9056 | Puerta de entrada (Spring Cloud Gateway MVC + Eureka Client) |

## Requisitos
- Java 17+
- No hace falta instalar Maven (cada proyecto trae `mvnw`)

## Arrancar (en este orden, una terminal por servicio)

```powershell
cd eurekaserverservice
.\mvnw spring-boot:run "-Dspring-boot.run.jvmArguments=-Xmx256m"
```
```powershell
cd userservice
.\mvnw spring-boot:run "-Dspring-boot.run.jvmArguments=-Xmx256m"
```
```powershell
cd apigateway
.\mvnw spring-boot:run "-Dspring-boot.run.jvmArguments=-Xmx256m"
```

En Linux/Mac usa `./mvnw` en lugar de `.\mvnw`.

## Probar
- Panel de Eureka: http://localhost:9099 (deben aparecer `USERSERVICE` y `APIGATEWAY` en estado UP)
- Directo al servicio: http://localhost:8086/cliente
- Por el gateway: http://localhost:9056/cliente -> `Welcome to client`

## Notas
- Si el puerto 9099 está ocupado, hay otra instancia de Eureka corriendo:
  `Get-NetTCPConnection -LocalPort 9099 -State Listen` y luego `Stop-Process -Id <PID> -Force`.
- Los servicios cliente deben usar `spring-cloud-starter-netflix-eureka-client`; solo `eurekaserverservice` usa `eureka-server`.
