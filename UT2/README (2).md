# UT2 – Introducción a Bash · Bloques 1 y 2

**Alumno/a:** TU NOMBRE  
**Fecha:** 20/10/2026

## Máquina de trabajo

| | |
|---|---|
| **Dónde se escriben y ejecutan** | Máquina virtual **Ubuntu Server** (`ubuntu-aso`) |
| **Editor** | `nano`, directamente en la VM |
| **Ruta en la VM** | `~/ut2/bloque1` y `~/ut2/bloque2` |
| **Repositorio** | Este documento recoge el código completo y la salida de cada script |

## Procedimiento seguido

1. Inicio de sesión en la VM Ubuntu Server.
2. Creación de las carpetas de trabajo:
   ```bash
   mkdir -p ~/ut2/bloque1 ~/ut2/bloque2
   ```
3. Para cada script:
   ```bash
   nano script.sh         # escribir y guardar (Ctrl+O, Enter, Ctrl+X)
   chmod u+x script.sh    # permiso de ejecución
   bash -n script.sh      # comprobar sintaxis (sin salida = correcto)
   ./script.sh            # ejecutar
   ```
4. Comprobación final con `ls -l ~/ut2/bloque1 ~/ut2/bloque2`.

---

## Bloque 1 – Entorno de trabajo y primer script

### `hola.sh`
Saluda al usuario y muestra la fecha, el nombre del equipo y el directorio actual.

**Código** (`~/ut2/bloque1/hola.sh`):

```bash
#!/bin/bash
#
# Nombre:      hola.sh
# Descripción: Mi primer script. Saluda al usuario y muestra la fecha,
#              el nombre del equipo y el directorio actual.
# Autor/a:     TU NOMBRE
# Fecha:       2026-10-20
# Versión:     1.0
# Máquina:     VM Ubuntu Server (ubuntu-aso)
# Uso:         ./hola.sh
#

echo "Hola, $USER. Hoy es $(date +%A), $(date +%d/%m/%Y)."
echo "Estás en el equipo $(hostname)."
echo "Directorio actual: $PWD"

exit 0
```

**Ejecución y salida:**

```bash
./hola.sh
```
```
Hola, tuusuario. Hoy es martes, 20/10/2026.
Estás en el equipo ubuntu-aso.
Directorio actual: /home/tuusuario/ut2/bloque1
```

### `info_sistema.sh`
Resumen del servidor: distribución, kernel, equipo, usuario, tiempo encendido, IP, uso del disco raíz y memoria.

**Código** (`~/ut2/bloque1/info_sistema.sh`):

```bash
#!/bin/bash
#
# Nombre:      info_sistema.sh
# Descripción: Resumen del servidor: sistema, kernel, IP, disco y memoria.
# Autor/a:     TU NOMBRE
# Fecha:       2026-10-20
# Versión:     1.0
# Máquina:     VM Ubuntu Server (ubuntu-aso)
# Uso:         ./info_sistema.sh
#

echo "===== Información del sistema ====="
echo "Sistema:     $(grep PRETTY_NAME /etc/os-release | cut -d'"' -f2)"
echo "Kernel:      $(uname -r)"
echo "Equipo:      $(hostname)"
echo "Usuario:     $USER"
echo "Encendido:   $(uptime -p)"
echo "IP:          $(hostname -I | cut -d" " -f1)"
echo
echo "----- Disco raíz -----"
df -h /
echo
echo "----- Memoria -----"
free -h

exit 0
```

**Ejecución y salida:**

```bash
./info_sistema.sh
```
```
===== Información del sistema =====
Sistema:     Ubuntu 24.04 LTS
Kernel:      6.8.0-xx-generic
Equipo:      ubuntu-aso
Usuario:     tuusuario
Encendido:   up 25 minutes
IP:          192.168.x.x
...
```

**Conceptos:** shebang `#!/bin/bash`, cabecera de documentación, `echo`, sustitución de comandos `$(...)`, permisos con `chmod u+x`, ejecución con `./`, código de salida `exit 0`.

---

## Bloque 2 – Variables, comillas y parámetros

### `variables.sh`
Asignación de variables, uso de llaves `${}`, constantes con `readonly`, tipos de comillas, sustitución de comandos y variables de entorno.

**Código** (`~/ut2/bloque2/variables.sh`):

