# Guía Completa de ACL en Linux

## 1. ¿Qué son las ACL y para qué sirven?

Las ACL (Access Control Lists / Listas de Control de Acceso) son una extensión del sistema de permisos tradicional de Unix (`rwx` para dueño/grupo/otros). Permiten dar permisos **específicos a usuarios o grupos individuales**, sin depender del modelo rígido de "un dueño, un grupo, todos los demás".

### Casos de uso reales
- Dar acceso de lectura a un usuario puntual sobre un archivo, sin meterlo en ningún grupo.
- Permitir que varios grupos distintos tengan permisos diferentes sobre la misma carpeta (ej: un grupo con solo lectura, otro con lectura/escritura).
- Configurar permisos por defecto para que se apliquen automáticamente a archivos nuevos dentro de una carpeta.
- Dar "tránsito" (solo ejecución) a una carpeta sin dar acceso de lectura general.

---

## 2. Instalación

En sistemas basados en Debian/Ubuntu/Kali:
```bash
sudo apt install acl -y
```

En RedHat/Fedora/CentOS:
```bash
sudo dnf install acl -y
```

Verificar que el sistema de archivos soporta ACL (la mayoría de ext4, xfs y btrfs lo soportan por defecto):
```bash
mount | grep acl
```

---

## 3. Comandos principales

### `getfacl` — ver las reglas ACL

```bash
getfacl archivo.txt
```
Muestra todas las entradas ACL de un archivo o carpeta.

```bash
getfacl -R carpeta/
```
`-R` = recursivo. Muestra ACL de la carpeta y de todo su contenido.

```bash
getfacl -Rs carpeta/ 2>/dev/null
```
`-s` = skip (omite archivos sin ACL configurada, mostrando solo los relevantes). Útil para auditar rápido.

```bash
getfacl -R carpeta/ > respaldo_acl.txt
```
Exporta todas las reglas ACL a un archivo de texto (para respaldo o documentación).

---

### `setfacl` — modificar reglas ACL

#### Formato general
```
setfacl [opciones] regla archivo
```

Donde la regla sigue el patrón: `tipo:entidad:permisos`

| Tipo | Significado |
|---|---|
| `u` o `user` | Usuario específico |
| `g` o `group` | Grupo específico |
| `o` o `other` | Otros (no requiere entidad) |
| `m` o `mask` | Máscara (límite de permisos efectivos) |

#### Dar permisos a un usuario específico
```bash
setfacl -m u:prueba:r archivo.txt        # solo lectura
setfacl -m u:prueba:rw archivo.txt       # lectura y escritura
setfacl -m u:prueba:rwx carpeta/         # lectura, escritura y ejecución
setfacl -m u:prueba:x carpeta/           # solo tránsito (entrar sin listar)
```

#### Dar permisos a un grupo específico
```bash
setfacl -m g:equipo:rw archivo.txt
setfacl -m g:otrogrupo:r archivo.txt
```

#### Quitar todos los permisos (dejando la entrada visible en `0`)
```bash
setfacl -m u:prueba:0 archivo.txt
# equivalente a:
setfacl -m u:prueba:--- archivo.txt
```

#### Eliminar una entrada ACL por completo (ya no aparece en getfacl)
```bash
setfacl -x u:prueba archivo.txt
setfacl -x g:equipo archivo.txt
```

#### Eliminar TODAS las ACL de un archivo (deja solo permisos Unix normales)
```bash
setfacl -b archivo.txt
```

#### Aplicar varias reglas a la vez
```bash
setfacl -m u:prueba:rw,g:equipo:r,o::--- archivo.txt
```

#### Aplicar recursivamente a una carpeta y su contenido
```bash
setfacl -R -m u:prueba:rx carpeta/
```

#### Copiar la ACL de un archivo a otro
```bash
getfacl archivo_origen.txt | setfacl --set-file=- archivo_destino.txt
```

#### Restaurar ACL desde un respaldo
```bash
setfacl --restore=respaldo_acl.txt
```

---

## 4. ACL por defecto (default ACL)

Las ACL normales solo aplican al archivo/carpeta donde las pones. Las **ACL por defecto** se heredan automáticamente por los archivos y subcarpetas **nuevos** que se creen dentro de una carpeta — parecido a lo que hace `setgid`, pero más flexible porque puedes definir permisos por usuario o grupo, no solo el grupo dueño.

### Establecer una ACL por defecto
```bash
setfacl -d -m u:prueba:rw /srv/compartida
```
`-d` = default. A partir de ahora, cualquier archivo nuevo creado dentro de `/srv/compartida` le dará automáticamente lectura/escritura a `prueba`.

### Ver las ACL por defecto de una carpeta
```bash
getfacl /srv/compartida
```
Aparecerán con el prefijo `default:`:
```
default:user::rwx
default:group::rwx
default:user:prueba:rw-
default:other::---
```

### Quitar una ACL por defecto específica
```bash
setfacl -k /srv/compartida
```
`-k` elimina **todas** las ACL por defecto de la carpeta (no las normales, solo las que aplican a archivos futuros).

