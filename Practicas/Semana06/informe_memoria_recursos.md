# 🟢 Informe Técnico — Práctica 06: Memoria y Recursos del Sistema

**Alumno:** Omar Said Duarte Ruiz 
**Materia:** Sistemas Operativos 
**Entorno:** Ubuntu Linux (`omar8@omar8-Notebook`) 

---

## 1. 🎬 Situación inicial
En el trabajo diario con sistemas operativos, es común escuchar la afirmación *"la computadora está lenta porque la memoria RAM está llena"*. Sin embargo, en Linux la memoria no utilizada se aprovecha dinámicamente para cachés y buffers que optimizan el rendimiento del disco. Para determinar la verdadera salud del equipo, es necesario realizar un diagnóstico basado en evidencias mediante comandos del sistema como `free`, `vmstat`, `uptime` y el análisis del directorio `/proc`.

---

## 2. 📚 Explicación de RAM, Memoria Virtual y Swap

* **RAM (Random Access Memory):** Es la memoria física de acceso rápido utilizada directamente por la CPU para ejecutar el kernel, servicios y aplicaciones activas. Es volátil.
* **Memoria Virtual:** Es una abstracción del sistema operativo que asigna direcciones virtuales a los procesos. Permite que cada programa trabaje en un espacio continuo e independiente, mapeando esas direcciones a la RAM física o al espacio de intercambio.
* **Swap (Espacio de Intercambio):** Es un área de almacenamiento utilizada como respaldo cuando la memoria RAM física tiene alta demanda. Permite mover páginas de memoria inactivas al disco. Aunque evita errores de falta de memoria (Out-Of-Memory), el acceso al disco es más lento que a la RAM.

---

## 3. 📊 Tabla de medición inicial

| Indicador | Valor obtenido | Interpretación |
|---|---|---|
| **RAM Total** | `14Gi` | Capacidad física total detectada en la máquina. |
| **RAM Usada (`used`)** | `3,6Gi` | Memoria asignada activamente a procesos. |
| **RAM Libre (`free`)** | `9,0Gi` | Memoria totalmente inactiva. |
| **Buffers / Caché** | `2,9Gi` | Memoria usada para acelerar lectura/escritura de disco. |
| **RAM Disponible (`available`)** | `11Gi` | Estimación real de RAM lista para nuevos programas. |
| **Swap Total / Usada** | `4,0Gi` / `0B` | Estado de la partición/archivo de intercambio. |
| **Carga Promedio** | `0,56, 0,66, 0,36` | Tareas ejecutables/esperando en 1, 5 y 15 min (a los 6 min de encendido). |
| **CPU Lógicas (`nproc`)** | `12` | Núcleos/hilos de procesamiento disponibles en el procesador. |

*Evidencia referenciada: `![Medición Inicial](evidencias/01_free_uptime_nproc.png)`*

---

## 4. 🔍 Interpretación de `free -h`
El comando `free -h` mostró una capacidad total de **14Gi** de RAM. 

* **Punto clave:** La columna **`used`** (3,6Gi) refleja únicamente la memoria asignada a los procesos en ejecución. Linux aprovecha **2,9Gi** en **`búf/caché`** para agilizar operaciones de archivos.
* **Criterio de salud:** El dato crítico para medir disponibilidad es la columna **`disponible`**, que registró **11Gi**, demostrando que el equipo cuenta con amplia holgura de memoria para cualquier tarea sin necesidad de recurrir a la memoria de intercambio (`Inter:` / `Swap`), la cual se mantiene en **0B** usados de **4,0Gi**.

---

## 5. 📈 Interpretación de `vmstat 1 5`
El comando `vmstat 1 5` recopiló 5 muestras de 1 segundo sobre el estado del sistema:

* **Procesos (`r` y `b`):** La columna `r` inició con 2 procesos listos y luego se mantuvo en 0. La columna `b` se mantuvo constante en `0`, confirmando que no existen procesos bloqueados esperando operaciones de E/S.
* **Paginación (`si` y `so`):** Las columnas `si` (swap in) y `so` (swap out) se mantuvieron exactamente en `0` durante todas las lecturas. Esto confirma la ausencia total de intercambio entre RAM y disco.
* **CPU (`us`, `sy`, `id`, `wa`):** El porcentaje de CPU inactiva (`id`) se mantuvo entre **92% y 99%**, demostrando que el procesador se encuentra prácticamente libre.

*Evidencia referenciada: `![Salida vmstat](evidencias/03_vmstat.png)`*

---

## 6. ⚙️ Procesos con mayor consumo de CPU y memoria

Mediante la ordenación de procesos con `ps aux` se obtuvieron los siguientes resultados:

1. **Mayor consumo de Memoria (`ps aux --sort=-%mem`):**
   * **Proceso / Comando:** `/snap/firefox/9036/usr/lib/firefox/firefox` (PID `6140`)
   * **Consumo registrado:** **4.6% MEM** (722184 KB de RSS).
   * **Análisis:** Corresponde al proceso principal del navegador Firefox con múltiples pestañas y procesos secundarios de renderizado asociados.

2. **Mayor consumo de CPU (`ps aux --sort=-%cpu`):**
   * **Proceso / Comando:** `ps aux --sort=-%cpu` (PID `11097`) y `/snap/firefox/9036/usr/lib/firefox/firefox` (PID `6140`)
   * **Consumo registrado:** Pico puntual de **100% CPU** por el propio comando de consulta `ps`, seguido por Firefox con **32.1% CPU**.
   * **Análisis:** El pico de 100% es un instante transitorio durante la evaluación de la tabla de procesos por la shell.

*Evidencias referenciadas:*
* `![Procesos Memoria](evidencias/04_procesos_memoria.png)`
* `![Procesos CPU](evidencias/05_procesos_cpu.png)`

---

## 7. 🧪 Comparación Antes / Durante / Después (Experimento Controlado)

Se realizó una prueba con el proceso en segundo plano `sleep 3600` (PID `2636`):

| Indicador | Antes (`08:27:50`) | Durante (`08:29:46`) | Después (`08:30:46`) | Interpretación |
|---|---|---|---|---|
| **RAM Disponible** | `10Gi` | `10Gi` | `11Gi` | La asignación de memoria se mantuvo estable con ligeras variaciones normales de liberación del sistema. |
| **Swap Usada** | `0B` | `0B` | `0B` | No se requirió transferencia a disco. |
| **Carga Promedio** | `0,99, 0,84, 0,60` | `0,99, 0,84, 0,60` | `1,00, 0,87, 0,63` | Variación mínima en los promedios del planificador. |
| **Estado de `sleep`** | No evaluado | `S` (`0.0% CPU`, `0.0% MEM`) | Presente en `pgrep` con PID `2636` | El proceso permanece suspendido/esperando sin consumir recursos. |

*Evidencias referenciadas:*
* `![Antes](evidencias/06_comparacion_antes.png)`
* `![Durante](evidencias/07_comparacion_durante.png)`
* `![Después](evidencias/08_comparacion_despues.png)`

---

## 8. 🧭 Datos investigados en `/proc`

El sistema de archivos virtual `/proc` expuso los datos internos del kernel:

1. **Análisis de `/proc/meminfo`:** 
   Se registraron exactamente `MemTotal: 15543848 kB` (~14.8 GB) y `MemAvailable: 11796676 kB` (~11.2 GB), confirmando con valores enteros en kilobytes las cifras mostradas por `free -h`. `SwapTotal` y `SwapFree` coincidieron en `4194300 kB` (4 GB).

2. **Análisis de `/proc/9635/status` (Shell Bash):** 
   Al consultar el estado del PID de la shell `bash` se obtuvo:
   * **Name:** `bash`
   * **State:** `S (sleeping)`
   * **Pid / PPid:** `9635` / `9593`
   * **VmSize:** `9096 kB` (Memoria virtual total del proceso)
   * **VmRSS:** `5928 kB` (Memoria física residente consumida)
   * **Threads:** `1`