```bash
#!/bin/bash
#
# Nombre:      variables.sh
# Descripción: Variables, llaves ${}, comillas y sustitución $(...).
# Autor/a:     TU NOMBRE
# Fecha:       2026-10-20
# Versión:     1.0
# Máquina:     VM Ubuntu Server (ubuntu-aso)
# Uso:         ./variables.sh
#

# Asignación: sin espacios alrededor del =
servidor="web01"
puerto=22
readonly UMBRAL=80

echo "Conectando a $servidor por el puerto $puerto"
echo "Fichero de log: ${servidor}_acceso.log"
echo "Umbral de aviso: ${UMBRAL} %"
echo

# Comillas
echo "Dobles:  Hola $USER"
echo 'Simples: Hola $USER'
echo "Barra:   Precio: \$5"
echo

# Variable con espacios: siempre entre comillas
fichero="informe final.txt"
touch "/tmp/$fichero"
echo "Creado: /tmp/$fichero"
rm -f "/tmp/$fichero"
echo

# Sustitución de comandos
hoy=$(date +%F)
usuarios=$(who | wc -l)
ip=$(hostname -I | cut -d" " -f1)
echo "El $hoy hay $usuarios sesiones abiertas en $ip"
echo

# Variables de entorno y especiales
echo "Usuario: $USER  |  Home: $HOME"
echo "Equipo:  $HOSTNAME  |  Directorio: $PWD"
echo "Script:  $0  |  PID: $$"

exit 0
```

**Ejecución y salida:**

```bash
./variables.sh
```
```
Conectando a web01 por el puerto 22
Fichero de log: web01_acceso.log
Umbral de aviso: 80 %

Dobles:  Hola tuusuario
Simples: Hola $USER
Barra:   Precio: $5
...
```

| Comillas | Comportamiento |
|---|---|
| `"..."` dobles | Expanden variables y `$(...)` |
| `'...'` simples | Todo literal, no expanden nada |
| `\` barra invertida | Hace literal el carácter siguiente |

### `parametros.sh`
Muestra los parámetros de posición recibidos y los recorre con `"$@"`.

**Código** (`~/ut2/bloque2/parametros.sh`):

```bash
#!/bin/bash
#
# Nombre:      parametros.sh
# Descripción: Muestra los parámetros que recibe el script.
# Autor/a:     TU NOMBRE
# Fecha:       2026-10-20
# Versión:     1.0
# Máquina:     VM Ubuntu Server (ubuntu-aso)
# Uso:         ./parametros.sh [arg1] [arg2] ...
#

echo "Script:        $0"
echo "Primer arg.:   $1"
echo "Segundo arg.:  $2"
echo "Total:         $#"
echo "Todos:         $*"
echo

n=1
for arg in "$@"; do
    echo "Argumento $n: $arg"
    n=$((n + 1))
done

exit 0
```

**Ejecución y salida:**

```bash
./parametros.sh ana "Recursos Humanos"
```
```
Script:        ./parametros.sh
Primer arg.:   ana
Segundo arg.:  Recursos Humanos
Total:         2
Todos:         ana Recursos Humanos

Argumento 1: ana
Argumento 2: Recursos Humanos
```

| Variable | Significado |
|---|---|
| `$0` | Nombre del script |
| `$1`, `$2`… | Argumentos de posición |
| `$#` | Número de argumentos |
| `$*` / `$@` | Todos los argumentos (`"$@"` respeta cada uno por separado) |
| `$$` | PID del proceso |

### `saludo.sh`
Usa valores por defecto con `${n:-valor}` cuando no se pasan argumentos.

**Código** (`~/ut2/bloque2/saludo.sh`):

```bash
#!/bin/bash
#
# Nombre:      saludo.sh
# Descripción: Saluda y asigna departamento, con valores por defecto.
# Autor/a:     TU NOMBRE
# Fecha:       2026-10-20
# Versión:     1.0
# Máquina:     VM Ubuntu Server (ubuntu-aso)
# Uso:         ./saludo.sh [NOMBRE] [DEPARTAMENTO]
#

nombre=${1:-$USER}
departamento=${2:-"Sistemas"}

echo "Hola, $nombre."
echo "Departamento: $departamento"
echo "Tu cuenta sería: ${nombre}_$(date +%Y)"

exit 0
```

**Ejecución y salida:**

```bash
./saludo.sh
./saludo.sh ana "Recursos Humanos"
```
```
Hola, tuusuario.
Departamento: Sistemas
Tu cuenta sería: tuusuario_2026
Hola, ana.
Departamento: Recursos Humanos
Tu cuenta sería: ana_2026
```

---

## Problemas encontrados y solución

| Problema | Causa | Solución |
|---|---|---|
| `servidor: command not found` | Espacios alrededor del `=` | Escribir `servidor="web01"` |
| Un nombre con espacios crea dos ficheros | Variable sin comillas | Usar siempre `"$fichero"` |
| `Permission denied` | El script no tenía permiso de ejecución | `chmod u+x script.sh` |
| `$'\r': command not found` | Finales de línea CRLF (Windows) | Guardar con LF o usar `dos2unix` |

## Capturas

<!-- Añade aquí tus capturas: ejecución de cada script y salida de ls -l -->

## Uso de IA

<!-- Indica qué partes has hecho con ayuda de una IA y cómo la has usado. -->
