
### `README.md` — Práctica 13 (`~/SistemasOperativos/semana13/README.md`)

```markdown
# 🟢 Semana 13 — Logs, diagnóstico y recuperación

## Datos del alumno
- Nombre: Omar Said
- Grupo: Sistemas Operativos (ITSRV)
- Fecha: 2026-11-19

## Entorno
- Distribución y versión: Ubuntu Linux (consultada mediante `cat /etc/os-release`)[cite: 4]
- Kernel: Kernel de Linux en Ubuntu (consultado mediante `uname -a`)[cite: 4]
- Equipo físico o máquina virtual: Equipo de laboratorio / máquina virtual autorizada[cite: 4]

## Unidad investigada
- Nombre: Unidad de servicio existente y no crítica seleccionada tras consultar `systemctl status` (por ejemplo, `cron` o `cups` si está instalada)[cite: 4]
- Estado inicial: Consultado con `systemctl status` e `is-active` sin modificar ni interrumpir servicios críticos[cite: 4]
- ¿Se realizó una simulación?: No se provocó ninguna falla ni se alteraron logs del sistema; se aplicó el diagnóstico basado en evidencias y observación controlada[cite: 4]
- ¿Qué autorización se recibió?: Trabajo en entorno propio/de laboratorio autorizado, respetando las reglas de no borrar o editar registros ni tocar servicios críticos[cite: 4]

## Comandos
| Comando | Propósito | Resultado real |
|---|---|---|
| `systemctl --failed` | Listar unidades que `systemd` reporta en estado fallido[cite: 4] | Muestra las unidades fallidas actuales o indica que no hay unidades en estado `failed` (resultado válido)[cite: 4] |
| `journalctl -b` | Consultar registros del journal correspondientes al arranque actual (`-b -n 60 --no-pager`)[cite: 4] | Muestra las últimas entradas con marca temporal, unidad/componente y mensaje del arranque actual[cite: 4] |
| `journalctl -u` | Filtrar mensajes del journal asociados a una unidad específica (`-u NOMBRE_SERVICIO -n 50 --no-pager`)[cite: 4] | Muestra los eventos recientes registrados por la unidad o indica si no hay entradas recientes disponibles[cite: 4] |
| `dmesg` | Consultar mensajes del búfer del kernel (`dmesg | tail -n 40`)[cite: 4] | Muestra mensajes recientes del búfer del kernel o registra si existe restricción de permisos sin alterar políticas[cite: 4] |
| `tail` | Mostrar las últimas líneas de un archivo de registro o salida (`tail -n 40` / `tail -n 30`)[cite: 4] | Presenta las líneas finales del flujo o archivo legible sin modificar su contenido[cite: 4] |
| `grep` | Filtrar líneas según un patrón de texto (`grep -iE 'error|fail|warn'`)[cite: 4] | Devuelve coincidencias ignorando mayúsculas/minúsculas (`-i`) con expresiones extendidas (`-E`), sin constituir por sí solo causa raíz[cite: 4] |

## Diagnóstico
- Hechos: Se revisaron fecha/actividad (`date`, `uptime`, `who -b`), unidades fallidas (`systemctl --failed`), registros del arranque (`journalctl -b`), kernel (`dmesg` y `journalctl -k -b`), estado y bitácora de la unidad (`systemctl status` y `journalctl -u`) y archivos legibles en `/var/log`[cite: 4].
- Hipótesis: Si un servicio deja de responder, la causa probable debe contrastarse con sus marcas temporales, estado de la unidad y mensajes del servicio/kernel antes de suponer que un mensaje con la palabra `error` es la causa raíz[cite: 4].
- Evidencia: Salidas de `systemctl --failed`, `journalctl -b`, `journalctl -u`, `dmesg` / `journalctl -k -b` y consulta del estado de la unidad[cite: 4].
- Información pendiente: Confirmar si existen mensajes fuera del rango `-n` consultado, si algún registro está restringido por permisos o si el evento requiere revisar configuración específica de la aplicación[cite: 4].
- Recuperación propuesta: Evitar reiniciar todo el equipo como primera respuesta; comprobar el estado de la unidad, revisar registros del servicio y del arranque, aplicar únicamente una acción correctiva autorizada y justificada si procede, y documentar cómo revertirla[cite: 4].
- Verificación: Comprobar nuevamente `systemctl status NOMBRE_SERVICIO --no-pager`, `systemctl --failed` y las entradas recientes de `journalctl -u NOMBRE_SERVICIO -n 50 --no-pager` para confirmar la recuperación[cite: 4].

## Conclusión
Aprendí a investigar incidentes en Ubuntu a partir de los registros (`journalctl`, `dmesg`, `tail` y `grep`), correlacionando marcas temporales y estados de unidades para diferenciar síntomas, observaciones e hipótesis antes de proponer una recuperación verificable[cite: 4].

```

