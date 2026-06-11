---
tipo: herramienta
categoria: reconocimiento
fuente: https://nmap.org/docs.html
fecha: 2026-05-30
estado: completo
tags:
  - herramienta
  - reconocimiento
  - nmap
---

# Nmap

**Categoría:** Reconocimiento / Escaneo de redes
**Fecha:** 2026-05-30
**Etiquetas:** #herramienta/recon

---

## ¿Qué hace?

Nmap escanea redes para encontrar hosts vivos, puertos abiertos, servicios y sus versiones, y detectar sistemas operativos. Es lo **primero** que tirás en cualquier máquina de HTB o en un pentest.

## Lo básico

```bash
nmap [opciones] <IP>
```

### Escaneos que más voy a usar

```bash
# El combo clásico de HTB (versiones + scripts básicos)
sudo nmap -sC -sV <IP>

# Escaneo de todos los puertos (tarda, pero a veces encontrás joyas)
sudo nmap -p- <IP>

# SYN scan (sigiloso, el más común, necesita sudo)
sudo nmap -sS <IP>

# Ping sweep — descubrir hosts activos en una red
sudo nmap -sn 192.168.1.0/24

# Detectar sistema operativo
sudo nmap -O <IP>

# Escaneo de vulnerabilidades con scripts
nmap --script vuln <IP>
```

## Flags que más voy a usar

| Flag | Para qué sirve |
|------|----------------|
| `-sS` | SYN scan — el más usado con sudo |
| `-sT` | TCP Connect — cuando no tenés sudo, completa el handshake |
| `-sU` | UDP scan — **muy** lento, solo si sabés que hay UDP |
| `-sV` | Detecta versiones de servicios (Apache 2.4.41, OpenSSH 8.9, etc.) |
| `-sC` | Corre los scripts por defecto (safe scripts) |
| `-O` | Detecta sistema operativo |
| `-p-` | Todos los puertos del 1 al 65535 |
| `-p 22,80,443` | Puertos específicos |
| `--top-ports 100` | Solo los 100 más comunes (más rápido) |
| `-Pn` | Saltear ping — tratar al host como activo aunque no responda |
| `-T4` | Velocidad más agresiva (T3 es default) |
| `-oN`/`-oX` | Guardar output en formato normal / XML |
| `-v` / `-vv` | Más detalles en pantalla |
| `-f` | Fragmentar paquetes (para evadir firewalls) |
| `--script` | Correr scripts NSE (vuln, http-enum, smb-enum...) |

## Trucos que fui aprendiendo

- **`-Pn` es OBLIGATORIO en HTB**: Las máquinas de HTB no responden ping. Si no ponés esto, Nmap asume que está caída y no escanea nada.
- **Siempre guardá la salida**: `-oN escaneo.txt` así después podés revisar sin tener que escanear de nuevo.
- **SYN scan (-sS) no queda en logs**: Porque no completa el handshake. El servicio ni se entera que lo escaneaste.
- **Si no tenés sudo, usá `-sT`**: Hace el three-way handshake completo, queda en logs del servicio.
- **Combo con scripts http**: `--script http-enum,http-title,http-headers` en puertos web te ahorra un montón de tiempo.
- **UDP es lentísimo**: Solo lo uso si ya sé que hay un servicio UDP (como DNS o SNMP). Escanear todo en UDP puede tardar horas.

### Ejemplos que uso seguido

```bash
# Full scan para HTB
sudo nmap -sC -sV -p- -Pn -oN full_scan.txt 10.10.10.10

# Escaneo rápido solo de servicios comunes
nmap --top-ports 1000 -Pn -oN quick_scan.txt 10.10.10.10

# Enumeración web rápida
nmap -p 80,443 --script http-enum,http-title,http-headers -Pn 10.10.10.10

# Evadir firewall con fragmentación y decoys
sudo nmap -f -D RND:10 -Pn 10.10.10.10
```

## Relacionado con

- [[Protocolo IP]] — nmap usa paquetes IP para escanear
- [[TCP vs UDP]] — nmap escanea TCP (-sS) y UDP (-sU)
- [[Modelo OSI]] — nmap opera en capas 3 (IP) y 4 (TCP/UDP)
- [[Gobuster]] — para enumerar directorios web después de nmap
- [[ffuf]] — fuzzing web, otro paso después del nmap
