---
tipo: fundamento
categoria: redes
fuente: Professor Messer SY0-701, CyBOK v1.1.0
fecha: 2026-06-08
estado: completo
tags:
  - fundamento
  - redes
  - dns
  - security+
---

# DNS (Domain Name System)

> [!abstract] La agenda telefónica de Internet: traduce nombres que los humanos entendemos (google.com) a IPs que las computadoras entienden (142.250.80.46).

---

## ¿Qué es?

DNS es un sistema jerárquico y descentralizado que resuelve nombres de dominio a direcciones IP (y viceversa). Sin DNS, tendríamos que navegar escribiendo `142.250.80.46` en vez de `google.com`.

Es **uno de los protocolos más críticos** de Internet —y también uno de los más atacados— porque prácticamente todo lo que hacemos en red empieza con una consulta DNS.

## ¿Cómo funciona?

### La resolución paso a paso

Cuando escribís `www.ejemplo.com` en el navegador:

```
1.  Consultás al resolver local (normalmente tu ISP o 8.8.8.8)
2.  El resolver pregunta al root server (.). "¿Quién maneja .com?"
3.  El root responde: "Preguntale a X.X.X.X (TLD server de .com)"
4.  El TLD server responde: "ejemplo.com está en Y.Y.Y.Y (nameserver autoritativo)"
5.  El nameserver autoritativo devuelve: "www.ejemplo.com → 192.0.2.10"
6.  El resolver te pasa la IP. Tu navegador abre conexión.
```

Todo esto pasa en **milisegundos**. Y se cachea para no repetir la cadena entera cada vez.

### Tipos de registro DNS

| Registro | Qué resuelve | Ejemplo |
|----------|-------------|---------|
| **A** | Nombre → IPv4 | `google.com → 142.250.80.46` |
| **AAAA** | Nombre → IPv6 | `google.com → 2607:f8b0:...` |
| **CNAME** | Alias de nombre | `www.ejemplo.com → ejemplo.com` |
| **MX** | Servidor de mail | `@ejemplo.com → mail.ejemplo.com` |
| **TXT** | Texto (verificación, SPF, DKIM) | `v=spf1 include:_spf.google.com` |
| **NS** | Nameserver autoritativo | `ejemplo.com → ns1.ejemplo.com` |
| **PTR** | IP → Nombre (resolución inversa) | `10.0.0.1 → server.ejemplo.com` |
| **SOA** | Autoridad del dominio (admin, serial, refresh) | Información administrativa |

## ¿Por qué importa en ciberseguridad?

DNS es un vector de ataque enorme porque:
1. **Todo lo usa** — si atacás DNS, afectás a todos los servicios
2. **Es difícil de proteger** — el protocolo original no tenía seguridad
3. **Pasa por firewalls** — casi nadie bloquea DNS (puerto 53)

### Vectores de ataque

| Ataque | Cómo funciona | Relevancia Security+ |
|--------|--------------|---------------------|
| **DNS Spoofing / Cache Poisoning** | Metés registros falsos en la caché de un resolver. El usuario escribe `banco.com` y lo redirige a tu server. | ⭐ Alto |
| **DNS Tunneling** | Usás consultas DNS para exfiltrar datos o crear un túnel. Cada consulta lleva un pedacito de info codificado en el subdominio. Los firewalls no lo bloquean porque parece DNS legítimo. | ⭐ Alto |
| **DDoS Amplification** | Mandás una consulta DNS chiquita con IP falsa (la víctima). El servidor DNS responde con un paquete mucho más grande. Amplificación de ~50x. | ⭐ Alto |
| **DNS Hijacking** | Tomás control del nameserver de un dominio o redirigís las consultas a un resolver malicioso. | ⭐ Medio |
| **Domain Shadowing** | Creás subdominios maliciosos en cuentas de registrador legítimas robadas. | ⭐ Medio |
| **Phishing** | Comprás un dominio parecido (typosquatting: g00gle.com, go0gle.com). | ⭐ Alto |

### Mitigación / Defensa

