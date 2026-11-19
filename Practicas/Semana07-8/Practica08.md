

### 1. Archivo: `README.md`

```markdown
# 🔵 Semana 08 — Sistemas de archivos y laboratorio

## Datos del alumno
- Nombre: Omar Said
- Grupo: [Completar tu grupo]
- Fecha: 2026-11-19

## Entorno
- Distribución y versión de Ubuntu: [Salida de lsb_release -a o /etc/os-release]
- Tipo de máquina: [Máquina virtual / Equipo físico]
- Tipo de red virtual: Red aislada (Host-only / Red interna de VirtualBox)
- IP confirmada de Metasploitable: [IP_OBJETIVO confirmada en el laboratorio]

## Objetivo
Identificar desde la terminal de Ubuntu los dispositivos, sistemas de archivos, espacio disponible, puntos de montaje y configuración de `/etc/fstab`, relacionando los servicios detectados en la Práctica 07 con una inspección informativa no destructiva sobre Metasploitable dentro de la red aislada.

## Inventario de almacenamiento
| Comando | Qué consulta | Hallazgo real |
|---|---|---|
| `lsblk -f` | Dispositivos de bloques, particiones, sistemas de archivos, etiquetas, UUID y puntos de montaje | [Pegar resumen o salida clave de lsblk -f] |
| `df -h` | Tamaño total, espacio usado, disponible, porcentaje y punto de montaje de sistemas de archivos montados | [Pegar resumen o salida clave de df -h] |
| `du -sh` | Espacio ocupado por un directorio y su contenido accesible (`~/SistemasOperativos`, `semana07`, `/var/log`) | [Pegar tamaños reportados por du -sh] |
| `findmnt` | Árbol y estado actual de sistemas de archivos montados (incluidos montajes virtuales) | [Pegar puntos de montaje principales] |
| `cat /etc/fstab` | Definiciones de montaje configuradas para el sistema (lectura sin edición) | [Indicar particiones/UUID observados sin modificar el archivo] |

## Relación con Metasploitable
- Servicio(s) observados en la práctica 07: [Puertos y servicios reportados por nmap -sV --top-ports 100]
- ¿Se autorizó acceso remoto?: [Sí / No, según lo indicado por el docente]
- Consultas realizadas (si corresponde): [whoami, hostname, pwd, df -h, mount; o "Ninguna por falta de credenciales/autorización"]
- Limitaciones: Un puerto abierto o un servicio identificado no confirma vulnerabilidades explotables ni permite asumir la estructura de archivos o permisos del objetivo sin evidencia directa.

## Conclusión
Aprendí a diferenciar disco, partición, sistema de archivos, UUID y punto de montaje, así como a interpretar por qué `df -h` y `du -sh` entregan cifras distintas. También comprendí la importancia de realizar consultas de solo lectura sin modificar configuraciones ni intentar accesos no autorizados dentro de un entorno aislado.

```

---

### 2. Archivo: `informe_sistemas_archivos_y_laboratorio.md`

```markdown
# Informe — Práctica 08: Sistemas de archivos y laboratorio controlado con Metasploitable

## 1. Situación planteada
En la administración de sistemas no basta con identificar qué servicios de red responden; también es indispensable comprender cómo se organiza el almacenamiento, qué dispositivos y sistemas de archivos existen, cuánto espacio está disponible y cómo se relacionan los servicios expuestos con la administración y seguridad del sistema. Esta práctica continúa el trabajo en la red virtual aislada con Ubuntu como estación de análisis y Metasploitable como objetivo autorizado, manteniendo un enfoque estrictamente informativo y no destructivo.

## 2. Explicación de disco, partición, sistema de archivos y punto de montaje
- **Disco / dispositivo:** Medio físico o virtual que aporta almacenamiento de bloques (por ejemplo, un disco virtual en una máquina virtual).
- **Partición:** División lógica de un dispositivo de almacenamiento para separar estructuras o volúmenes.
- **Sistema de archivos:** Estructura lógica que organiza archivos, directorios, metadatos y bloques de datos sobre un dispositivo o partición.
- **Punto de montaje:** Directorio del árbol de archivos de Linux a través del cual se accede al contenido de un sistema de archivos montado.
- **UUID:** Identificador único universal asociado al sistema de archivos/dispositivo, útil para referenciarlo de forma estable sin depender únicamente de nombres cambiantes de dispositivo.
- **Diferencia entre `df -h`, `du -sh` y montar vs. formatear:** `df -h` reporta el espacio a nivel del sistema de archivos montado (capacidad total, usado y libre del volumen), mientras que `du -sh` estima el espacio consumido por los archivos dentro de una ruta específica (sujeto a permisos de lectura). Montar conecta un sistema de archivos ya existente a un directorio; formatear crea una nueva estructura de sistema de archivos y puede destruir el acceso a los datos previos, por lo que no se ejecutan `mkfs`, cambios con `fdisk`/`parted` ni escrituras con `dd`.

## 3. Resultados reales de `lsblk -f`, `df -h` y `du -sh`
### `lsblk -f`
```text
[Pegar aquí la salida de lsblk -f en Ubuntu]

```

### `df -h`

```text
[Pegar aquí la salida de df -h en Ubuntu]

