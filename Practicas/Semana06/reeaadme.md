# 🟢 Semana 06 — Memoria y recursos en Ubuntu

## 👤 Datos del alumno

- Nombre: Omar Said Duarte Ruiz
- Grupo: Sistemas Operativos (TecNM)
- Fecha: 19 de octubre de 2026


## 💻 Entorno

- Distribución y versión: Ubuntu Linux (Usuario: omar8)
- Equipo físico o máquina virtual: Equipo físico (omar8-Notebook)
- CPU lógicas: 12


## 🎯 Objetivo

Describir qué recursos se observaron y qué se comparó respecto al uso de RAM, memoria virtual, swap, carga del sistema y su comportamiento mediante experimentos controlados.

## 📊 Medición inicial

| Indicador | Resultado | Interpretación |
| --- | --- | --- |
| RAM total | 14Gi | Capacidad física total de memoria detectada en el sistema. |
| RAM disponible | 11Gi | Memoria libre y lista para asignarse inmediatamente a nuevas aplicaciones sin saturar el sistema. |
| Swap total | 4,0Gi | Espacio de intercambio total configurado en el disco. |
| Swap usada | 0B | No hay demanda de intercambio activo en disco. |
| Carga promedio | 0,56, 0,66, 0,36 | Promedio de tareas ejecutables o en espera en los últimos 1, 5 y 15 minutos (a los 6 min de encendido). |

## ⚙️ Procesos observados

- Mayor uso de CPU observado: `/snap/firefox/9036/usr/lib/firefox/firefox` (PID `6140`) con 32.1% de CPU (y pico transitorio de 100% en el comando de consulta `ps`).
- Mayor uso de memoria observado: `/snap/firefox/9036/usr/lib/firefox/firefox` (PID `6140`) con 4.6% de MEM (722184 KB de RSS).
- PID analizado: `9635` (Shell Bash del usuario).
- ¿Qué información consulté en /proc?: Se consultó `/proc/meminfo` para verificar el total y disponibilidad de memoria en kilobytes, y `/proc/9635/status` para auditar el estado, tamaño de memoria virtual (`VmSize`), residente (`VmRSS`) y hilos de la sesión de terminal.


## 🧪 Experimento antes/durante/después

- **Antes:** Se registró la línea base del sistema con `free -h` (11Gi disponibles, 0B de swap usada) y carga estable.
- **Durante:** Se ejecutó un proceso de prueba controlado (`sleep 3600`, PID `2636`). Al verificar con `free -h` y `ps`, el proceso se mantuvo en estado `S` (sleeping) consumiendo 0.0% de CPU y memoria, sin alterar significativamente los recursos del sistema.
- **Después:** Al verificar tras el funcionamiento de la prueba, la RAM disponible aumentó ligeramente a 11Gi por la liberación de cachés del sistema, y la swap continuó en 0B, comprobando que tareas inactivas no generan impacto negativo.


## 🔍 Diagnóstico

Relacionando los hallazgos con los comandos y las evidencias:
- La afirmación de una supuesta lentitud por "90% de RAM llena" queda refutada al comprobar que la columna `disponible` marcaba 11Gi de 14Gi (un ~78% de holgura real).
- Los valores de `si` y `so` en `vmstat` se mantuvieron en `0`, descartando problemas de paginación o thrashing en disco.
- La carga promedio del sistema estuvo muy por debajo del límite de las 12 CPU lógicas (`nproc`), confirmando estabilidad general.


## 💡 Conclusión

Aprendí que en Linux la memoria RAM libre no utilizada es un concepto engañoso, ya que el sistema operativo aprovecha inteligentemente el espacio sobrante para cachés y buffers de rendimiento. Evaluar adecuadamente la columna `disponible` de `free -h`, auditar el directorio virtual `/proc` y analizar procesos mediante PIDs específicos permite realizar diagnósticos técnicos reales y fundamentados, evitando conclusiones erróneas basadas en mitos sobre la saturación de memoria.
