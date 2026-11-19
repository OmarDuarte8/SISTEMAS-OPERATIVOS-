

### 1. Práctica 09 — `~/SistemasOperativos/semana09/README.md`

```markdown
# 🟢 Semana 09 — Entrada/salida y dispositivos

## Datos del alumno
- Nombre: Omar Said
- Grupo: 1
- Fecha: 2026-11-19

## Entorno
- Distribución y versión: [Salida de cat /etc/os-release]
- Kernel: [Salida de uname -a]
- Equipo físico o máquina virtual: [Equipo físico / Máquina virtual]

## Objetivo
Identificar dispositivos de entrada/salida, USB, PCI y almacenamiento reconocidos por Ubuntu, así como consultar el inventario de hardware y los mensajes del kernel para analizar cómo el sistema detecta e inicializa dispositivos sin realizar cambios destructivos.

## Inventario
| Comando | Propósito | Resultado real |
|---|---|---|
| `lsusb` | Listar dispositivos USB reconocidos y sus identificadores de bus/dispositivo y fabricante/producto | [Pegar resumen o salida de lsusb] |
| `lsusb -t` | Mostrar topología USB en árbol, puertos, controladores y velocidad negociada | [Pegar salida de lsusb -t] |
| `lspci -nn` | Listar dispositivos del bus PCI junto con identificadores numéricos de fabricante y dispositivo | [Pegar resumen o salida de lspci -nn] |
| `sudo lshw -short` | Obtener un resumen compacto del inventario general de hardware | [Pegar resumen de sudo lshw -short] |
| `lsblk -f` | Mostrar dispositivos de bloques, sistemas de archivos, etiquetas, UUID y puntos de montaje | [Pegar salida de lsblk -f] |
| `dmesg` | Consultar mensajes del búfer del kernel (filtrados con `tail` y `grep`) | [Pegar hallazgo principal de dmesg / journalctl -k] |

## Diagnóstico
- Dispositivo seleccionado: [Nombre e ID de fabricante/producto observado]
- Evidencia de detección: [Salida de lsusb, lspci o lsblk donde aparece]
- Mensajes relevantes: [Líneas de dmesg o journalctl -k relacionadas, o indicar si no hubo coincidencia clara]
- Limitaciones: Detectar un dispositivo o ver un mensaje informativo/de advertencia en el kernel no confirma que todas sus funciones estén operativas ni reemplaza la verificación del estado del controlador o de la aplicación.
- Próximo paso recomendado: Verificar permisos, conexión física/lógica y el comportamiento del servicio o aplicación asociada antes de instalar o modificar controladores.

## Conclusión
Aprendí a distinguir entre dispositivos de entrada, salida y entrada/salida, así como entre buses USB y PCI y dispositivos de bloques. También comprendí cómo correlacionar el inventario de hardware con los mensajes del kernel manteniendo siempre un enfoque de observación sin modificar controladores.

```

---

### 2. Práctica 09 — `~/SistemasOperativos/semana09/informe_dispositivos_y_entrada_salida.md`

```markdown
# Informe — Práctica 09: Entrada/salida y dispositivos en Ubuntu

## 1. Descripción del caso y objetivo
Cuando un periférico conectado no responde o su funcionamiento es incierto, es necesario comprobar primero qué reconoce el sistema operativo y qué eventos registra el kernel antes de modificar controladores o configuraciones. El objetivo de esta práctica es consultar dispositivos USB, PCI y de almacenamiento en Ubuntu, obtener un inventario de hardware y analizar mensajes del kernel mediante consultas de solo lectura.

## 2. Definición de entrada, salida, periférico, controlador y kernel
- **Dispositivo de entrada:** Permite introducir información al sistema (por ejemplo, teclado o micrófono).
- **Dispositivo de salida:** Permite presentar información desde el sistema (por ejemplo, pantalla o altavoz).
- **Dispositivo de entrada/salida:** Permite transferir información en ambos sentidos (por ejemplo, interfaces de red o unidades de almacenamiento).
- **Periférico:** Dispositivo conectado al equipo para ampliar sus capacidades.
- **Controlador (`driver`):** Componente de software que permite al sistema operativo comunicarse con un dispositivo y operar sus funciones.
- **Kernel:** Núcleo del sistema operativo que administra recursos y media la interacción entre el software y el hardware.

## 3. Resultados reales de `lsusb`, `lspci`, `lshw` y `lsblk`
### Entorno y almacenamiento (`uname -a`, `cat /etc/os-release`, `lsblk -f`)
```text
[Pegar salidas reales de uname -a, cat /etc/os-release y lsblk -f]

```

### Dispositivos USB (`lsusb` y `lsusb -t`)

```text
[Pegar salidas reales de lsusb y lsusb -t]

