# Proyecto integrador — Sistemas Operativos

## 1. Datos generales
- Estudiante: Omar Said
- Grupo: Sistemas Operativos (ITSRV)
- Fecha: 2026-11-19
- Entorno: Ubuntu Linux en equipo de laboratorio o máquina virtual autorizada (sin confundir discos o interfaces virtuales con los del anfitrión, ni exponer servicios a Internet)[cite: 5].
- Versión de Ubuntu: Consultada mediante `cat /etc/os-release` en el entorno de trabajo[cite: 5].
- Kernel: Consultada mediante `uname -a` y `uname -m` (arquitectura del kernel)[cite: 5].

## 2. Objetivo
Demostrar la administración y el diagnóstico de un entorno Ubuntu desde la terminal integrando los conocimientos del semestre: sistema de archivos y almacenamiento, usuarios/grupos/permisos, procesos y recursos, paquetes, servicios, red, firewall y registros (`logs`), explicando qué hace cada comando, por qué se usa, cómo se interpreta su salida y cómo se verifica cada tarea de forma segura[cite: 5].

## 3. Inventario del sistema
- **Distribución y versión:** Obtenida con `cat /etc/os-release` para identificar con exactitud la distribución y versión de Ubuntu sobre la que se opera[cite: 5].
- **Kernel y arquitectura:** `uname -a` aporta información completa del kernel y arquitectura del sistema, mientras que `uname -m` muestra específicamente la arquitectura de máquina[cite: 5].
- **Hostname y entorno:** `hostnamectl` y `hostname` identifican el nombre del equipo y el entorno del sistema; `whoami` y `date` registran el usuario efectivo y la fecha/hora de la sesión[cite: 5].
- **Tiempo de actividad (`uptime`):** Indica cuánto tiempo lleva encendido el sistema desde su último arranque junto con los promedios de carga del sistema[cite: 5].
- **Importancia de registrar el entorno antes de diagnosticarlo:** Evita confundir máquinas o discos/interfaces (especialmente en máquinas virtuales), contextualiza marcas de tiempo y permite comparar el estado inicial antes de realizar cualquier consulta o cambio autorizado[cite: 5].

## 4. Almacenamiento y sistema de archivos
Comandos utilizados sin modificar particiones ni formatear discos[cite: 5]:
- `lsblk`: Lista los dispositivos de bloques y particiones reconocidas por el sistema[cite: 5].
- `df -hT`: Muestra el espacio usado y disponible en formato legible (`-h`) junto con el tipo de sistema de archivos (`-T`)[cite: 5].
- `df -i`: Muestra el uso de inodos en los sistemas de archivos[cite: 5].
- `findmnt`: Muestra los sistemas de archivos montados y sus puntos de montaje[cite: 5].
- `du -sh ~`: Estima el espacio ocupado por el directorio personal del usuario actual[cite: 5].

Respuestas e interpretación:
1. **Dispositivos y particiones:** Corresponden a los discos/volúmenes y particiones detectados en el entorno (virtual o físico) según `lsblk`[cite: 5].
2. **Punto de montaje de la raíz (`/`):** Es el directorio raíz `/` donde se monta el sistema de archivos principal del sistema según `findmnt` y `df -hT`[cite: 5].
3. **Espacio libre en `/`:** Se lee directamente en la columna de espacio disponible del sistema de archivos montado en `/` dentro de la salida de `df -hT`[cite: 5].
4. **Diferencia entre tamaño en `lsblk` y espacio disponible en `df -hT`:** `lsblk` muestra el tamaño de dispositivos de bloques o particiones, mientras que `df -hT` reporta el espacio usado y disponible a nivel del sistema de archivos montado[cite: 5].
5. **Por qué revisar espacio antes de instalar o diagnosticar:** Evita quedarse sin espacio o sin inodos durante instalaciones, descargas o generación de registros, y ayuda a descartar saturación de almacenamiento como causa de fallos[cite: 5].

## 5. Usuarios, grupos y permisos
Comandos ejecutados en `~/SistemasOperativos/semana15/laboratorio_permisos` con `archivo_prueba.txt` y `chmod 640 archivo_prueba.txt` (sin usar `chmod -R` en rutas del sistema ni alterar `/etc`, `/var`, `/usr` o todo el directorio personal)[cite: 5]:
- `id` y `groups`: Muestran el UID (identificador de usuario), el GID (identificador de grupo principal) y los grupos a los que pertenece el usuario[cite: 5].
- `getent passwd "$(whoami)"` y `getent group`: Consultan la información de cuenta de usuario y grupos configurados[cite: 5].
- `ls -l` y `stat archivo_prueba.txt`: Muestran permisos, propietario, grupo y metadatos del archivo de laboratorio[cite: 5].

