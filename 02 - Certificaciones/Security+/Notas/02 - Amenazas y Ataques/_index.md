---
tags:
  - security+
  - dominio2
  - amenazas
estado: vacio
---

# Dominio 2 — Amenazas, Vulnerabilidades y Mitigaciones

> [!abstract] Pesa **22%** del examen. El dominio más pesado. Tipos de ataques, malware, actores, vulnerabilidades.

## Peso en el examen

| Formato | Cantidad aprox |
|---------|---------------|
| Preguntas totales del dominio | ~19-20 |
| Peso relativo | 22% |

## Objetivos del dominio

- [ ] **2.1** — Comparar y contrastar tipos de amenazas (malware, ingeniería social, DDoS, etc.)
- [ ] **2.2** — Explicar técnicas de ataque (MITM, SQLi, XSS, etc.)
- [ ] **2.3** — Explicar tipos de vulnerabilidades (zero-day, misconfiguration, etc.)
- [ ] **2.4** — Comparar y contrastar actores de amenaza y sus motivaciones
- [ ] **2.5** — Explicar el propósito de las técnicas de inteligencia de amenazas
- [ ] **2.6** — Explicar el ciclo de vida de la gestión de vulnerabilidades

## Conceptos clave para documentar acá

### Malware
- [ ] Virus, Worm, Ransomware, Trojan, Rootkit, RAT, Keylogger, Botnet
- [ ] Fileless malware, polymorphic malware, logic bomb

### Ataques de red
- [ ] DoS / DDoS (+ tipos: SYN flood, ICMP flood, amplification, UDP flood)
- [ ] On-path attack (MITM), replay attack
- [ ] ARP poisoning, DNS poisoning (volver a [[01 - Fundamentos/Redes/DNS|DNS]])
- [ ] Evil twin, rogue AP, VLAN hopping

### Ingeniería social
- [ ] Phishing, spear phishing, whaling, smishing, vishing
- [ ] Pretexting, baiting, tailgating, piggybacking

### Ataques a aplicaciones
- [ ] SQL injection, XSS (reflejado/almacenado/DOM), CSRF/XSRF
- [ ] IDOR, LFI/RFI, command injection, SSRF

### Actores y motivaciones
- [ ] APT, insider threat, hacktivist, script kiddie, organized crime, shadow IT

### Gestión de vulnerabilidades
- [ ] Escaneo, parches, CVSS, responsible disclosure
- [ ] Bug bounty, red team / blue team / purple team

## Conexiones con fundamentos

- [[01 - Fundamentos/Redes/DNS]] — DNS poisoning ya documentado
- [[01 - Fundamentos/Redes/Modelo TCP IP]] — tipos de ataque por capa
- [[01 - Fundamentos/Redes/TCP vs UDP]] — SYN flood, UDP flood

## Mis notas del dominio

- [[Ransomware]]
- [[]]

---

> [!tip] Este dominio conecta directo con lo que ya estudiaste de redes. Empezá por lo que ya conocés (ataques de red) y después expandí a aplicaciones y malware.

---

## Contenido de ejemplo — llenalo a tu manera

### Malware

| Tipo | Qué hace | Cómo llega | Mitigación |
|------|----------|------------|------------|
| **Virus** | Se adjunta a archivos, se replica al ejecutarlos | Email, descargas | Antivirus, EDR |
| **Worm** | Se autoreplica sin intervención humana, explota vulnerabilidades de red | Puertos abiertos, exploits | Parches, segmentación |
| **Ransomware** | Encripta archivos y pide rescate | Phishing, RDP expuesto | Backups 3-2-1, app whitelisting |
| **Trojan** | Parece legítimo pero hace algo malo | Descargas "gratis", email | No ejecutar cosas raras, EDR |
| **Rootkit** | Se esconde en el kernel/modo privilegiado | Exploit de kernel | Secure Boot, parches, reinstalar |
| **RAT** | Acceso remoto encubierto | Troyano, documento malicioso | EDR, monitoreo de conexiones outbound |
| **Keylogger** | Captura teclas | Software o hardware (USB) | MFA |
| **Botnet** | Red de equipos infectados controlados por C2 | Cualquier malware que conecte a C2 | Filtrado DNS, bloqueo de C2, EDR |

### Tipos de atacantes

| Actor | Motivación |
|-------|-----------|
| **APT** | Espionaje estatal, robo de IP |
| **Insider threat** | Venganza, dinero, error accidental |
| **Hacktivist** | Activismo político/social |
| **Script kiddie** | Reputación, usa herramientas ajenas |
| **Organized crime** | Dinero (ransomware, tarjetas) |
| **Shadow IT** | Empleados que usan herramientas no aprobadas |

### Ingeniería social

| Técnica | Cómo funciona |
|---------|--------------|
| **Phishing** | Correo falso genérico |
| **Spear phishing** | Correo falso personalizado |
| **Whaling** | Spear phishing a directivos |
| **Smishing** | Phishing por SMS |
| **Vishing** | Phishing por llamada |
| **Pretexting** | Inventás un escenario para sacar info |
| **Baiting** | Dejás un USB infectado en la recepción |
| **Tailgating** | Entrás detrás de alguien con badge |

### Ataques de red

| Ataque | Cómo funciona |
|--------|--------------|
| **On-path (MITM)** | Interceptás tráfico entre dos partes |
| **Replay** | Capturás y reenviás comunicación |
| **Evil twin** | Clonás un SSID, la víctima se conecta al tuyo |
| **Rogue AP** | AP no autorizado en la red corporativa |
| **ARP poisoning** | ARP falsos para ser MITM en la LAN |
| **VLAN hopping** | Saltás de VLAN por switch spoofing |
| **DoS/DDoS** | Saturación de servicio |
| **DNS poisoning** | Modificás caché DNS para redirigir