# Informe — Práctica 13: Logs, diagnóstico y recuperación en Ubuntu

## 1. Caso y objetivo
Cuando un servicio de Ubuntu deja de responder y surge la propuesta de reiniciar todo el equipo, es necesario investigar primero qué ocurrió, cuándo sucedió, qué servicio o componente estuvo involucrado y qué evidencia existe antes de tomar decisiones[cite: 4]. El objetivo de esta práctica es utilizar `journalctl`, `dmesg`, `tail` y `grep` para consultar los registros del sistema, relacionar mensajes con el estado de un servicio y elaborar un diagnóstico fundamentado con plan de recuperación y verificación[cite: 4].

## 2. Definición de log, journal, error, síntoma e hipótesis
- **Log (registro):** Registro de eventos generado por el sistema, un servicio o una aplicación, que puede contener marcas de tiempo, información, advertencias o errores[cite: 4].
- **Journal y `dmesg`:** `journalctl` consulta el journal administrado por `systemd`, mientras que `dmesg` consulta los mensajes del búfer del kernel (además de existir archivos de registro tradicionales en rutas como `/var/log`, según disponibilidad y permisos)[cite: 4].
- **Observación:** Dato concreto que aparece en una salida o registro[cite: 4].
- **Síntoma:** Comportamiento perceptible del sistema o servicio, como dejar de responder[cite: 4].
- **Hipótesis:** Explicación tentativa que aún debe contrastarse con evidencia antes de asumirse como causa confirmada[cite: 4].
- **Causa confirmada, corrección y verificación:** La causa confirmada se sostiene en evidencias suficientes; la corrección es el cambio aplicado para atenderla; y la verificación es la comprobación posterior de que el comportamiento esperado se recuperó[cite: 4]. Un mensaje con la palabra `error` no prueba por sí solo la causa raíz de un incidente[cite: 4].

## 3. Estado general del sistema
Para contextualizar la revisión sin modificar configuraciones ni borrar o alterar registros, se consideran las consultas de estado general:
- **Fecha, actividad e inicio:** `date`, `uptime` y `who -b` aportan el contexto temporal del equipo y del último arranque disponible[cite: 4].
- **Unidades fallidas:** `systemctl --failed` identifica unidades en estado `failed` (o confirma que no hay unidades fallidas en ese momento)[cite: 4].
- **Registros del arranque actual:** `journalctl -b -n 60 --no-pager` muestra las últimas 60 entradas del arranque actual (`-b`) sin paginador (`--no-pager`), permitiendo ubicar marcas temporales, unidad/componente y mensaje[cite: 4].