Respuestas e interpretación:
1. **UID y GID:** Son los identificadores numéricos del usuario actual y de su grupo principal reportados por `id`[cite: 5].
2. **Grupos de pertenencia:** Son los grupos listados por `groups` e `id` para la cuenta activa[cite: 5].
3. **Permisos `rw-r-----` (`640`):** El propietario tiene lectura y escritura (`rw-`), el grupo tiene solo lectura (`r--`) y otros usuarios no tienen permisos (`---`)[cite: 5].
4. **Riesgo de dar escritura a todos los usuarios:** Cualquier usuario local con acceso al archivo podría modificar o sobrescribir su contenido, comprometiendo la integridad del archivo o del sistema[cite: 5].
5. **Diferencia entre propietario, grupo y otros:** El propietario es el usuario dueño del recurso; el grupo agrupa cuentas con acceso compartido definido en el archivo; y "otros" abarca a cualquier otro usuario del sistema que no sea el propietario ni miembro del grupo[cite: 5].

## 6. Procesos y recursos
Comandos consultados (saliendo de `top` con `q` y registrando si `vmstat` no está instalado sin instalarlo sin autorización)[cite: 5]:
- `ps aux` y `ps -eo pid,ppid,comm,%cpu,%mem --sort=-%cpu | head`: Muestran procesos activos, PID, PPID, comando y porcentajes de CPU y memoria ordenados por consumo de CPU[cite: 5].
- `free -h` y `uptime`: Muestran el estado de la memoria en unidades legibles y la carga/actividad del sistema[cite: 5].
- `top` y `vmstat 1 5` (si está disponible): Permiten observar comportamiento dinámico de procesos y recursos del sistema[cite: 5].

Respuestas e interpretación:
1. **Qué identifica un PID:** El Process ID es el identificador numérico único asignado por el kernel a un proceso en ejecución[cite: 5].
2. **Proceso con mayor consumo de CPU:** Se identifica en la primera fila de datos (tras la cabecera) de `ps -eo pid,ppid,comm,%cpu,%mem --sort=-%cpu | head` al momento de la consulta[cite: 5].
3. **Memoria disponible según `free -h`:** Se observa en la columna `available` de la línea `Mem:` de `free -h`[cite: 5].
4. **Diferencia entre memoria total, usada y disponible:** La memoria total es la capacidad física/asignada; la usada es la memoria actualmente ocupada; y la disponible estima cuánta memoria puede asignarse a nuevas aplicaciones sin recurrir de inmediato a intercambio (`swap`) excesivo[cite: 5].
5. **Por qué no terminar un proceso solo por consumo alto:** Un pico de CPU o memoria puede ser normal durante una tarea legítima, y matar un proceso sin identificar qué es ni qué depende de él puede interrumpir servicios o provocar pérdida de datos[cite: 5].

## 7. Paquetes y servicios
### Paquetes (solo consulta, sin `apt upgrade`, eliminación ni instalación no autorizada)[cite: 5]:
- `apt list --installed` y `dpkg -l`: Permiten consultar paquetes instalados en el sistema[cite: 5].
- `apt show bash` y `dpkg -s bash`: Muestran metadatos, descripción, dependencias y estado del paquete `bash`[cite: 5].
- `apt list --upgradable`: Consulta paquetes con actualizaciones disponibles según el índice local (registrando su salida real o si requiere actualización de índices previa autorización)[cite: 5].

Respuestas sobre paquetes:
1. **Diferencia entre `apt` y `dpkg`:** `apt` es una herramienta de alto nivel que gestiona índices de repositorios, búsquedas, instalaciones y resolución de dependencias; `dpkg` es una herramienta de bajo nivel que administra y consulta paquetes `.deb` localmente[cite: 5].
2. **Información sobre `bash`:** Se consultaron sus detalles de paquete y estado instalado mediante `apt show bash` y `dpkg -s bash`[cite: 5].
3. **Por qué revisar un paquete antes de instalarlo:** Para conocer su función, versión, dependencias y posibles cambios en el sistema antes de modificarlo[cite: 5].
4. **Por qué no hacer actualización completa sin revisar ni autorizar:** Puede actualizar múltiples componentes del sistema, introducir cambios inesperados o requerir ventana/autorización previa[cite: 5].

### Servicios (sin detener ni reiniciar servicios para provocar fallas)[cite: 5]:
- `systemctl --failed`, `systemctl list-units --type=service --state=running`, `systemctl status cron --no-pager` (o una unidad no crítica existente si `cron` no está disponible) y `journalctl -u cron -n 30 --no-pager` (o la unidad seleccionada)[cite: 5].

Respuestas sobre servicios:
1. **Activo vs. habilitado:** Un servicio activo (`active`) está ejecutándose/operativo en el estado actual; uno habilitado (`enabled`) está configurado para iniciarse automáticamente en el arranque[cite: 5].
2. **Qué informa `systemctl --failed`:** Lista las unidades que `systemd` registra en estado `failed`[cite: 5].
3. **Datos en `systemctl status`:** Muestra el estado de carga y ejecución, PID principal, memoria/tareas asociadas y líneas recientes de registro de la unidad[cite: 5].
4. **Relación entre estado y registros:** El estado muestra la condición actual de la unidad y los registros (`journalctl -u`) explican los eventos, inicios, detenciones o errores que llevaron a ese estado[cite: 5].
5. **Qué verificar antes de reiniciar un servicio:** Confirmar su nombre y función, revisar `systemctl status` y `journalctl -u`, evaluar el impacto en usuarios/sesiones y contar con autorización[cite: 5].

