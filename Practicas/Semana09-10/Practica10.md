### 3. Práctica 10 — `~/SistemasOperativos/semana10/README.md`

```markdown
# 🔵 Semana 10 — Servicios y arranque

## Datos del alumno
- Nombre: Omar Said
- Grupo: 1
- Fecha: 2026-11-19

## Entorno
- Distribución y versión: [Salida de cat /etc/os-release]
- Kernel: [Salida de uname -a]
- Equipo físico o máquina virtual: [Equipo físico / Máquina virtual]

## Servicio investigado
- Nombre de la unidad: [Nombre de la unidad no crítica elegida, p. ej. cron o cups si aplica]
- ¿Existe en el sistema?: [Sí / No, según systemctl status / is-active]
- Estado observado: [Resultado literal de systemctl is-active]
- Habilitación al arranque: [Resultado literal de systemctl is-enabled]
- ¿Se realizó una prueba controlada?: [Sí (con autorización del docente y restauración al estado original) / No (omitida por seguridad o falta de unidad/autorización)]

## Comandos
| Comando | Propósito | Resultado real |
|---|---|---|
| `systemctl --failed` | Listar unidades que systemd reporta en estado fallido | [Pegar salida de systemctl --failed] |
| `systemctl status` | Consultar estado detallado de una unidad específica | [Pegar resumen de systemctl status] |
| `systemctl is-active` | Consultar únicamente si la unidad está activa en este momento | [Resultado literal: active / inactive / etc.] |
| `systemctl is-enabled` | Consultar si la unidad está habilitada para inicio automático | [Resultado literal: enabled / disabled / static / etc.] |
| `journalctl -u` | Consultar registros asociados a la unidad seleccionada | [Pegar hallazgo de journalctl -u] |
| `systemctl get-default` | Consultar el objetivo (`target`) predeterminado de arranque de systemd | [Resultado de systemctl get-default] |

## Diagnóstico
- Hechos observados: [Estado general, unidades fallidas y estado/registros del servicio]
- Hipótesis: [Hipótesis técnica basada en la evidencia observada]
- Evidencia: [Comandos y salidas que respaldan el diagnóstico]
- Acción propuesta: [Acción segura y no disruptiva, o restauración al estado original]
- Verificación: [Comprobación posterior con systemctl status / is-active]

## Conclusión
Aprendí a administrar y consultar servicios con `systemctl` y `journalctl` bajo `systemd`, diferenciando entre el estado actual (`active`/`inactive`/`failed`) y la habilitación al arranque (`enabled`/`disabled`), así como a realizar cambios solo con autorización y restaurando el estado original.

```

---

### 4. Práctica 10 — `~/SistemasOperativos/semana10/informe_servicios_y_arranque.md`

```markdown
# Informe — Práctica 10: Servicios y arranque en Ubuntu

## 1. Descripción del caso y objetivo
Cuando una aplicación o servicio no responde, actuar sin diagnóstico —como reiniciar todo el equipo de inmediato— puede interrumpir procesos o perder pistas del fallo. El objetivo de esta práctica es comprender cómo `systemd` administra servicios y coordina el arranque en Ubuntu, consultar estados y registros con `systemctl` y `journalctl`, y realizar únicamente pruebas controladas y reversibles sobre una unidad no crítica cuando esté autorizado.

## 2. Explicación de servicio, `systemd` y `systemctl`
- **Servicio:** Programa o proceso (habitualmente en segundo plano) que proporciona una funcionalidad al sistema o a otras aplicaciones.
- **`systemd`:** Sistema de inicio y gestor de servicios que inicializa unidades y administra su ciclo de vida en Ubuntu.
- **`systemctl`:** Herramienta de línea de comandos para consultar el estado de `systemd`, inspeccionar unidades y gestionar operaciones permitidas sobre ellas.

## 3. Estado general del sistema y unidades fallidas
- **Tiempo encendido (`uptime`):** [Pegar salida de uptime]
- **Hora de inicio (`who -b`):** [Pegar salida de who -b]
- **Estado de systemd (`systemctl is-system-running`):** [Pegar salida de systemctl is-system-running]
- **Unidades fallidas (`systemctl --failed`):** [Pegar salida de systemctl --failed]
- **Servicios activos (`systemctl list-units --type=service --state=running --no-pager`):** [Pegar resumen o cantidad de servicios activos]

## 4. Servicio elegido y justificación de su selección
Se seleccionó la unidad `[NOMBRE_SERVICIO]` tras consultar unidades existentes y no críticas para la sesión activa (evitando servicios esenciales como red, escritorio, almacenamiento o SSH).
- **Salida de `systemctl status`:** `[Pegar salida]`
- **Salida de `systemctl cat`:** `[Pegar fragmento relevante de la definición de la unidad]`

## 5. Diferencia entre estado actual y habilitación al arranque
- **Estado actual (`active`, `inactive`, `failed`):** Indica la condición de ejecución de la unidad en el momento de la consulta (`systemctl is-active NOMBRE_SERVICIO`: `[resultado]`).
- **Habilitación al arranque (`enabled`, `disabled`):** Indica si la unidad está configurada para activarse automáticamente al iniciar el sistema según los enlaces de `systemd` (`systemctl is-enabled NOMBRE_SERVICIO`: `[resultado]`).
Ambos conceptos son independientes: una unidad puede estar activa ahora pero deshabilitada para el arranque, o habilitada para el arranque pero inactiva en este momento.

## 6. Registros relevantes consultados con `journalctl`
```text
[Pegar salidas relevantes de journalctl -u NOMBRE_SERVICIO -n 40 --no-pager, journalctl -b -n 60 --no-pager y el filtro grep -iE 'error|fail|warn']

