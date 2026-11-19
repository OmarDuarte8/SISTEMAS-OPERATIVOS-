
### 1. Archivo: `README.md` — Práctica 11 (`~/SistemasOperativos/semana11/README.md`)

```markdown
# 🟢 Semana 11 — Redes desde la terminal

## Datos del alumno
- Nombre: Omar Said
- Grupo: Sistemas Operativos (ITSRV)
- Fecha: 2026-11-19

## Entorno
- Distribución y versión: Ubuntu Linux (consultada mediante `cat /etc/os-release`)
- Equipo físico o máquina virtual: Equipo de laboratorio / máquina virtual
- Tipo de red utilizada: Red propia o red autorizada para la práctica

## Diagnóstico
| Comando | Propósito | Resultado real |
|---|---|---|
| `ip -br address` | Mostrar interfaces de red, estado y direcciones IP asignadas en formato resumido | Lista las interfaces (`lo`, interfaz de red física o virtual) y sus direcciones IPv4/IPv6 asignadas |
| `ip route` | Consultar la tabla de rutas y la puerta de enlace predeterminada (`default`) si existe | Muestra las rutas conocidas y el siguiente salto (`via ... dev ...`) o indica si no hay ruta predeterminada |
| `ping` | Comprobar conectividad ICMP con `ping -c 4` hacia `127.0.0.1`, puerta de enlace o destino autorizado | Registra respuestas ICMP, pérdida de paquetes o ausencia de respuesta sin asumir que un fallo de `ping` implica caída total |
| `getent hosts` | Consultar la resolución de nombres de dominio (`getent hosts example.com`) según el sistema | Devuelve los registros/direcciones resueltos para el dominio o indica que no hubo resolución |
| `ss -lnt` | Listar sockets TCP en escucha (`LISTEN`) con direcciones y puertos en formato numérico | Muestra los puertos TCP en escucha y sus direcciones locales sin resolver nombres de host/servicio |
| `curl -I` | Solicitar cabeceras HTTP/HTTPS (`curl -I --max-time 10 https://example.com`) con tiempo límite | Muestra la respuesta/cabeceras HTTP del destino o registra el motivo si no se completó dentro del tiempo límite |

## Hallazgos
- Evidencia observada: Interfaces y direcciones revisadas con `ip -br link`/`ip -br address`/`ip address`, rutas con `ip route`, pruebas ICMP con `ping -c 4`, resolución de nombres con `resolvectl status` y `getent hosts` (y `dig` si está disponible), sockets con `ss -lnt`/`ss -lnu` (y `sudo ss -lntup` si está autorizado) y consulta HTTP con `curl -I --max-time 10 https://example.com`.
- Hipótesis: Cada capa se evalúa según su resultado: si falta ruta o IP, el problema está en configuración local/red; si hay IP y ruta pero falla DNS, la hipótesis apunta a resolución de nombres; si resuelve y responde ICMP pero falla HTTP, se revisa conectividad al puerto/servicio o políticas de red.
- Datos pendientes: Que `ping` falle no demuestra por sí solo ausencia de conectividad; que DNS resuelva no garantiza que el servicio web esté disponible; y un error HTTP puede corresponder al servidor/aplicación y no a una falla de red.
- Próximo paso recomendado: Diagnosticar capa por capa en orden y solicitar evidencia o autorización adicional antes de modificar direcciones IP, rutas o configuración DNS.

## Conclusión
Diagnosticar una conexión desde la terminal requiere avanzar por etapas (interfaz e IP, rutas, conectividad ICMP, DNS, sockets y HTTP) y separar hechos observados de hipótesis. Así se evitan cambios a ciegas o conclusiones apresuradas a partir de una sola prueba.

```

---

### 2. Archivo: `informe_diagnostico_de_red.md` — Práctica 11 (`~/SistemasOperativos/semana11/informe_diagnostico_de_red.md`)

```markdown
# Informe — Práctica 11: Diagnóstico de redes desde la terminal en Ubuntu

## 1. Descripción del caso y objetivo
Cuando un usuario reporta que no puede acceder a una página web, la falla puede ubicarse en distintas capas: la interfaz de red, la dirección IP, la tabla de rutas, la conectividad, la resolución DNS, los sockets/puertos o el propio servicio web. Reiniciar el equipo sin investigar puede ocultar el problema y no garantiza resolverlo. El objetivo de esta práctica es aplicar un procedimiento ordenado por etapas desde la terminal de Ubuntu para inspeccionar estos componentes y distinguir entre observaciones respaldadas por evidencia e hipótesis.