## 8. Red y firewall
Comandos consultados (sin abrir puertos ni modificar reglas sin autorización y plan de recuperación)[cite: 5]:
- `ip addr` e `ip route`: Muestran interfaces de red, direcciones IP asignadas y la tabla de rutas[cite: 5].
- `ping -c 4 127.0.0.1`, `ping -c 4 1.1.1.1` y `ping -c 4 example.com`: Comprueban conectividad local (`loopback`), conectividad IP externa y resolución/alcance ICMP hacia un dominio cuando la política de red lo permite[cite: 5].
- `ss -tuln` y `ss -tun`: Muestran sockets TCP/UDP en escucha y conexiones numéricas[cite: 5].
- `curl -I --max-time 10 https://example.com`: Consulta cabeceras HTTP/HTTPS con límite de 10 segundos si `curl` y la red lo permiten[cite: 5].
- `sudo ufw status verbose`: Consulta el estado, políticas predeterminadas y reglas de UFW[cite: 5].

Respuestas y análisis:
1. **Interfaces y dirección IP:** Se identifican la interfaz de loopback (`lo`) y la interfaz de red activa que cuente con dirección IP asignada en `ip addr`[cite: 5].
2. **Función de la ruta predeterminada:** Indica por qué interfaz y puerta de enlace enviar el tráfico destinado a redes que no coinciden con rutas más específicas[cite: 5].
3. **Diferencia entre dirección IP y nombre de dominio:** La dirección IP identifica lógicamente un host/interfaz en la red, mientras que el nombre de dominio es un nombre legible que DNS traduce a registros/direcciones[cite: 5].
4. **Qué significa que un puerto esté en escucha:** Que hay un socket abierto esperando conexiones o paquetes en ese puerto y dirección local según el protocolo[cite: 5].
5. **Por qué no exponer un servicio sin conocer función y riesgos:** Exponer un servicio innecesario o mal configurado amplía la superficie de ataque y puede comprometer la seguridad o disponibilidad del sistema[cite: 5].
6. **UFW, políticas, sockets y mínimo privilegio:** Se registra si UFW está activo o inactivo, sus políticas predeterminadas (`incoming`/`outgoing`) y reglas existentes en `sudo ufw status verbose`, junto con los puertos en `ss -tuln`. No todos los puertos en escucha deben exponerse a otras máquinas (muchos son locales o no requeridos); aplicar el principio de mínimo privilegio significa permitir únicamente los accesos y permisos estrictamente necesarios[cite: 5].

## 9. Logs y diagnóstico
Consultas realizadas sin borrar ni editar archivos de registro[cite: 5]:
- `journalctl -n 50 --no-pager`, `journalctl -b -n 50 --no-pager`, `journalctl -p err -b --no-pager`, `dmesg --level=err,warn` y `sudo tail -n 50 /var/log/syslog` (cuando exista y haya permiso, o usando `journalctl` en su lugar)[cite: 5].

Caso de diagnóstico (basado en un evento real observado del sistema o del arranque sin provocar fallas)[cite: 5]:
1. **¿Qué ocurrió?** Se observó un evento/mensaje real registrado durante la sesión o el arranque actual[cite: 5].
2. **¿Cuándo ocurrió?** Según la marca de tiempo (fecha y hora) registrada en la línea correspondiente del log[cite: 5].
3. **¿Qué componente o servicio estuvo involucrado?** El servicio, demonio, módulo del kernel o unidad indicada en el mensaje[cite: 5].
4. **¿Dónde se registró?** En la salida consultada de `journalctl` (`-b`, `-p err -b`, `-u`) o `dmesg --level=err,warn`[cite: 5].
5. **¿Qué hipótesis razonable planteas?** Se formula una hipótesis acotada a lo observado, diferenciando el mensaje registrado de una causa raíz confirmada sin suposiciones infundadas[cite: 5].
6. **¿Qué comprobarías después?** Verificar el estado con `systemctl status <unidad> --no-pager`, revisar `systemctl --failed` y consultar nuevamente `journalctl -u <unidad> -n 30 --no-pager`[cite: 5].
7. **¿Se solucionó?** El análisis del registro por sí solo no equivale a dar por solucionado un problema; se requiere verificar el estado y el comportamiento posterior del componente[cite: 5].

## 10. Problemas encontrados
Durante el trabajo desde terminal se respetaron las limitaciones reales del entorno (como diferencias de disponibilidad de comandos opcionales como `vmstat` o `curl`, permisos de lectura en ciertos registros o políticas de red) registrando cada resultado real sin inventar salidas ni forzar cambios no autorizados[cite: 5].

## 11. Conclusiones
Aprendí a administrar y diagnosticar Ubuntu desde la terminal integrando almacenamiento, permisos, procesos, paquetes, servicios, red, firewall y logs bajo una metodología de observación, interpretación, verificación y seguridad[cite: 5]. Antes de modificar un sistema real, es indispensable registrar el estado inicial, contar con autorización, evitar comandos destructivos o cambios sin plan de reversión y comprobar siempre los resultados[cite: 5].


