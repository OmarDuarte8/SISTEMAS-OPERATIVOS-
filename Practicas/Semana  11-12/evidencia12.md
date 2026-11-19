

### `README.md` — Práctica 12 (`~/SistemasOperativos/semana12/README.md`)

```markdown
# 🔵 Semana 12 — Seguridad y firewall

## Datos del alumno
- Nombre: Omar Said
- Grupo: Sistemas Operativos (ITSRV)
- Fecha: 2026-11-19

## Entorno
- Distribución y versión: Ubuntu Linux (versión consultada en el equipo de laboratorio mediante `cat /etc/os-release`)
- Equipo físico o máquina virtual: Equipo de laboratorio / máquina virtual de prueba
- ¿Es un entorno de laboratorio?: Sí, entorno controlado y autorizado para prácticas académicas

## Estado inicial
- Estado de UFW: Consultado con `sudo ufw status` (inactivo o activo según la configuración inicial del equipo de laboratorio, sin alterar al inicio)
- Política de entrada: Predeterminada según `sudo ufw status verbose` (habitualmente `deny (incoming)` o la definida en el sistema)
- Política de salida: Predeterminada según `sudo ufw status verbose` (habitualmente `allow (outgoing)` o la definida en el sistema)
- Reglas existentes: Consultadas con `sudo ufw status numbered` antes de cualquier análisis o cambio
- Servicios en escucha: Revisados mediante `ss -lnt`, `ss -lnu` (y `sudo ss -lntup` si está autorizado)

## Análisis
| Comando | Propósito | Resultado real |
|---|---|---|
| `id` | Mostrar el UID, GID y grupos a los que pertenece el usuario actual | Muestra usuario, grupo principal y grupos secundarios para verificar el principio de mínimo privilegio |
| `ls -l` | Consultar permisos, propietario, grupo y tamaño de directorios/archivos propios | Muestra permisos de lectura/escritura/ejecución (p. ej. `drwxr-xr-x`) sobre `~/SistemasOperativos` |
| `ss -lnt` | Listar sockets TCP en escucha con direcciones y puertos en formato numérico | Muestra sockets TCP en estado `LISTEN` sin resolver nombres de servicio/host |
| `sudo ufw status verbose` | Mostrar estado de UFW, políticas predeterminadas y detalle de reglas | Indica si UFW está activo/inactivo y las políticas de entrada, salida y enrutamiento |
| `sudo ufw status numbered` | Listar las reglas activas de UFW con su número de índice | Enumera cada regla con un número para identificarla con precisión antes de revertir una temporal |

## Cambios autorizados
- ¿Se modificó UFW?: Solo si el docente lo autorizó expresamente en un entorno de laboratorio seguro; en caso contrario, únicamente se consultó y documentó
- Regla aplicada, si corresponde: Regla específica aprobada por el docente (nunca un puerto al azar ni borrando reglas existentes o ejecutando `ufw reset`)
- Evidencia de verificación: Comprobación con `sudo ufw status numbered` y `sudo ufw status verbose` comparando contra el estado inicial
- ¿Se restauró el estado?: Sí, retirando con `sudo ufw delete NUMERO` únicamente la regla temporal autorizada si así se indicó, dejando intactas las reglas previas

## Conclusión
El principio de mínimo privilegio consiste en otorgar únicamente los accesos y permisos estrictamente necesarios para cumplir una función. Revisar sockets en escucha, permisos de archivos y el estado de UFW antes de aplicar reglas evita bloquear sesiones legítimas (como SSH) y permite tomar decisiones de seguridad justificadas y reversibles.

```
