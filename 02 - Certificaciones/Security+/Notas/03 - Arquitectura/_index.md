---
tags:
  - security+
  - dominio3
  - arquitectura
estado: vacio
---

# Dominio 3 — Arquitectura de Seguridad

> [!abstract] Pesa **18%** del examen. Modelos de confianza, infraestructura, nube, hardening.

## Peso en el examen

| Formato | Cantidad aprox |
|---------|---------------|
| Preguntas totales del dominio | ~16-17 |
| Peso relativo | 18% |

## Objetivos del dominio

- [ ] **3.1** — Explicar modelos de arquitectura (Zero Trust, defense-in-depth)
- [ ] **3.2** — Resumir conceptos de seguridad en la nube
- [ ] **3.3** — Comparar y contrastar conceptos de resiliencia (redundancia, HA, backups)
- [ ] **3.4** — Explicar la importancia de la seguridad física
- [ ] **3.5** — Resumir conceptos de implementación segura de infraestructura

## Conceptos clave para documentar acá

### Modelos de confianza
- [ ] **Zero Trust** — "never trust, always verify". Microsegmentación, NIST 800-207
- [ ] **Defense in depth** — Capas de defensa (física, técnica, administrativa)
- [ ] **Least privilege** — Principio de mínimo privilegio en arquitectura

### Nube
- [ ] IaaS / PaaS / SaaS — qué es cada uno, responsabilidad compartida
- [ ] CASB, SASE, SD-WAN, SDN
- [ ] Cloud deployment models (public, private, hybrid, community)
- [ ] API security, CASB

### Infraestructura de red
- [ ] Firewalls (stateful, stateless, NGFW, WAF), IDS/IPS
- [ ] NAC, HSM, TPM, switches gestionables, VLANs
- [ ] DMZ, VPN, VLAN, segmentation

### Criptografía
- [ ] Simétrico vs asimétrico, hashing, salting
- [ ] **PKI** — CA, RA, certificados, CSR, OCSP, CRL
- [ ] **TLS handshake** — paso a paso, certificados
- [ ] Tipos de cifrado: AES, RSA, ECC, Diffie-Hellman
- [ ] Hashing: SHA-2/3, MD5 (obsoleto), HMAC

### Hardening
- [ ] CIS benchmarks, GPO, SELinux, AppArmor
- [ ] Parches, versiones, deprecación de protocolos inseguros
- [ ] Secure boot, UEFI, measured boot

### Seguridad física
- [ ] Biometría, badges, CCTV, mantrap, locks
- [ ] UPS, generator, HVAC

## Conexiones con fundamentos

- [[01 - Fundamentos/Web/_index]] — HTTP/HTTPS, TLS
- Futuro: Criptografía/ (cuando lo crees)

## Mis notas del dominio

*(Agregá links a notas individuales acá)*
- [[]]

---

> [!tip] Criptografía suele ser de lo más denso. Arrancá con PKI y TLS handshake que seguro caen en el examen.
