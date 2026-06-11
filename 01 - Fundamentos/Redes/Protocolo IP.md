---
tipo: fundamento
categoria: redes
fuente: CyBOK v1.1.0
fecha: 2026-05-30
estado: completo
tags:
  - fundamento
  - redes
  - ip
---

# Protocolo IP

> [!abstract] El protocolo que se encarga de que los datos lleguen de una IP a otra, aunque no garantiza que lleguen bien.

---

## ¿Qué es?

IP (Internet Protocol) es el que maneja las direcciones y el ruteo. Cuando dices "la IP de la máquina es 10.10.10.10", estás hablando de este protocolo.

Lo clave que me costó entender: **IP no es confiable**. IP agarra los datos, les pone la dirección de origen y destino, y los manda. Si se pierden en el camino, IP no reenvía. Eso lo hace TCP más arriba. IP es como el correo común — mandás la carta y rezá.

## ¿Cómo funciona?

### Header IP (lo básico)

El paquete IP tiene un encabezado con lo esencial:

| Campo | Qué hace |
|-------|----------|
| IP origen | De dónde viene |
| IP destino | Para dónde va |
| TTL | Cuántos saltos puede dar antes de morir |
| Protocolo | Si lo que lleva adentro es TCP (6), UDP (17), ICMP (1)... |
| Checksum | Verifica que el header no esté corrupto |

### Ruteo

Cada router mira la IP destino, revisa su tabla de rutas, y manda el paquete al siguiente router. El TTL baja en cada salto — si llega a 0, se descarta. Esto evita que los paquetes den vueltas para siempre si hay un loop.

### Fragmentación

Si el paquete es más grande de lo que el medio físico soporta (MTU, típicamente 1500 bytes), lo parte en pedazos. Cada pedazo viaja por separado y se rearma en el destino. En IPv4 los routers pueden fragmentar, en IPv6 no — el que manda tiene que calcular el tamaño justo.

## ¿Por qué importa en ciberseguridad?

Porque IP es la base de TODO ataque de red. Si entendés IP, entendés cómo falsear tráfico, cómo interceptarlo o cómo saturar un objetivo.

### Ataques que me parecen importantes

- **IP Spoofing**: Falseás la IP de origen para que parezca que el paquete viene de otro lado. Se usa para evadir ACLs o en DDoS.
- **DDoS por amplificación**: Mandás una consulta chica con IP origen falsa a un servidor DNS/NTP, y la respuesta (mucho más grande) va a la víctima. No sabés quién sos porque la IP está falseada.
- **ICMP Redirect**: Un atacante en la misma red te hace creer que la ruta correcta pasa por él.

### Defensa básica

- Firewalls con filtrado de direcciones IP
- uRPF: el router descarta paquetes que vienen de una ruta que no coincide con la IP origen
- IPSec: cifra paquetes IP enteros para que no los puedan leer ni modificar

## En la práctica

- Cuando usás **Nmap -sS**, estás generando paquetes IP con protocolo TCP
- **tcpdump** te deja filtrar por IP: `tcpdump host 10.10.10.10`
- Con **Scapy** podés construir paquetes IP con la IP origen que quieras (para pruebas)
- En HTB, cuando necesitás **pivoting**, estás re-enrutando paquetes IP a través de un foothold

## Puntos que me costaron

- **IP ≠ confiable**: Pensaba que IP garantizaba entrega como cuando ponés un tracking de envío. No, IP es "best-effort". La confiabilidad la da TCP encima.
- **TTL no es tiempo, son saltos**: El nombre "Time To Live" es re engañoso. No expira por segundos, el contador baja en cada router que atraviesa.
- **IPv6 cambia las reglas**: Ya no hay fragmentación en routers, todo lo maneja el host de origen. Tampoco hay checksum en el header (lo hace la capa de enlace).

## Términos clave

| Término | Lo que entendí |
|---------|---------------|
| TTL | Máximo 255 saltos, baja en cada router, llega a 0 = paquete muere |
| MTU | 1500 bytes en Ethernet, si el paquete es más grande se fragmenta |
| Spoofing | Falsear la IP de origen en un paquete |
| Anycast | Misma IP anunciada desde varios lugares (CDN, DNS root) |

## Relacionado con

- [[Modelo OSI]] 
- [[Modelo TCP IP]] 
- [[TCP vs UDP]] 
- [[Nmap]]

## Recursos

| Recurso | Tipo | Nota |
| ------- | ---- | ---- |
| CyBOK v1.1.0 | Doc | Explica IP, ruteo y ataques comunes |