```



### Dispositivos PCI (`lspci -nn` y filtros)

```text
[Pegar salidas reales de lspci -nn y lspci | grep -i network / audio]

```


### Inventario general de hardware (`sudo lshw -short` y `sudo lshw -class network`)

```text
[Pegar salidas reales de sudo lshw -short y sudo lshw -class network]

```

## 4. Mensajes relevantes de `dmesg` o `journalctl -k`

```text
[Pegar líneas relevantes de dmesg | tail -n 40, dmesg | grep -i usb | tail -n 30, dmesg | grep -iE 'error|fail|warn' | tail -n 30 o journalctl -k -n 40 --no-pager]

```

Un mensaje informativo o una coincidencia de `error`/`fail`/`warn` no constituye por sí sola un diagnóstico definitivo del problema investigado, ni la ausencia de una línea prueba que no haya ocurrido un evento previo.

## 5. Tabla de observación, interpretación y limitaciones

| Categoría / Evento | Comando / Fuente | Dato observado | Interpretación | Dato pendiente / Limitación |
| --- | --- | --- | --- | --- |
| Almacenamiento | `lsblk -f` | [Dato real] | [Discos/particiones/sistemas de archivos] | No detalla el estado interno del hardware ni otras máquinas |
| USB | `lsusb` / `lsusb -t` | [Dato real] | [Buses, dispositivos y velocidades negociadas] | La velocidad negociada no garantiza rendimiento efectivo de aplicación |
| Bus PCI | `lspci -nn` | [Dato real] | [Componentes internos e IDs numéricos] | Requiere consulta adicional para analizar registros detallados |
| Hardware general | `sudo lshw -short` | [Dato real] | [Resumen del hardware físico o virtual] | En VM parte del hardware es virtualizado |
| Red | `sudo lshw -class network` | [Dato real] | [Interfaz de red detectada] | Detectar la interfaz no confirma conectividad externa |
| Mensajes del kernel | `dmesg` / `journalctl -k` | [Dato real] | [Eventos de detección/inicialización] | El búfer limitado o permisos restringidos pueden limitar el historial |

## 6. Resolución del reto de diagnóstico

Ante un dispositivo USB conectado que el usuario reporta como no funcional, sigo una secuencia no destructiva:

1. **Qué comprobaría primero:** Verificar la presencia del dispositivo con `lsusb` y su topología/velocidad con `lsusb -t`, complementando con `dmesg | grep -i usb | tail -n 30`.
2. **Qué salida demostraría que el sistema detecta el dispositivo:** Una entrada con identificadores de bus/dispositivo y fabricante/producto en `lsusb` (y su rama en `lsusb -t`), acompañada de mensajes de enumeración/conexión en `dmesg`.
3. **Qué salida podría sugerir un problema de inicialización:** Mensajes de desconexión/reconexión repetida, fallos de descriptores o advertencias/errores del controlador USB en `dmesg` o `journalctl -k -n 40 --no-pager`.
4. **Por qué detectar el dispositivo no garantiza que una aplicación pueda utilizarlo:** El kernel puede reconocer el hardware en el bus sin que exista el controlador adecuado, sin que se cargue el módulo correcto, con permisos insuficientes o con la aplicación configurada para otro dispositivo.
5. **Qué paso recomendaría antes de instalar o cambiar un controlador:** Confirmar la conexión física/lógica, revisar si el dispositivo aparece en `lsblk -f` (si es almacenamiento) o en `lsusb`, validar los mensajes del kernel y comprobar si el problema corresponde a permisos o configuración de la aplicación antes de alterar controladores.

## 7. Evidencias insertadas mediante Markdown

* `evidencias/01_sistema_y_almacenamiento.png` — Salidas de `uname -a`, `cat /etc/os-release` y `lsblk -f`.
* `evidencias/02_dispositivos_usb.png` — Salidas de `lsusb` y `lsusb -t`.
* `evidencias/03_dispositivos_pci.png` — Salidas de `lspci -nn` y filtro con `grep -i`.
* `evidencias/04_inventario_hardware.png` — Salidas de `sudo lshw -short` y `sudo lshw -class network`.
* `evidencias/05_mensajes_kernel.png` — Salidas de `dmesg` o `journalctl -k`.

## 8. Conclusión y recomendación administrativa

Consultar `lsusb`, `lspci`, `lshw`, `lsblk` y `dmesg` permite reunir evidencia objetiva antes de intervenir un equipo. La recomendación administrativa es observar y correlacionar los datos disponibles sin desconectar dispositivos activos ni modificar controladores a ciegas, verificando siempre el impacto de cada comprobación.

```

---