| Defensa | Cómo funciona |
|---------|--------------|
| **DNSSEC** | Firma digitalmente los registros DNS. El resolver verifica que la respuesta sea auténtica. No evita ataques, pero evita spoofing. |
| **DNS over HTTPS (DoH)** | Encripta las consultas DNS en tráfico HTTPS (puerto 443). Evita que tu ISP vea qué sitios visitás. |
| **DNS over TLS (DoT)** | Similar a DoH pero usa TLS dedicado (puerto 853). |
| **DNS Filtering** | Bloqueás dominios maliciosos conocidos (ej: Cisco Umbrella, Quad9). |
| **Rate Limiting** | Limitás consultas por fuente para mitigar DDoS amplification. |

## En la práctica

- **En HTB/CTF**: A veces hay que configurar `/etc/hosts` para resolver dominios de máquinas locales. Siempre revisá si hay subdominios con herramientas como `gobuster dns` o `dnsrecon`.
- **En pentest**: DNS zone transfer (AXFR) para ver si un servidor DNS filtra todos sus registros. `dig axfr @ns1.ejemplo.com ejemplo.com`
- **En SOC**: Revisás logs DNS para detectar C2 (command & control) — los malwares suelen hacer consultas a dominios generados por DGA.
- **En Security+**: Te van a preguntar sobre DNSSEC, tipos de ataque, y cómo mitigarlos. Saber diferenciar spoofing de hijacking es clave.

```bash
# Comandos útiles que uso seguido
dig google.com                    # Consulta estándar
dig -x 8.8.8.8                    # Resolución inversa
dig google.com ANY                # Todos los registros (obsoleto en muchos servers)
dig google.com +short             # Solo la IP
nslookup google.com               # Versión simple
host google.com                   # Versión más simple aún
```

## Términos clave

| Término | Lo que entendí |
|---------|---------------|
| Resolver | El server que hace las consultas por vos (tu ISP, 8.8.8.8, 1.1.1.1) |
| TLD | Top-Level Domain (.com, .org, .io, .ru) |
| Autoritativo | El server que tiene la respuesta "oficial" para un dominio |
| Caché | Guardado temporal de resoluciones para no repetir la consulta |
| TTL | Tiempo que un registro vive en caché antes de pedirlo de nuevo |
| FQDN | Full Qualified Domain Name — el nombre completo con punto al final (`www.google.com.`) |
| DGA | Domain Generation Algorithm — malware genera miles de dominios para C2 |

## Puntos que me costó entender

- **Diferencia entre resolver y autoritativo**: El resolver es el que pregunta. El autoritativo es el que tiene la respuesta. Google DNS (8.8.8.8) es un resolver público. El NS de tu dominio es autoritativo para tu dominio.
- **La jerarquía de roots**: Hay 13 "root servers" (letra A hasta M). Pero no son 13 servidores físicos, son 13 identidades lógicas con cientos de servidores replicados por el mundo (Anycast).
- **DNSSEC no encripta**: Esto me confundió al principio. DNSSEC no cifra nada, solo **firma** los registros para verificar que no fueron modificados. Si querés privacidad, necesitás DoH o DoT.
- **DNS no es solo para web**: Todo lo que usa nombres de dominio usa DNS. SSH, mail, bases de datos, APIs. Si DNS falla, se cae todo.
- **Amplification attack ≠ Spoofing**: En amplification usás IP spoofeada para que la respuesta le llegue a la víctima. La clave es que la consulta es chica y la respuesta es enorme.

## Relacionado con

- [[Protocolo IP]]
- [[Modelo TCP IP]]
- [[Modelo OSI]]
- [[01 - Fundamentos/Web/HTTP]] (cuando lo cree)
- [[Nmap]]

## Recursos

| Recurso | Tipo | Nota |
| ------- | ---- | ---- |
| Professor Messer — DNS | Video | Security+ SY0-701, explica ataques DNS muy claro |
| CyBOK v1.1.0 | Doc | Sección de redes, cubre DNS en profundidad técnica |
| DNS in Detail — TryHackMe | Lab | Habitación gratuita de THM para practicar resolución |