*Evidencias referenciadas:*
* `![Meminfo](evidencias/02_meminfo.png)`
* `![Status del PID](evidencias/09_proc_pid_status.png)`

---

## 9. 🧩 Diagnóstico del escenario

**Planteamiento:** *Un usuario afirma que su computadora está lenta porque la RAM está al 90%.*

**Diagnóstico basado en la evidencia del equipo:**
1. **Refutación de la hipótesis:** En esta laptop, la memoria disponible es de **11Gi** de un total de **14Gi**, lo que equivale a un **~78% de memoria disponible y libre para uso**.
2. **Evaluación de la carga de trabajo:** Con **12 núcleos de CPU lógicos** (`nproc`), una carga promedio de `1,00` representa apenas una fracción de la capacidad total del procesador.
3. **Ausencia de colas en disco:** Los valores `si` y `so` de `vmstat` en `0` descartan ralentización por thrashing de swap.

---

## 10. 💡 Conclusión y medidas prudentes

* **Conclusión:** La RAM en Linux está diseñada para aprovecharse activamente como caché. Evaluar únicamente la memoria `libre` en lugar de la `disponible` conduce a diagnósticos erróneos.
* **Medidas prudentes recomendadas:**
  1. Utilizar siempre `free -h` observando la columna **`disponible`** y la línea **`Inter:` / `Swap`**.
  2. Usar `ps aux --sort=-%mem` y `ps aux --sort=-%

## 📸 Evidencias descritas

![Medición inicial de uptime](evidencias/uptime.png)
*Figura 1: Medición inicial de los recursos globales del sistema utilizando `free -h` (mostrando 14Gi de RAM total y 11Gi disponibles), el tiempo encendido y carga promedio con `uptime`, y el número de CPU lógicas con `nproc` (12 núcleos).*

![Información de meminfo](evidencias/meminfo.png)
*Figura 2: Consulta detallada de las primeras líneas del archivo del kernel `/proc/meminfo`, mostrando en kilobytes los valores brutos de memoria total (`MemTotal`), libre (`MemFree`), disponible (`MemAvailable`) y swap.*

![Estadísticas con vmstats](evidencias/vmstats.png)
*Figura 3: Monitoreo estadístico del sistema ejecutando `vmstat 1 5`, donde se observa la inactividad de la swap (`si`/`so` en 0) y un porcentaje alto de CPU inactiva (`id` entre 92% y 99%).*

![Procesos ordenados por memoria](evidencias/procesosmemoria.png)
*Figura 4: Listado de procesos ordenados descendentemente por su porcentaje de consumo de memoria RAM con `ps aux --sort=-%mem`, destacando el proceso principal de Firefox con 4.6% de MEM.*

![Procesos ordenados por CPU](evidencias/procesosmemoria.png)
*Figura 5: Listado de procesos ordenados descendentemente por su porcentaje de uso de CPU con `ps aux --sort=-%cpu`, mostrando el pico transitorio de la consulta y la actividad del navegador.*

![Comparación antes](evidencias/comparacionantes.png)
*Figura 6: Registro de la línea base ("Antes") con fecha, comandos `free -h` y muestreo de `vmstat` previo a la ejecución de la prueba controlada.*

![Comparación durante](evidencias/comparaciondurante.png)
*Figura 7: Medición "Durante" la ejecución del proceso de prueba en segundo plano, comprobando con `ps -C sleep` que el PID activo se encontraba en estado de espera (`S`) consumiendo 0% de recursos.*

![Comparación después](evidencias/comparaciondespues.png)
*Figura 8: Verificación "Después" de finalizar el proceso, consultando el estado de la RAM, la carga del sistema y confirmando con `pgrep` la persistencia o cierre del proceso.*

![Estado del PID en proc](evidencias/pidstatus.png)
*Figura 9: Auditoría interna del archivo `/proc/PID/status` filtrando los campos de la shell interactiva `bash`, mostrando su estado `sleeping`, tamaño de memoria virtual (`VmSize`) y residente (`VmRSS`).*