```

### `du -sh` (`~/SistemasOperativos`, `~/SistemasOperativos/semana07` y `/var/log`)

```text
[Pegar aquí las salidas de du -sh]

```

**Por qué no significan exactamente lo mismo:** `df -h` mide el espacio disponible y ocupado en todo el sistema de archivos montado (incluyendo archivos eliminados que aún mantienen descriptores abiertos o espacio reservado), mientras que `du -sh` suma el tamaño de los archivos accesibles dentro de una carpeta concreta.

## 4. Explicación de `blkid`, `findmnt`, `mount` y `/etc/fstab`

* **`blkid`:** Consulta metadatos de dispositivos de bloques reconocidos, como su UUID y tipo de sistema de archivos (`ext4`, `swap`, etc.).
* **`findmnt`:** Muestra de forma estructurada los sistemas de archivos actualmente montados y sus puntos de montaje, incluyendo sistemas virtuales (`proc`, `sysfs`, `tmpfs`, etc.) que no corresponden a particiones físicas de disco.
* **`mount`:** Sin argumentos, lista los sistemas de archivos montados actualmente y sus opciones de montaje.
* **`/etc/fstab`:** Archivo de configuración consultado para montar sistemas de archivos durante el arranque o según reglas definidas. Se leyó únicamente con `cat /etc/fstab` sin modificarlo, ya que un error de sintaxis o de identificador puede impedir el montaje correcto o causar problemas en el inicio del sistema.

## 5. Relación entre los servicios observados y el riesgo administrativo

A partir del inventario obtenido en la Práctica 07 (`~/SistemasOperativos/semana07/reconocimiento_metasploitable.txt`):

* **Servicios observados:** [Listar puertos, protocolos y versiones estimadas por Nmap].
* **Relación con la administración y el riesgo:** Exponer servicios de red o mantener versiones desactualizadas o mal configuradas amplía la superficie de exposición del sistema. Sin embargo, detectar un puerto abierto o el banner de un servicio indica únicamente que hay un proceso escuchando en ese puerto; no demuestra por sí solo que exista una vulnerabilidad explotable ni revela la estructura interna del almacenamiento.

## 6. Registro de acceso a Metasploitable (sin revelar contraseñas)

* **Estado del acceso:** [Indicar si hubo sesión SSH autorizada por el docente o si no se realizó conexión remota por falta de autorización/credenciales/servicio].
* **Consultas de solo lectura ejecutadas (si hubo sesión autorizada):** `whoami`, `hostname`, `pwd`, `df -h`, `mount` (cerrando posteriormente con `exit`).

```text
[Pegar aquí las salidas de las consultas de solo lectura si hubo sesión autorizada, o documentar el motivo por el que no se accedió sin intentar eludir autenticación]

```

* **Nota de seguridad:** En ningún caso se registran contraseñas ni se realizan modificaciones, descargas, cambios de permisos o intentos de eludir controles de acceso.

## 7. Tabla de observación, interpretación y límites

| Observación | Comando/evidencia | Interpretación razonable | Lo que NO puedo afirmar todavía |
| --- | --- | --- | --- |
| Dispositivo o partición observada | `lsblk -f` | [Tu interpretación del layout de discos/particiones] | No puedo afirmar el estado físico interno del hardware ni particiones de otros equipos |
| Espacio disponible | `df -h` | [Tu interpretación del espacio libre/usado en los volúmenes montados] | No puedo asumir que un volumen pequeño esté corrompido o sin margen sin revisar qué contiene |
| Directorio con mayor uso entre los consultados | `du -sh` | [Comparación entre `/var/log`, `~/SistemasOperativos` y `semana07`] | No puedo contabilizar archivos o subdirectorios a los que mi usuario no tenga permiso de lectura |
| Puerto/servicio detectado | Reporte Nmap | [Servicio/versión estimada y su función general] | No puedo afirmar que haya una vulnerabilidad confirmada ni que el sistema haya sido alterado |
| Sistema de archivos del objetivo (si hubo acceso autorizado) | `df -h` / `mount` | [Montajes y espacio observados en Metasploitable, o "No aplica sin acceso autorizado"] | No puedo deducir todos los archivos o permisos del objetivo únicamente por su sistema de archivos |

## 8. Evidencias referenciadas con Markdown

* `evidencias/01_lsblk_df.png` — Captura de `lsblk -f` y `df -h`.
* `evidencias/02_du_findmnt.png` — Captura de `du -sh` y `findmnt` (o `mount`).
* `evidencias/03_fstab_lectura.png` — Lectura de `/etc/fstab` sin edición.
* `evidencias/04_consultas_objetivo_o_analisis.png` — Consultas de solo lectura en el objetivo autorizado o evidencia de análisis/limitaciones.

## 9. Conclusión técnica y medidas prudentes

Conocer el estado de los discos, sistemas de archivos y puntos de montaje permite administrar el almacenamiento con criterio antes de realizar cualquier cambio. Como medidas prudentes, es indispensable trabajar siempre en una red aislada con objetivos autorizados, evitar comandos destructivos o ediciones improvisadas en `/etc/fstab`, limitar las pruebas a consultas informativas de solo lectura y mantener apagados o aislados los entornos vulnerables al concluir la práctica.

```

---



```
