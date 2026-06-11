---
tags:
  - moc
  - herramientas
---

# 🛠️ Herramientas

Referencia rápida de comandos y herramientas ordenadas por fase de un pentest.

## Estructura

| Carpeta | Fase | Herramientas ej. |
|---------|------|-------------------|
| `Reconocimiento/` | OSINT, footprinting | Nmap, Gobuster, ffuf, theHarvester |
| `Explotacion/` | Explotación de vulnerabilidades | Metasploit, Burp Suite, SQLMap, Hydra |
| `Post-Explotacion/` | Escalada, persistencia | LinPEAS, Mimikatz, BloodHound, PsExec |
| `Movimiento-Lateral/` | Pivotear entre hosts | CrackMapExec, Chisel, SSH tunneling |
| `Ingenieria-Social/` | Phishing, OSINT | SET, GoPhish, EvilGinx |
| `Movil/` | Android/iOS | MobSF, Frida, APKTool |
| `Wireless/` | Redes inalámbricas | Aircrack-ng, Reaver, Wifite |

## Cómo usar esta sección

- Usa la plantilla **[[_Plantillas/Herramienta o Comando]]**.
- **Una nota por herramienta** o comando (ej: `nmap.md`, `gobuster.md`).
- Pon ejemplos reales que hayas usado en [[04 - Labs y Practica/_index|Labs]].
- Etiqueta con `#herramienta/recon`, `#herramienta/explotacion`, etc.
- Mantén las opciones importantes actualizadas en la tabla.

## Notas existentes

- [[Nmap]] — escaneo de redes, completo

## Pendiente priorizado

- [x] Nmap — *completado*
- [ ] Gobuster / Feroxbuster
- [ ] Burp Suite
- [ ] Metasploit
- [ ] SQLMap
- [ ] LinPEAS / WinPEAS
- [ ] Hydra
