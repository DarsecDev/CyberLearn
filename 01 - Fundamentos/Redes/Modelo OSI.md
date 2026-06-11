---
tipo: fundamento
categoria: redes
fuente: CyBOK v1.1.0, Ciberseguridad - A. Corletti - 2017
fecha: 2026-05-30
estado: completo
tags:
  - fundamento
  - redes
  - osi
---

# Modelo OSI

> [!abstract] Una forma de dividir la comunicación en redes en 7 capas para entender qué pasa cuando mandás datos de un lado a otro.

---

## ¿Qué es?

El modelo OSI lo crearon en los 80 para que todos los fabricantes hablen el mismo idioma en redes. Básicamente agarraron todo el proceso de "mandar datos de una compu a otra" y lo partieron en 7 capas. Cada capa solo se preocupa por su parte, y le pasa el resultado a la de arriba o abajo.

A mí me sirve como mapa mental: cuando veo un ataque o una herramienta, sé en qué capa está operando y eso me ayuda a entender qué puede hacer.

## Las 7 capas (de abajo hacia arriba)

| Capa | Qué hace | Ejemplos |
|------|----------|----------|
| 1 — Física | El cable, la señal, el WiFi | Hubs, repetidores |
| 2 — Enlace | Direcciones MAC, tramas | Switches, ARP |
| 3 — Red | Direcciones IP, ruteo | Routers, IP |
| 4 — Transporte | Puertos, confiabilidad | TCP, UDP |
| 5 — Sesión | Administra la conexión | RPC, NetBIOS |
| 6 — Presentación | Formato, cifrado | TLS/SSL |
| 7 — Aplicación | La interfaz con el usuario | HTTP, DNS, FTP |

Cuando mandás datos, viajan **de arriba a abajo** en el emisor (cada capa le añade su info), viajan por el medio físico, y **de abajo a arriba** en el receptor (cada capa saca su info).

```
Emisor:  7→6→5→4→3→2→1  ──── cable ────→  1→2→3→4→5→6→7  Receptor
         (encapsulación)                      (desencapsulación)
```

## ¿Cómo aplica en ciberseguridad?

Lo que me voló la cabeza fue darme cuenta de que **cada capa se puede atacar distinto**:

- **Capas 1-2 (Física/Enlace)**: Tenés que tener acceso físico. ARP spoofing, MAC flooding.
- **Capa 3 (Red)**: IP spoofing, DDoS. Acá operan los firewalls de red.
- **Capa 4 (Transporte)**: SYN flood, escaneo de puertos. Lo que hace Nmap.
- **Capas 5-6 (Sesión/Presentación)**: Session hijacking, SSL stripping.
- **Capa 7 (Aplicación)**: SQLi, XSS, inyecciones. Lo más común en pentest web.

En la práctica:
- Cuando tiro un **Nmap -sS**, estoy operando en capa 4 (SYN scan)
- Cuando hago **ARP spoofing** con Bettercap → capa 2
- Un **firewall** puede filtrar en capa 3 (IP), capa 4 (puertos) o capa 7 (contenido)

## Términos clave

| Término | Lo que entendí |
|---------|---------------|
| PDU | El nombre que le dan al dato en cada capa (segmento, paquete, trama...) |
| Encapsulación | Cada capa envuelve el dato de arriba con su propio header |
| MTU | Tamaño máximo de un paquete antes de tener que fragmentarlo (1500 bytes) |

## Puntos que me costaron

- **Diferencia capa 2 vs 3**: Capa 2 mueve datos dentro de la misma red (con MAC), capa 3 mueve datos entre redes distintas (con IP). Un switch es capa 2, un router capa 3. Me confundía porque los switches "inteligentes" también hacen cosas de capa 3.
- **OSI es teoría, TCP/IP es práctica**: En el mundo real nadie usa OSI como tal. Se usa TCP/IP que tiene 4 capas. OSI es útil para estudiar y entender conceptos separados, pero las capas 5 y 6 no existen como protocolos separados en la práctica.
- **Al principio me parecía al pedo**: Hasta que entendí que sin este modelo, no podés razonar sobre dónde ocurre cada ataque.

## Relacionado con

- [[Modelo TCP IP]] 
- [[Protocolo IP]] 
- [[TCP vs UDP]] 
- [[Nmap]]

## Recursos

| Recurso | Tipo | Nota |
| ------- | ---- | ---- |
| Ciberseguridad — A. Corletti | Libro | Capítulo 2: El modelo OSI con ejemplos recontra claros |
| CyBOK v1.1.0 | Doc | Fundamentos de redes, medio técnico pero completo |