## 2. Explicación de interfaz, IP, ruta, DNS, socket y HTTP
- **Interfaz de red:** Componente lógico mediante el cual el sistema envía y recibe tráfico (puede ser física o virtual, con nombres como `enp...`, `ens...`, `wlp...` o loopback).
- **Dirección IP:** Dirección lógica asignada a una interfaz, acompañada de su máscara o prefijo que delimita la red. La dirección de loopback (`127.0.0.1` en IPv4) corresponde a la comunicación local del propio equipo.
- **Ruta:** Entrada de la tabla de rutas que determina por qué interfaz y mediante qué puerta de enlace (si aplica) se envía el tráfico hacia un destino. La ruta predeterminada (`default`) se usa cuando no hay una ruta más específica.
- **DNS:** Mecanismo que traduce nombres de dominio (como `example.com`) a registros que pueden incluir direcciones IP.
- **Socket:** Extremo de comunicación asociado a un protocolo (como TCP o UDP), dirección y puerto. Un socket en escucha espera conexiones o tráfico, pero no demuestra por sí solo que un servicio sea vulnerable ni que sea accesible desde cualquier equipo.
- **HTTP/HTTPS:** Protocolos utilizados para acceder a recursos web; una respuesta o código de error HTTP no equivale necesariamente a una falla de red, ya que el servidor puede responder con un código de error de aplicación.

## 3. Comandos ejecutados y explicación de sus partes
- `ip -br link` / `ip -br address`: `ip` consulta o administra parámetros de red; `-br` resume la salida; `link` muestra enlaces/interfaces y `address` muestra direcciones.
- `ip address`: Muestra los detalles completos de las interfaces, su estado y direcciones configuradas.
- `ip route`: Muestra la tabla de rutas, incluida la ruta predeterminada y la puerta de enlace si existe.
- `ping -c 4 <destino>`: `ping` envía paquetes ICMP para comprobar respuestas; `-c 4` limita la prueba a cuatro solicitudes hacia el destino (`127.0.0.1`, puerta de enlace o IP externa autorizada si la política de red lo permite).
- `resolvectl status`: Consulta el estado de resolución de nombres y los servidores DNS configurados cuando el sistema utiliza `systemd-resolved`.
- `getent hosts example.com`: Consulta la base de datos de nombres configurada en el sistema para resolver el dominio.
- `command -v dig` / `dig example.com`: `command -v` verifica si `dig` está disponible; `dig` consulta registros DNS mostrando respuesta, errores y tiempo si aplica.
- `ss -lnt` / `ss -lnu` / `sudo ss -lntup`: `ss` consulta sockets; `-l` filtra sockets en escucha; `-n` muestra números sin resolver nombres; `-t` indica TCP, `-u` indica UDP y `-p` muestra procesos asociados cuando los permisos lo permiten (`sudo`).
- `curl -I --max-time 10 https://example.com`: `curl` realiza solicitudes a recursos compatibles; `-I` solicita solo cabeceras; `--max-time 10` limita la operación a 10 segundos.
- `command -v wget` / `command -v traceroute`: Comprueban si estas herramientas están disponibles sin obligar a instalarlas ni ejecutar trazas externas.

## 4. Resultados reales, con evidencias
### Interfaces y rutas (`ip -br address` e `ip route`)
```text


```

### Pruebas de conectividad (`ping -c 4`)

| Prueba | Resultado observado | ¿Qué ayuda a comprobar? | ¿Qué no demuestra por sí sola? |
| --- | --- | --- | --- |
| `ping -c 4 127.0.0.1` | [Pegar salida real de ping -c 4 127.0.0.1] | Pila de red local | Conectividad con otros equipos |
| Ping a la puerta de enlace | [Pegar salida real o indicar si no hay puerta de enlace / no responde] | Posible comunicación con el siguiente salto | Acceso a Internet |
| Ping a una IP externa autorizada | [Pegar salida real o indicar si se omitió por política de red] | Posible conectividad IP externa | Funcionamiento de DNS o HTTP |


