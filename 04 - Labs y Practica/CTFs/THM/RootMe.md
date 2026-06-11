---
plataforma: TryHackMe
dificultad: Easy
os: Linux
ip: 10.67.148.115
estado: en-progreso
tags:
  - writeup
fecha: 2026-06-10
---

# Untitled

> [!info] Info rápida
> **Plataforma: TryHackMe** | **Dificultad: Easy** | **OS: Linux** | **IP: 10.67.148.115** 

---

## 🎯 Objetivo
*¿Qué hay que conseguir? User flag, root flag, ambas.*
Tengo que conseguir una reverse shell conseguir una user flag y hacer una escala de privilegio para ser root y conseguir otra flag
---

## 🔍 Reconocimiento

### Nmap — Escaneo inicial
nmap -p- --open -sS --min-rate=5000 -v -n -Pn 10.67.148.115 -oG ArchivosPorts   

### Nmap — Escaneo profundo
nmap -p22,80 -sVC 10.67.148.115 -oN Targeted   

### Servicios identificados
| Puerto | Servicio | Versión | Notas                     |
| ------ | -------- | ------- | ------------------------- |
| 22     | ssh      | OpenSSH |                           |
| 80     | http     | Apache  | Panel de subida de archivos |

---

## 🌐 Enumeración

### Web
gobuster dir -u http://10.67.148.115 -w /usr/share/wordlists/dirb/common.txt

### Hallazgos importantes
- /panel/ → formulario de subida de archivos
- /uploads/ → directorio donde se almacenan archivos subidos

---

## 🚪 Foothold

### Vulnerabilidad identificada
**Tipo:** File upload - bypass de extensión
**Por qué funciona:** El panel bloquea .php pero permite .phtml o .php5

### Explotación
Subir shell.phtml con reverse shell a /panel/
Ejecutar: http://10.67.148.115/uploads/shell.phtml

### Acceso obtenido
- **Usuario:** www-data
- **Tipo de shell:** Reverse shell

---

## ⬆️ Escalada de Privilegios

### Enumeración local
find / -perm -4000 2>/dev/null

### Vector encontrado
**Tipo:** SUID (python)
**Por qué funciona:** /usr/bin/python tiene SUID bit activado, permitiendo ejecución como root

### Explotación
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'

### Acceso obtenido
- **Usuario:** root
- **Proof:** whoami → root

---

## 🏁 Flags

| Flag | Hash | Path                 |
| ---- | ---- | -------------------- |
| User |      | home/rootme/user.txt |
| Root |      | /root/root.txt       |

---

## 🛠️ Herramientas usadas
| Herramienta | Para qué la usé |
|-------------|-----------------|
| Nmap        | Escaneo de puertos |
| Gobuster    | Enumeración de directorios web |
| nc/netcat   | Reverse shell listener |

---

## 🧠 Lecciones aprendidas
- Bypass de extensiones en subida de archivos (.phtml, .php5)
- Uso de SUID en Python para escalar privilegios
- Enumeración de binarios SUID con find

## 😤 Dificultades que tuve
- El panel de upload bloquea .php → hay que probar variantes como .phtml, .php5, .php7
- La reverse shell puede no ejecutarse si no tiene los permisos adecuados (chmod +s)

## 🔗 Relacionado con
- [[Subida de archivos]]
- [[SUID privilege escalation]]
- [[Reverse shells]]