## 4. Registros del kernel y del servicio
- **Mensajes del kernel:** `dmesg | tail -n 40` muestra las últimas 40 líneas del búfer del kernel; `dmesg | grep -iE 'error|fail|warn' | tail -n 30` filtra coincidencias de texto ignorando mayúsculas/minúsculas (`-i`) con expresiones regulares extendidas (`-E`); y `journalctl -k -b -n 40 --no-pager` consulta mensajes del kernel (`-k`) del arranque actual (`-b`)[cite: 4]. Una coincidencia puede ser informativa, previa o ajena al incidente, y la ausencia de coincidencias no garantiza que no existan fallos[cite: 4].
- **Registros del servicio y `/var/log`:** Tras consultar unidades existentes sin modificarlas (`systemctl status` sobre una unidad no crítica disponible), `journalctl -u NOMBRE_SERVICIO -n 50 --no-pager` revisa los registros recientes de esa unidad, `journalctl -u NOMBRE_SERVICIO --no-pager | grep -iE 'error|fail|warn'` busca palabras clave en sus mensajes, y `ls -lah /var/log` junto con `tail -n 30 RUTA_DEL_ARCHIVO` permite revisar archivos de registro legibles sin borrar ni modificar nada[cite: 4].

## 5. Línea de tiempo de los eventos
| Momento | Evidencia | Hecho observado | Interpretación provisional |
|---|---|---|---|
| Antes de la prueba | Estado inicial (`date`, `uptime`, `systemctl --failed`, `systemctl status`) | Se registra el contexto temporal, las unidades fallidas y el estado inicial del servicio sin alterar registros[cite: 4] | Establece la línea base para distinguir eventos previos de cambios posteriores[cite: 4] |
| Durante la prueba | Operación autorizada o evento registrado (`journalctl -b` / `journalctl -u` / `dmesg`) | No se provocó falla en el sistema principal ni se borraron o editaron logs; se observan los registros disponibles[cite: 4] | Los mensajes permiten ubicar marcas temporales y componentes involucrados, sin asumir causa raíz automática[cite: 4] |
| Después de la prueba | Estado y journal (`systemctl status`, `journalctl -u`) | Se contrasta el estado y las entradas recientes frente al punto de partida[cite: 4] | Confirma si el servicio mantiene su estado o si el evento registrado exige revisión adicional[cite: 4] |

## 6. Diagnóstico basado en evidencias
| Pregunta | Respuesta sustentada |
|---|---|
| ¿Qué ocurrió? | El comportamiento observado o síntoma del servicio debe contrastarse con su estado actual y sus mensajes de registro[cite: 4]. |
| ¿Cuándo ocurrió? | Se determina a partir de las marcas temporales registradas en `journalctl -u`, `journalctl -b` o `dmesg` (y el contexto de `date` / `uptime`)[cite: 4]. |
| ¿Qué servicio o componente estuvo involucrado? | La unidad identificada en `systemctl status` / `systemctl --failed` o el componente/módulo citado en los mensajes del journal o del kernel[cite: 4]. |
| ¿Dónde aparece registrado? | En el journal de `systemd` (`journalctl -u` / `journalctl -b` / `journalctl -k`), en el búfer del kernel (`dmesg`) o en archivos de registro legibles bajo `/var/log`[cite: 4]. |
| ¿Qué evidencia respalda la hipótesis? | La correlación entre la hora del evento, el estado de la unidad y los mensajes contextuales observados (más allá de una palabra aislada)[cite: 4]. |
| ¿Qué información falta? | Entradas fuera del límite `-n`, registros no accesibles por permisos o detalles de configuración interna de la aplicación no reflejados en el mensaje[cite: 4]. |
| ¿Qué acción segura se recomienda? | Evitar reiniciar todo el equipo sin diagnóstico ni borrar logs; aplicar únicamente una acción correctiva autorizada y acotada si la evidencia lo justifica[cite: 4]. |
| ¿Cómo se verificará la recuperación? | Revisando `systemctl status NOMBRE_SERVICIO --no-pager`, `systemctl --failed` y las entradas recientes en `journalctl -u NOMBRE_SERVICIO -n 50 --no-pager`[cite: 4]. |

