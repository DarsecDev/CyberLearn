---
tags:
  - moc
  - fundamentos
---

# 🧠 Fundamentos

Base teórica de ciberseguridad. Cada nota aquí explica **qué es algo, cómo funciona y por qué importa** en seguridad.

## Estructura

| Carpeta | Temas |
|---------|-------|
| `Redes/` | TCP/IP, DNS, HTTP, subredes, OSI, switches, routing |
| `SistemasOperativos/` | Linux (permisos, procesos, systemd), Windows (AD, privilegios) |
| `Criptografia/` | Hashing, cifrado simétrico/asimétrico, PKI, TLS, certificados |
| `Web/` | OWASP Top 10, Same-Origin, CORS, cookies, sesiones |
| `Metodologias/` | PTES, OWASP Testing Guide, Cyber Kill Chain, MITRE ATT&CK |

## Cómo usar esta sección

- Usa la plantilla **[[_Plantillas/Tema Teorico]]** para cada concepto.
- Una nota por concepto. Si un tema es muy grande, divídelo en sub-notas.
- Etiquetas sugeridas: `#fundamento/redes`, `#fundamento/web`, `#fundamento/crypto`
- Enlaza herramientas relacionadas de `03 Herramientas` y recursos de `00 Recursos`.

## Notas existentes

- [[Modelo OSI]] — 7 capas, completo
- [[Modelo TCP IP]] — 4 capas, completo
- [[Protocolo IP]] — IPv4, fragmentación, ruteo
- [[TCP vs UDP]] — comparativa capa de transporte

## Pendiente priorizado

- [x] **Redes**: OSI, TCP/IP, IP, TCP vs UDP — *completado*
- [x] **Redes**: DNS — *completado*
- [ ] **Redes**: HTTP/HTTPS, Subnetting, ARP — *siguiente*
- [ ] **Sistemas**: Linux 101 (permisos, procesos, sudoers) — *esencial para CTF*
- [ ] **Web**: OWASP Top 10 — *esencial para pentest*
- [ ] **Cripto**: Hashing vs cifrado, TLS handshake
- [ ] **Metodologías**: Cyber Kill Chain, MITRE ATT&CK