### Combinar default + setgid (patrón recomendado para carpetas compartidas)
```bash
sudo chmod g+s /srv/compartida
sudo setfacl -d -m g:equipo:rwx /srv/compartida
```
Esto asegura que todo archivo nuevo herede tanto el grupo (`setgid`) como los permisos ACL correctos (`default ACL`), sin que nadie tenga que acordarse de nada.

---

## 5. Entendiendo la salida de `getfacl`

```
# file: contra.txt
# owner: darsec
# group: darsec
user::rw-
user:prueba:r--
group::rw-
mask::rw-
other::r--
```

| Línea | Significado |
|---|---|
| `# file`, `# owner`, `# group` | Metadatos: archivo, dueño, grupo dueño |
| `user::rw-` | Permisos del dueño (sin nombre = es el dueño) |
| `user:prueba:r--` | Permiso ACL específico para el usuario `prueba` |
| `group::rw-` | Permisos del grupo dueño del archivo |
| `mask::rw-` | Límite máximo de permisos efectivos para entradas nombradas (usuarios/grupos específicos vía ACL) |
| `other::r--` | Permisos para todos los demás |

### El `mask` — el más confuso y el más importante

El mask **limita** los permisos efectivos de cualquier entrada ACL nombrada (usuario o grupo específico), sin importar lo que digas al asignarlos. Si el mask es `r--` y le diste `rwx` a un usuario, su permiso **efectivo real** será solo `r--`.

Ver el permiso efectivo real (algunos sistemas lo muestran con un comentario `#effective:`):
```bash
getfacl archivo.txt
```

Recalcular el mask automáticamente al máximo necesario:
```bash
setfacl -m u:prueba:rwx archivo.txt   # esto recalcula el mask automáticamente
```

Fijar el mask manualmente:
```bash
setfacl -m m::rwx archivo.txt
```

---

## 6. El indicador `+` en `ls -l`

Cuando un archivo o carpeta tiene ACL adicional (más allá de los permisos Unix normales), `ls -l` lo marca con un `+` al final de los permisos:

```bash
ls -l contra.txt
-rw-r--r--+ 1 darsec darsec 45 jul  5 13:40 contra.txt
```

Ese `+` es la señal rápida para saber, sin correr `getfacl`, que ese archivo tiene reglas ACL configuradas.

---

## 7. Auditoría — encontrar archivos con ACL en el sistema

### Buscar en carpetas específicas
```bash
sudo getfacl -Rs /home /srv 2>/dev/null
```

### Usando find + ls para detectar el símbolo `+`
```bash
find /home /srv -type f 2>/dev/null | xargs ls -l 2>/dev/null | grep '+'
```

### Guardar un respaldo completo de ACLs (para control de cambios)
```bash
sudo getfacl -R /srv/compartida > /root/respaldo_acl_$(date +%F).txt
```

---

## 8. Tabla resumen de comandos

| Comando | Qué hace |
|---|---|
| `getfacl archivo` | Ver ACL de un archivo |
| `getfacl -R carpeta/` | Ver ACL recursivo |
| `getfacl -Rs carpeta/` | Ver solo archivos con ACL (omite el resto) |
| `setfacl -m u:usuario:permisos archivo` | Agregar/modificar permiso de usuario |
| `setfacl -m g:grupo:permisos archivo` | Agregar/modificar permiso de grupo |
| `setfacl -x u:usuario archivo` | Eliminar entrada ACL de usuario |
| `setfacl -x g:grupo archivo` | Eliminar entrada ACL de grupo |
| `setfacl -b archivo` | Eliminar TODAS las ACL del archivo |
| `setfacl -R -m ... carpeta/` | Aplicar ACL recursivamente |
| `setfacl -d -m ... carpeta/` | Establecer ACL por defecto (herencia) |
| `setfacl -k carpeta/` | Eliminar ACL por defecto |
| `setfacl --set-file=- destino` | Copiar ACL de un archivo a otro |
| `setfacl --restore=archivo.txt` | Restaurar ACL desde respaldo |
| `getfacl origen \| setfacl --set-file=- destino` | Clonar ACL entre archivos |

---

## 9. Buenas prácticas

1. **Usa ACL para excepciones puntuales**, no como reemplazo general de grupos — si varias personas necesitan el mismo acceso de forma permanente, un grupo normal es más simple de mantener.
2. **Documenta tus ACL** con `getfacl -R > respaldo.txt` antes de hacer cambios grandes, así puedes revertir fácilmente.
3. **Revisa el mask** cada vez que asignes permisos — es la causa más común de "le di permiso pero no funciona".
4. **Combina setgid + ACL por defecto** en carpetas compartidas para automatizar la herencia de grupo y permisos.
5. **Ten cuidado con `-R`** (recursivo): puede sobrescribir permisos de muchos archivos a la vez sin previo aviso — revisa antes con `getfacl -R` qué hay actualmente.
6. **No abuses de ACL en todo el sistema** — demasiadas reglas dispersas dificultan la auditoría. Úsalas donde realmente aporten valor (carpetas compartidas, archivos sensibles con excepciones puntuales).
