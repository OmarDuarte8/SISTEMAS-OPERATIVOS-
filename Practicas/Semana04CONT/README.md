# 🕵️ Semana 04 — Investigación de seguridad

## 👤 Alumno
Omar Said Duarte Ruiz (No. Control: 242240010)

## 📅 Fecha
Octubre de 2026

## 🖥️ Sistema anfitrión
Ubuntu (omar8-Notebook)

## 🧪 Máquina objetivo
Metasploitable 2

## 🌐 Red
Host-Only (vboxnet0)

## 📡 IP Ubuntu
192.168.56.1

## 🎯 IP Metasploitable
192.168.56.101

## 🔎 Servicios encontrados
* Puerto 21/tcp (ftp)
* Puerto 22/tcp (ssh)
* Puerto 23/tcp (telnet)
* Puerto 80/tcp (http)
* Puerto 3306/tcp (mysql)
*(Entre otros múltiples servicios expuestos en el laboratorio).*

## 🔐 Usuario utilizado
* **Usuario:** msfadmin
* **UID:** 1000
* **GID:** 1000

## 👥 Grupos identificados
* adm, dialout, cdrom, floppy, audio, dip, video, plugdev, fuse, lpadmin, admin, sambashare, msfadmin.

## 🔒 Análisis de permisos
* Se examinó el archivo `/etc/passwd` (lectura global para identificar cuentas) frente a `/etc/shadow` (restringido estrictamente a root para proteger hashes de contraseñas).
* Se listaron los binarios con permisos SUID (`find / -type f -perm -4000 2>/dev/null`), identificando comandos críticos del sistema.
* Se realizó un experimento práctico con `chmod 600` sobre un archivo confidencial de prueba, restringiendo el acceso exclusivamente al propietario.

## 🚨 Hallazgos
1. Exposición masiva de servicios vulnerables y desactualizados por defecto en Metasploitable 2.
2. Uso de credenciales predeterminadas conocidas (`msfadmin:msfadmin`).
3. Presencia de múltiples binarios y servicios con configuraciones altamente permisivas.

## 🛡️ Medidas de protección
* Deshabilitar servicios innecesarios o inseguros (como Telnet y FTP antiguo).
* Aplicar el principio de mínimo privilegio en los grupos y permisos de usuario.
* Evitar el uso de contraseñas por defecto y credenciales débiles.
* Auditar periódicamente los permisos SUID y los archivos sensibles del sistema.

## 🧠 Conclusión
La práctica permitió comprender de manera práctica que la seguridad en Linux no depende únicamente de la autenticación inicial, sino de una correcta segmentación de redes, una gestión estricta de usuarios/grupos y una auditoría rigurosa de permisos en los archivos del sistema.
