---
tags:
  - security+
  - dominio4
  - operaciones
estado: vacio
---

# Dominio 4 — Operaciones de Seguridad

> [!abstract] Pesa **28%** del examen. El que más pesa. Respuesta a incidentes, forense, monitoreo, BCP/DRP.

## Peso en el examen

| Formato | Cantidad aprox |
|---------|---------------|
| Preguntas totales del dominio | ~25-26 |
| Peso relativo | 28% |

## Objetivos del dominio

- [ ] **4.1** — Explicar la importancia de la respuesta a incidentes
- [ ] **4.2** — Explicar el proceso de recolección de evidencia forense
- [ ] **4.3** — Explicar la importancia de las actividades de monitoreo y detección
- [ ] **4.4** — Resumir los conceptos de mejora de la seguridad (hardening, parches)
- [ ] **4.5** — Explicar los conceptos de BCP y DRP
- [ ] **4.6** — Explicar la gestión de identidad y acceso

## Conceptos clave para documentar acá

### Respuesta a incidentes (IR)
- [ ] **Las 6 fases**: Preparation → Identification → Containment → Eradication → Recovery → Lessons Learned
- [ ] Tipos de incidentes, playbooks, SOAR
- [ ] Equipos: CSIRT, SOC, NOC

### Forense digital
- [ ] Cadena de custodia, adquisición de evidencia
- [ ] Orden de volatilidad (RAM → swap → disk → remote)
- [ ] Hash verification, write blocker, imaging (dd, FTK Imager)

### Monitoreo
- [ ] **SIEM** — Log collection, correlación, alerts, dashboards
- [ ] Syslog, SNMP, NetFlow, packet capture
- [ ] EDR, XDR, NDR, MDR
- [ ] Métricas: MTTR, MTBF, MTD, RTO, RPO, SLA

### Backups y resiliencia
- [ ] **Backup 3-2-1**: 3 copias, 2 medios, 1 offsite
- [ ] Full / incremental / differential backup
- [ ] Site types: hot / warm / cold
- [ ] BCP vs DRP, BIA, RTO/RPO

### Gestión de identidad
- [ ] SSO, SAML, OAuth, OpenID Connect
- [ ] MFA (algo que sabés, algo que tenés, algo que sos)
- [ ] LDAP, Active Directory, federación, PKI
- [ ] Password managers, biometrics

### Hardening
- [ ] CIS benchmarks, GPO, SELinux, bastion hosts
- [ ] Immutable systems, golden image

## Conexiones con fundamentos

- [[03 - Herramientas/Reconocimiento/Nmap]] — monitoreo de red

## Mis notas del dominio

*(Agregá links a notas individuales acá)*
- [[]]

---

> [!tip] Es el dominio más pesado (28%). No lo dejes para el final. Las 6 fases de IR y las métricas (RTO/RPO/MTTR) caen seguro.
