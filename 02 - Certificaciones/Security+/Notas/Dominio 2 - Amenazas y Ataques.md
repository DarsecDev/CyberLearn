---
tags:
  - securityplus
  - dominio2
  - amenazas
estado: borrador
---

# Dominio 2 — Amenazas, Vulnerabilidades y Mitigaciones

> [!abstract] Pesa **22%** del examen. Es el dominio más grande y donde más preguntas de "elegí la mejor defensa" caen.

## Malware

| Tipo           | Qué hace                                                                | Cómo llega                         | Mitigación                                 |
| -------------- | ----------------------------------------------------------------------- | ---------------------------------- | ------------------------------------------ |
| **Virus**      | Se adjunta a archivos, se replica al ejecutarlos                        | Email, descargas                   | Antivirus, EDR                             |
| **Worm**       | Se autoreplica sin intervención humana, explota vulnerabilidades de red | Puertos abiertos, exploits         | Parches, segmentación                      |
| **Ransomware** | Encripta archivos y pide rescate                                        | Phishing, RDP expuesto             | Backups 3-2-1, app whitelisting            |
| **Trojan**     | Parece legítimo pero hace algo malo                                     | Descargas "gratis", email          | No ejecutar cosas raras, EDR               |
| **Rootkit**    | Se esconde en el kernel/modo privilegiado                               | Exploit de kernel                  | Secure Boot, parches, reinstalar           |
| **RAT**        | Acceso remoto encubierto                                                | Troyano, documento malicioso       | EDR, monitoreo de conexiones outbound      |
| **Keylogger**  | Captura teclas                                                          | Software o hardware (USB)          | Autenticación MFA (no depende del teclado) |
| **Botnet**     | Red de equipos infectados controlados por C2                            | Cualquier malware que conecte a C2 | Filtrado DNS, bloqueo de C2, EDR           |

> [!tip] En el examen te van a dar un escenario ("un usuario descargó un archivo y ahora los archivos tienen extensión .encrypted") y tenés que elegir qué malware es y cómo mitigarlo.

## Tipos de atacantes y motivaciones

| Actor                                | Motivación                                     |
| ------------------------------------ | ---------------------------------------------- |
| **APT** (Advanced Persistent Threat) | Espionaje estatal, robo de IP                  |
| **Insider threat**                   | Venganza, dinero, error accidental             |
| **Hacktivist**                       | Activismo político/social                      |
| **Script kiddie**                    | Reputación, diversión, usa herramientas ajenas |
| **Organized crime**                  | Dinero (ransomware, robo de tarjetas)          |
| **Shadow IT**                        | Empleados que usan herramientas no aprobadas   |

## Ingeniería social

- **Phishing**: correo falso genérico
- **Spear phishing**: correo falso personalizado para alguien específico
- **Whaling**: spear phishing a directivos/CEOs
- **Smishing**: phishing por SMS
- **Vishing**: phishing por llamada telefónica
- **Pretexting**: inventás un escenario para sacar info
- **Baiting**: dejás un USB infectado en la recepción
- **Tailgating**: entrás detrás de alguien con badge

> [!tip] Common sense + MFA rompe casi todos estos ataques.

## Ataques de red

Ya cubrí algunos en [[01 - Fundamentos/Redes/DNS|DNS]] (spoofing, hijacking, tunneling, amplification). Sumá estos:

| Ataque                    | Cómo funciona                                                               |
| ------------------------- | --------------------------------------------------------------------------- |
| **On-path attack** (MITM) | El atacante intercepta y modifica tráfico entre dos partes                  |
| **Replay attack**         | Capturás una comunicación y la reenviás después                             |
| **Evil twin**             | Clonás un SSID legítimo y la víctima se conecta al tuyo                     |
| **Rogue AP**              | Ponés un AP no autorizado en la red corporativa                             |
| **ARP poisoning**         | Enviás ARP falsos para ser el MITM en la LAN                                |
| **VLAN hopping**          | Saltás de una VLAN a otra por switch spoofing o double tagging              |
| **DoS/DDoS**              | Saturás un servicio (SYN flood, ICMP flood, amplification)                  |
| **DNS poisoning**         | Modificás la caché DNS para apuntar a IP maliciosa — ya lo cubrí en [[DNS]] |