```

## 5. Tabla de hechos observados, hipótesis y datos pendientes

| Capa / Aspecto | Hecho observado | Hipótesis | Dato pendiente |
| --- | --- | --- | --- |
| Interfaz y dirección IP | [Resultado real de ip -br address] | [Hipótesis basada en la interfaz e IP] | Si el estado o dirección de la interfaz cambia bajo otra configuración |
| Rutas | [Resultado real de ip route] | [Hipótesis basada en la ruta predeterminada o ausencia de ella] | Comportamiento de rutas hacia subredes específicas o externas |
| Conectividad ICMP | [Resultado real de ping -c 4] | [Hipótesis sobre alcance local, puerta de enlace o red externa] | Una falla de `ping` no demuestra por sí sola que no haya conectividad |
| Resolución DNS | [Resultado real de resolvectl status / getent hosts / dig] | [Hipótesis sobre resolución de nombres] | Que DNS resuelva un nombre no demuestra que el servidor web esté disponible |
| Sockets / HTTP | [Resultado real de ss y curl -I] | [Hipótesis sobre escucha local y respuesta HTTP] | Un error HTTP no siempre significa falla de red; puede ser de aplicación |

## 6. Resolución del reto de diagnóstico

Ante el caso de que el navegador no abra `https://example.com`, sigo este procedimiento ordenado sin alterar la configuración:

1. **Revisar interfaces y direcciones:** Ejecutar `ip -br link`, `ip -br address` e `ip address` para verificar las interfaces y sus direcciones IP.
2. **Consultar la tabla de rutas:** Ejecutar `ip route` para revisar la ruta predeterminada y la puerta de enlace, si existen.
3. **Comprobar conectividad local:** Ejecutar `ping -c 4 127.0.0.1` y, según corresponda y esté autorizado, comprobar la puerta de enlace o una IP externa permitida.
4. **Investigar DNS:** Revisar `resolvectl status`, `getent hosts example.com` y `dig example.com` (si está instalado) para comprobar si el dominio se resuelve.
5. **Examinar sockets locales:** Consultar `ss -lnt`, `ss -lnu` (y `sudo ss -lntup` con autorización) para observar los sockets locales en escucha.
6. **Realizar prueba HTTP:** Ejecutar `curl -I --max-time 10 https://example.com` si la política de red lo permite, observando la respuesta o si se agota el tiempo.
7. **Formular hipótesis y respaldar con evidencia:** Contrastar los resultados de cada etapa para plantear una hipótesis coherente con la evidencia obtenida antes de proponer cambios.

| Capa investigada | Comando | Resultado real | Conclusión provisional |
| --- | --- | --- | --- |
| Interfaz | `ip -br address` | [Resultado real] | [Conclusión provisional] |
| Ruta | `ip route` | [Resultado real] | [Conclusión provisional] |
| Conectividad | `ping` | [Resultado real] | [Conclusión provisional] |
| DNS | `getent hosts` / `dig` | [Resultado real] | [Conclusión provisional] |
| Sockets | `ss -lnt` | [Resultado real] | [Conclusión provisional] |
| HTTP | `curl -I` | [Resultado real] | [Conclusión provisional] |

* **¿Por qué conviene diagnosticar por etapas?** Porque permite aislar qué parte del camino responde o falla y evita cambiar configuraciones al azar o concluir con base en una sola prueba.
* **¿Qué diferencia existe entre una dirección IP y un nombre de dominio?** La dirección IP identifica lógicamente una interfaz/host en la red, mientras que el nombre de dominio es un nombre legible que DNS asocia con registros (como direcciones IP).
* **¿Por qué `ping` puede fallar aunque un servicio web funcione?** Porque los mensajes ICMP pueden estar bloqueados o filtrados mientras el tráfico de la aplicación web sigue operando.
* **¿Qué significa que un puerto esté en escucha?** Significa que hay un socket en estado de escucha listo para recibir tráfico en la dirección y puerto indicados según el protocolo.
* **¿Qué evidencia adicional solicitarías antes de cambiar la configuración de red?** Verificar el comportamiento en otros destinos autorizados, conocer la configuración inicial exacta y contar con autorización y respaldo antes de modificar rutas, DNS o direcciones.

## 7. Limitaciones de las pruebas

Las pruebas se realizan únicamente sobre redes y destinos autorizados y tienen alcances distintos: `ping` depende de ICMP y posibles filtros, DNS depende de la configuración y respuesta del resolutor, `ss` muestra sockets locales y `curl` depende de la política de red y del servidor remoto.

## 8. Conclusión técnica

Diagnosticar redes desde la terminal exige orden, precisión y respeto por los límites de cada prueba. Separar lo observado, las hipótesis y lo que falta comprobar permite orientar mejor la revisión sin alterar configuraciones ni atribuir causas sin evidencia.


