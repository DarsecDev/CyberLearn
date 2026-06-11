---
tipo: fundamento
categoria: redes
fuente: CyBOK v1.1.0
fecha: 2026-05-30
estado: completo
tags:
  - fundamento
  - redes
  - tcp
  - udp
  - transporte
---

# TCP vs UDP

> [!abstract] TCP es confiable pero lento, UDP es rápido pero se le pueden perder datos. Cada uno se usa para cosas distintas.

---

## ¿Qué es?

TCP y UDP son los dos protocolos de la **capa de Transporte**. Trabajan encima de IP y se encargan de que los datos lleguen de una **aplicación** a otra usando **puertos**. La gran diferencia: TCP checkea que todo llegue bien, UDP manda y se olvida.

## TCP — Cuando importa que no se pierda nada

TCP establece una conexión antes de mandar datos (el famoso **three-way handshake**):

```
Cliente                     Servidor
  │                            │
  │───── SYN ────────────────→│  "¿Estás ahí?"
  │←─── SYN/ACK ─────────────│  "Sí, estoy. ¿Vos?"
  │───── ACK ───────────────→│  "Sí, empecemos"
  │═════ Datos ══════════════│  (handshake hecho, va la data)
```

Después de eso:
- Cada paquete tiene un número de secuencia
- El receptor manda un ACK por cada paquete que recibe
- Si el emisor no recibe ACK, reenvía
- Si hay congestion, TCP reduce la velocidad

**Para qué sirve**: Web (HTTP/HTTPS), email (SMTP), FTP, SSH — cosas donde no querés perder ni un byte.

## UDP — Cuando importa la velocidad

UDP no hace handshake, no espera confirmación, no reenvía perdidos. Solo manda:

```
Cliente                     Servidor
  │                            │
  │───── Datos ──────────────→│  "Tomá"
  │───── Datos ──────────────→│  "Tomá"
  │───── Datos ──────────────→│  (total, si se pierde uno, next)
```

**Para qué sirve**: DNS (una consulta rápida), VoIP (si perdés 0.1s de audio, no pasa nada), streaming en vivo, gaming online.

## Comparación rápida

| | TCP | UDP |
|---|---|---|
| Conexión | Hace handshake primero | Manda directo |
| ¿Llegan todos? | Sí, reenvía si es necesario | No garantiza |
| Velocidad | Más lento (mucho overhead) | Rápido |
| Header | 20 bytes | 8 bytes |
| Orden | Mantiene orden | Pueden llegar desordenados |
| Se usa en | Web, email, SSH, FTP | DNS, VoIP, video streaming, gaming |

## ¿Por qué importa en ciberseguridad?

Según qué protocolo use un servicio, sabés qué tipo de ataque aplicar.

### TCP se ataca así:

- **SYN flood**: Mandás solo SYN sin completar el handshake. El servidor se llena de conexiones a medio abrir y no acepta más.
- **TCP sequence prediction**: Si podés adivinar el número de secuencia, podés inyectar paquetes falsos en una conexión (session hijacking).
- **RST attack**: Mandás un paquete falso con RST para matar una conexión legítima.

### UDP se ataca así:

- **UDP flood**: Mandás montones de datagramas UDP a puertos random. El servidor responde ICMP "Destination Unreachable" por cada uno hasta saturarse.
- **Amplificación DNS**: Mandás una consulta chiquita con IP falsa a un DNS, y la respuesta (grande) va a la víctima. Como es UDP, no verificás la IP origen.

### Escaneo:

- TCP se escanea rápido con `nmap -sS` (SYN scan)
- UDP es lentísimo (`nmap -sU`) porque tenés que esperar timeout para saber si un puerto está cerrado

## En la práctica

- **HTTP** → TCP/80, **HTTPS** → TCP/443
- **DNS** → UDP/53 (pero también puede usar TCP/53 para transferencias grandes)
- **VPN** (OpenVPN, WireGuard) → UDP, porque TCP sobre TCP es problemático
- **Nmap -sS** escanea TCP, **nmap -sU** escanea UDP

## Puntos que me costaron

- **UDP no es "inseguro"**: Pensaba que era inseguro porque no tiene handshake. No confundir: la seguridad la da TLS (que va encima de TCP). UDP es poco confiable nomás.
- **TCP también se puede falsear**: Aunque tiene handshake y números de secuencia, si podés sniffear el tráfico podés predecir los números e inyectar paquetes.
- **DNS usa los dos**: Las consultas normales van por UDP (rápido), pero si la respuesta es muy grande o es una transferencia de zona, usa TCP. Eso me confundió al principio.

## Relacionado con

- [[Protocolo IP]] — TCP y UDP viajan dentro de paquetes IP
- [[Modelo OSI]] — capa 4 de Transporte
- [[Modelo TCP IP]] — capa Transporte del modelo práctico
- [[Nmap]] — [[03 - Herramientas/Reconocimiento]] — escanea ambos

## Recursos

| Recurso | Tipo | Nota |
| ------- | ---- | ---- |
| CyBOK v1.1.0 | Doc | Capítulo Network Security — protocolos de transporte |
