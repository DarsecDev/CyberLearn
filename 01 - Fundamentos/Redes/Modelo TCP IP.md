---
tipo: fundamento
categoria: redes
fuente: CyBOK v1.1.0
fecha: 2026-05-30
estado: completo
tags:
  - fundamento
  - redes
  - tcpip
---

# Modelo TCP/IP

> [!abstract] El modelo de 4 capas que realmente usa internet. Es como el OSI pero comprimido y práctico.

---

## ¿Qué es?

Mientras que OSI es teoría, **TCP/IP es lo que realmente funciona**. Internet entera está construida sobre este modelo. Tiene 4 capas en vez de 7 porque juntó varias de OSI que en la práctica no tienen sentido separadas.

La equivalencia es más o menos así:

```
OSI (7 capas)         →   TCP/IP (4 capas)
──────────────────────────────────────────
Aplicación            \
Presentación           →   Aplicación
Sesión                /
Transporte            →   Transporte
Red                   →   Internet
Enlace                \
Física                 →   Acceso a Red
```

## Las 4 capas

1. **Acceso a Red** — Física + Enlace de OSI. Ethernet, WiFi, MAC. Acá están los switches.
2. **Internet** — Equivale a capa 3. IP, ICMP, ruteo. Acá están los routers.
3. **Transporte** — TCP y UDP. Puertos. Acá se decide si los datos llegan seguros o rápido.
4. **Aplicación** — Sesión + Presentación + Aplicación de OSI. HTTP, DNS, FTP, SSH.

Cuando mandás un mensaje:

```
App (HTTP GET) → Transporte (agrega puerto) → Internet (agrega IP) → Acceso a Red (agrega MAC) → Cable
```

## ¿Por qué importa en ciberseguridad?

Casi todo ataque de red que ves en HTB o en la calle opera sobre TCP/IP. Entenderlo te ayuda a:
- Leer tráfico en Wireshark
- Saber qué podés interceptar o falsear
- Entender el escaneo de Nmap

### Ataques comunes por capa

- **Internet**: IP spoofing (falsear origen), DDoS por amplificación, ICMP redirect
- **Transporte**: SYN flood, secuestro de sesión TCP, escaneo de puertos
- **Aplicación**: SQLi, XSS, DNS poisoning, request smuggling — lo típico de web

## Términos clave

| Término | Lo que entendí |
|---------|---------------|
| Segmento | La unidad de datos en capa Transporte (TCP o UDP) |
| Paquete | La unidad en capa Internet (IP header + datos) |
| Trama | La unidad en capa Acceso a Red (MAC header + datos) |
| Three-way handshake | SYN → SYN/ACK → ACK — cómo TCP establece conexión |

## Puntos que me costaron

- **TCP/IP no es un protocolo, es un stack**: Al principio pensaba que TCP/IP era "un protocolo". No, es un conjunto de protocolos: TCP, IP, UDP, ICMP, ARP, etc. Todos conviven en el stack.
- **ARP vive en el medio**: ARP mapea IPs a MACs... lógicamente está entre Internet y Acceso a Red. No encaja perfecto en una capa.
- **La capa de Aplicación abarca muchísimo**: HTTP, DNS, FTP, SSH, todo eso es "Aplicación". En OSI serían 3 capas separadas. TLS está como entre Transporte y Aplicación (algunos le dicen "capa 4.5").

## En la práctica

- **tcpdump** y **Wireshark** te muestran paquetes en todas las capas TCP/IP
- **Nmap** escanea puertos (capa Transporte) y detecta SO (capa Internet)
- **netcat** te deja mandar datos crudos en cualquier puerto
- Cuando ves `tcpdump -i eth0 host 10.10.10.10`, estás viendo tramas de capa Acceso a Red

## Relacionado con

- [[Modelo OSI]] — la teoría detrás de esto
- [[Protocolo IP]] — la capa Internet
- [[TCP vs UDP]] — los dos protocolos de la capa Transporte
- [[Nmap]] — [[03 - Herramientas/Reconocimiento]] — escanea puertos TCP/UDP

## Recursos

| Recurso | Tipo | Nota |
| ------- | ---- | ---- |
| CyBOK v1.1.0 | Doc | Capítulo Network Security — base de TCP/IP |
| TCP/IP Illustrated (Stevens) | Libro | Me dijeron que es LA referencia, lo tengo pendiente |