```

Que un filtro de palabras clave (`error|fail|warn`) no devuelva coincidencias no garantiza que el servicio esté libre de fallos, ya que algunos eventos no utilizan esos términos.

## 7. Descripción de la prueba controlada (o justificación de omisión)

* **Si se realizó con autorización del docente:**
* **Estado previo:** `is-active` = `[antes]`, `is-enabled` = `[antes]`.
* **Operación ejecutada:** `[sudo systemctl start/stop NOMBRE_SERVICIO]`.
* **Estado posterior:** `is-active` = `[después]`.


* **Si se omitió:** Se omitió la modificación del estado por no contar con una unidad de prueba autorizada y segura, completando el análisis mediante consultas de solo lectura sin afectar servicios esenciales.

## 8. Evidencia de verificación y restauración

Tras la prueba controlada (si se realizó), se restauró la unidad a su estado inicial (`[iniciado/detenido]` según correspondía originalmente) y se verificó con `systemctl status NOMBRE_SERVICIO --no-pager` e `is-active`.

## 9. Explicación del recorrido de arranque

```text
Encendido del equipo
        ↓
Firmware y cargador de arranque
        ↓
Inicio del kernel
        ↓
Inicio de systemd
        ↓
Activación de unidades y servicios (según objetivo predeterminado y dependencias)
        ↓
Sistema disponible para el usuario

```

Este esquema representa el flujo conceptual del arranque; en la práctica `systemd` resuelve dependencias y ejecuta tareas de forma coordinada y en parte concurrente. `systemctl get-default` reportó `[resultado de systemctl get-default]` y `systemctl list-dependencies` mostró las dependencias asociadas a la unidad consultada.

## 10. Resolución del reto de diagnóstico

Ante un servicio de aplicación que no responde, se debe investigar antes de reiniciar todo el servidor (ya que un reinicio general interrumpe todo el sistema y puede ocultar o dificultar el diagnóstico):

1. Revisar `systemctl status NOMBRE_SERVICIO --no-pager`.
2. Comprobar existencia y estado actual con `systemctl is-active NOMBRE_SERVICIO` y `systemctl is-enabled NOMBRE_SERVICIO`.
3. Consultar registros recientes con `journalctl -u NOMBRE_SERVICIO -n 40 --no-pager`.
4. Verificar si hay otras unidades con problemas mediante `systemctl --failed`.
5. Formular una hipótesis respaldada en la evidencia observada y proponer una acción segura (como iniciar o reiniciar únicamente la unidad afectada si está justificado y autorizado), comprobando el resultado y registrando el estado antes y después del cambio. Si la unidad no existe, verificar su nombre exacto o si el paquete/servicio está instalado en lugar de ejecutar comandos a ciegas.


## 12. Conclusión técnica y recomendaciones

Diagnosticar servicios mediante `systemctl` y `journalctl` permite identificar fallos o estados inesperados con precisión y menor impacto que reiniciar todo el sistema. Como buena práctica administrativa, se deben evitar cambios sobre servicios críticos, documentar el estado inicial antes de cualquier prueba autorizada y restaurar siempre el estado original al finalizar.

```

---



```
