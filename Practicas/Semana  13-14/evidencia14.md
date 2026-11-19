

### `README.md` — Práctica 14 (`~/SistemasOperativos/semana14/README.md`)

```markdown
# 🔵 Semana 14 — Administración de un servidor Ubuntu

## Datos del alumno
- Nombre: Omar Said
- Grupo: Sistemas Operativos (ITSRV)[cite: 4]
- Fecha: 2026-11-19

## Entorno
- Distribución y versión: Ubuntu Linux (consultada mediante `cat /etc/os-release`)[cite: 4]
- Kernel: Kernel de Linux en Ubuntu (consultado mediante `uname -a`)[cite: 4]
- Equipo físico o máquina virtual: Equipo de laboratorio / máquina virtual autorizada[cite: 4]
- Tipo de red: Entorno propio o de laboratorio aislado, sin exponer servicios a Internet[cite: 4]
- Servicio seleccionado: Servicio no crítico o de laboratorio autorizado identificado en el sistema (sin modificar servicios críticos ni instalar software sin autorización)[cite: 4]

## Inventario
- Almacenamiento: Revisado con `lsblk -f`, `df -h` y `du -sh ~/SistemasOperativos` para distinguir dispositivos, espacio de sistemas de archivos y uso de carpetas[cite: 4].
- Usuario y grupos: Identidad actual (`whoami`), UID/GID y grupos (`id`, `groups`) y permisos de `~/servidor_laboratorio` (`ls -l`)[cite: 4].
- Paquete: Consultada disponibilidad y estado local con `apt show` / `dpkg -l` / `dpkg -s` sin reinstalar ni instalar sin autorización[cite: 4].
- Dirección/interfaz: Interfaces y direcciones IP revisadas con `ip -br address`[cite: 4].
- Ruta: Tabla de rutas revisada con `ip route`[cite: 4].
- Servicio y estado: Estado actual (`systemctl status`, `systemctl is-active`) y habilitación al arranque (`systemctl is-enabled`) de la unidad seleccionada[cite: 4].
- Sockets: Sockets en escucha consultados con `ss -lnt` (y `sudo ss -lntup` si está autorizado)[cite: 4].
- Estado de UFW: Estado y reglas consultadas con `sudo ufw status verbose` sin abrir puertos públicos innecesarios ni alterar sesiones remotas[cite: 4].

## Pruebas
| Componente | Comando | Resultado real |
|---|---|---|
| Sistema | `uname -a` | Muestra información del kernel y arquitectura del equipo de laboratorio[cite: 4]. |
| Almacenamiento | `df -h` | Muestra capacidad, espacio usado y disponible de los sistemas de archivos montados[cite: 4]. |
| Identidad | `id` | Muestra UID, GID y grupos asociados al usuario de la sesión[cite: 4]. |
| Paquete | `dpkg -s` | Indica el estado y detalles del paquete consultado localmente en el sistema[cite: 4]. |
| Servicio | `systemctl status` | Muestra el estado actual y detalles de la unidad seleccionada[cite: 4]. |
| Red | `ip route` | Muestra la tabla de rutas y la puerta de enlace configurada si existe[cite: 4]. |
| Sockets | `ss -lnt` | Lista los sockets TCP en escucha en formato numérico[cite: 4]. |
| Firewall | `ufw status verbose` | Muestra el estado de UFW, políticas predeterminadas y reglas vigentes[cite: 4]. |
| Logs | `journalctl -u` | Muestra los registros recientes asociados a la unidad seleccionada[cite: 4]. |

## Cambios autorizados
- Cambio: Creación del directorio de trabajo `~/servidor_laboratorio/documentacion` y `~/servidor_laboratorio/evidencias`, además de `estado_inicial.md` (y cualquier cambio de servicio/regla solo si fue expresamente autorizado)[cite: 4].
- Motivo: Organizar la documentación del servidor de laboratorio y verificar el funcionamiento de forma controlada y reversible[cite: 4].
- Verificación: Comprobación con `systemctl status`, `ss -lnt`, `journalctl -u` y `systemctl --failed` comparando contra el estado inicial[cite: 4].
- Reversión o estado final: Cualquier prueba o modificación autorizada queda documentada con su procedimiento de reversión y restaurada al estado acordado al cerrar la práctica[cite: 4].

## Conclusión
Integrar almacenamiento, identidad y permisos, paquetes, servicios, red, sockets, firewall y registros desde la terminal permite administrar un servidor Ubuntu de forma ordenada, verificable y segura dentro de un entorno de laboratorio[cite: 4].

```
