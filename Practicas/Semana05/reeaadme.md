# 🟣 Semana 05 — Procesos en Ubuntu

## 👤 Datos del alumno

- Nombre: Omar Said Duarte Ruiz
- Grupo: Sistemas Operativos (TecNM)
- Fecha: 19 de octubre de 2026

## 💻 Entorno

- Distribución y versión de Ubuntu: Ubuntu Linux (`omar8@omar8-Notebook`)
- ¿Equipo físico o máquina virtual?: Equipo físico
- Terminal utilizada: `/dev/pts/0` (`/bin/bash`)

## 🎯 Objetivo

Explicar qué se investigó y qué se comprobó respecto al ciclo de vida de los procesos, sus estados, prioridades, trabajos en segundo plano y el uso seguro de señales de terminación.

## ⚡ Comandos principales

| Comando | ¿Qué hace? | Resultado observado |
| --- | --- | --- |
| `ps` | Muestra los procesos asociados a la terminal actual. | Listó la shell interactiva `bash` (PID 12722) y el proceso de consulta `ps`. |
| `ps aux` | Muestra una instantánea de los procesos de todo el sistema. | Desplegó los procesos globales de usuarios y del sistema con métricas de CPU y memoria. |
| `pgrep` | Busca o filtra procesos por nombre o patrón. | Localizó identificadores de proceso específicos de forma automatizada. |
| `top` | Muestra y actualiza en tiempo real el uso de recursos y procesos. | Permitió observar dinámicamente la carga de la CPU y la tabla de procesos activos. |
| `jobs -l` | Lista los trabajos controlados por la sesión actual con sus PID. | Mostró los procesos en segundo plano o suspendidos vinculados a la terminal. |
| `nice` | Ejecuta un comando asignándole un valor de prioridad inicial. | Modificó el valor de prioridad relativa (`NI`) del proceso de prueba. |
| `kill -TERM` | Envía una señal de terminación ordenada a un PID específico. | Solicitó al proceso finalizar de forma limpia liberando sus recursos. |

## 🔍 Hallazgos

- PID del proceso de prueba: Identificado mediante las pruebas de ejecución y control de trabajos.
- PPID: Mapeado correctamente al proceso padre correspondiente (por ejemplo, la shell `bash` con PID 12722).
- Estado observado: Predominio de procesos en estado `S` (Sleeping / espera interrumpible) y algunos en ejecución `R`.
- Valor NI: Configurado a `10` mediante el comando `nice`.
- ¿Cómo comprobé su terminación?: Verificando la ausencia del proceso al reejecutar la consulta de verificación por PID.

## 💡 Conclusión

Aprendí a monitorear y gestionar el ciclo de vida de los procesos de manera precisa mediante comandos de terminal. Comprendí por qué no se deben terminar procesos sin identificarlos previamente, ya que una acción incorrecta puede interrumpir servicios críticos del sistema o de otros usuarios, siendo fundamental verificar siempre el PID y PPID antes de aplicar señales de terminación.