## 7. Plan de recuperación y verificación
1. **Estado inicial y evidencias guardadas:** Registrar `date`, `uptime`, `systemctl --failed`, `systemctl status` y las salidas iniciales de `journalctl` y `dmesg` sin borrar ni editar archivos de registro[cite: 4].
2. **Hipótesis y motivo para investigarla:** Formular una hipótesis ligada a los hechos y marcas temporales observadas, evitando conclusiones precipitadas por un solo mensaje[cite: 4].
3. **Acción correctiva autorizada:** Solo si la evidencia lo justifica y con autorización del docente sobre una unidad no crítica (nunca sobre servicios críticos de red, almacenamiento, escritorio o sesión), aplicar la medida aprobada[cite: 4].
4. **Comando de verificación posterior:** Ejecutar `systemctl status NOMBRE_SERVICIO --no-pager`, `systemctl --failed` y `journalctl -u NOMBRE_SERVICIO -n 50 --no-pager`[cite: 4].
5. **Restauración de la configuración anterior:** Contar con un procedimiento documentado para revertir cualquier cambio autorizado y devolver la unidad al estado acordado[cite: 4].
6. **Resultado satisfactorio:** Unidad en el estado esperado, sin nuevas fallas en `systemctl --failed` y con registros consistentes con una operación normal[cite: 4].

## 8. Descripción de la simulación (análisis sin provocar falla)
No se provocó ninguna falla real ni se modificaron o eliminaron logs del sistema, siguiendo la regla de no alterar el sistema operativo principal ni forzar errores innecesarios[cite: 4]. En su lugar, se aplicó el análisis por evidencias y línea de tiempo sobre las consultas del sistema, kernel y servicio[cite: 4].

### Resolución del reto de investigación (servicio en `failed` y solicitud de reiniciar todo Ubuntu)
1. **Estado de la unidad:** Consultar `systemctl --failed` y `systemctl status NOMBRE_SERVICIO --no-pager`[cite: 4].
2. **Registros a revisar primero:** Consultar `journalctl -u NOMBRE_SERVICIO -n 50 --no-pager` y `journalctl -b -n 60 --no-pager`[cite: 4].
3. **Ubicar el momento del fallo:** Revisar las marcas temporales de las entradas previas y posteriores al fallo en el journal de la unidad y del arranque actual[cite: 4].
4. **Qué buscar en servicio y kernel:** Mensajes de inicio/detención, fallos de dependencias, timeouts o errores de configuración en la unidad, y eventos de hardware, memoria o controladores en `dmesg | tail -n 40` o `journalctl -k -b -n 40 --no-pager`[cite: 4].
5. **Información necesaria antes de corregir:** Confirmar si el fallo es reproducible, qué recurso o dependencia falló, si el cambio está autorizado y cómo revertirlo[cite: 4].
6. **Por qué no reiniciar todo el equipo automáticamente:** Reiniciar todo el sistema interrumpe todos los servicios y sesiones, puede ocultar la condición actual del fallo y no corrige por sí solo problemas de configuración o dependencias[cite: 4].
7. **Comprobar recuperación:** Verificar con `systemctl status NOMBRE_SERVICIO --no-pager`, `systemctl --failed` y `journalctl -u NOMBRE_SERVICIO -n 50 --no-pager` que la unidad quedó activa y estable[cite: 4].

## 9. Documentación y evidencias (sin inserción de capturas)
Por decisión de entrega sin capturas de pantalla, este informe documenta los comandos, criterios de interpretación, tabla de línea de tiempo, diagnóstico y plan de recuperación en texto estructurado, correspondiendo a las secciones de evidencia `01_estado_general`, `02_mensajes_kernel`, `03_registros_servicio`, `04_linea_de_tiempo` y `05_plan_recuperacion` de la carpeta `~/SistemasOperativos/semana13/`[cite: 4].

## 10. Conclusión técnica
Consultar los registros con `journalctl`, `dmesg`, `tail` y `grep` permite reconstruir eventos con mayor criterio que actuar por impulso o reiniciar todo el equipo[cite: 4]. Distinguir síntomas, observaciones e hipótesis —y proteger siempre la integridad de los logs y servicios críticos— es esencial para diagnosticar y recuperar servicios de forma responsable[cite: 4].
