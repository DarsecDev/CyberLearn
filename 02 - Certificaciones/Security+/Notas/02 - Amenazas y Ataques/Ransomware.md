---
tags:
  - securityplus
  - dominio2
  - malware
  - ransomware
estado: borrador
---

# Ransomware

> [!abstract] Malware que encripta archivos y pide rescate económico para liberarlos. La amenaza #1 de ciberseguridad actual.

## ¿Qué es?

Ransomware (= ransom + software) encripta archivos con un cifrado fuerte (AES-256 o similar) y muestra una nota de rescate pidiendo pago en criptomonedas. Los modernos además **exfiltran datos** antes de encriptar (doble extorsión: "pagá o filtramos todo").

Ejemplos conocidos: WannaCry (2017), Ryuk, REvil, LockBit, BlackCat/ALPHV.

## ¿Cómo funciona?

```
1. Infección → phishing mail, RDP expuesto, exploit en software público
2. C2 → el malware contacta al servidor del atacante, recibe clave pública
3. Encriptación → recorre archivos locales, shares de red, backups conectados
4. Nota de rescate → .txt, .html, fondo de pantalla
5. Exfiltración → en ransomware moderno, también roba datos antes de encriptar
```

### ¿Qué encripta?

Documentos, imágenes, bases de datos, backups. **No encripta** archivos del sistema (el sistema tiene que seguir andando para mostrar la nota).

### ¿Por qué es tan efectivo?

- El cifrado es irreversible sin la clave (no hay "decryptor" milagroso)
- Las empresas no pueden operar sin sus datos → pagan
- Las criptomonedas hacen imposible rastrear el pago
- El ransomware-as-a-service (RaaS) permite que cualquiera lo use sin ser hacker

## Vectores de entrada comunes

| Vector | Cómo llega |
|--------|-----------|
| **Phishing con adjunto** | Abrís un Word macro-enabled o PDF malicioso |
| **RDP expuesto** | Escanean puerto 3389, probán contraseñas débiles |
| **Vulnerabilidad pública** | Ej: Log4Shell, exploits en VPNs (Pulse, Fortinet) |
| **Drive-by download** | Visitás un sitio comprometido y se descarga solo |
| **Malvertising** | Anuncios falsos que redirigen a páginas con exploits |

## Mitigación — qué funciona de verdad

| Defensa | Por qué funciona |
|---------|-----------------|
| **Backups 3-2-1** | 3 copias, 2 medios distintos, 1 offsite (y **desconectado**). Si tenés backup, no necesitás pagar. |
| **App whitelisting** | Solo ejecutás software aprobado. El ransomware no puede correr. |
| **Segmentación de red** | Si un equipo se infecta, el ransomware no llega a los shares de red. |
| **Parches** | Muchos ransomware entran por CVS conocidas. Parcheá rápido. |
| **MFA en todo** | Si no hay RDP sin MFA, los ataques de fuerza bruta a RDP no funcionan. |
| **EDR** | Detección de comportamiento anómalo (un proceso leyendo miles de archivos seguido). |
| **Least privilege** | El usuario no tiene permisos de escritura en archivos críticos. |
| **Email filtering** | Bloqueá macros de Office, ejecutables, scripts. |

## En Security+

Te van a preguntar:
- **Qué es ransomware**: elegir la descripción correcta entre varias
- **Cómo mitigarlo**: la respuesta correcta siempre es "backups 3-2-1" o "segmentación"
- **Diferenciar tipos**: ransomware ≠ virus ≠ worm (el ransomware no se autoreplica solo, pero puede combinarse con worm como WannaCry)
- **Doble extorsión**: concepto nuevo en SY0-701, te pueden preguntar qué es

```bash
# Si estás en un incidente de ransomware real
# 1. NO APAGUES LA MÁQUINA (perdés memoria volátil)
# 2. Desconectá el cable de red (cortás C2 y propagación)
# 3. Contactá al equipo de IR
# 4. NO PAGUES (no hay garantía, financiás criminales)
# 5. El backup es tu única esperanza real
```

## Relacionado con

- [[01 - Fundamentos/_index|Fundamentos]] — Criptografía (cifrado simétrico/asimétrico)
- [[02 - Certificaciones/Security+/Vocabulario|Vocabulario]] — RDP, EDR, MFA, C2
