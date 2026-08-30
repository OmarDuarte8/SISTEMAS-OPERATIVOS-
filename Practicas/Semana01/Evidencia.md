## Semana 01 - Sistemas Operativos

## 🎯 Objetivo

¿Qué aprendí durante la práctica?Aprendí a identificar, diagnosticar y mapear la estructura de almacenamiento físico  de un equipo mediante comandos de la terminal en Linux, reconociendo particiones y tipos de sistemas de archivos.

## 🐧 Entorno

¿Qué distribución y versión de Ubuntu utilicé?Ubuntu 26.04

¿Qué arquitectura tiene mi sistema?Arquitectura de 64 bits .

¿Qué usuario utilicé?El usuario omar8.

## 💽 Diagnóstico del almacenamiento

¿Qué dispositivos encontré?Un disco físico principal tipo NVMe identificado como nvme0n1 además de muchos  loops.

¿Qué tamaños tienen?El disco principal nvme0n1 tiene un tamaño total de 476.9 GB.

## 🧩 Particiones

¿Qué particiones encontré?Se encontraron dos particiones principales en el disco: nvme0n1p1 y nvme0n1p2.

¿Qué función parece cumplir cada una?

nvme0n1p1: Partición de 1 GB con punto de montaje en /boot/efi, encargada de almacenar los archivos de arranque del sistema operativo.
nvme0n1p2: Partición de 475.9 GB con punto de montaje en la raíz (/), que contiene todo el sistema operativo, aplicaciones y archivos de usuario.

## 🗂️ Sistemas de archivos

¿Qué sistemas de archivos encontré?En las particiones principales se detectaron vfat (FAT32) para la partición EFI y ext4 para la partición raíz del sistema. 

¿Qué información obtuve mediante lsblk -f?Obtuve los tipos de sistemas de archivos (FSTYPE), las etiquetas, los códigos UUID únicos de cada partición (como 7ACD-2B4D para la EFI y bf52f420-... para la raíz) 

## 🔎 Herramientas utilizadas

¿Qué hace cada herramienta?

lsblk: Lista los dispositivos de bloques disponibles y su jerarquía estructural.
lsblk -f: Consulta los sistemas de archivos, etiquetas y códigos UUID asociados a los bloques.
fdisk -l: Analiza la tabla de particiones y la geometría del disco a bajo nivel.
parted -l: Muestra la distribución e información de partición con soporte avanzado para GPT.
blkid: Identifica los atributos de los bloques de dispositivos de forma directa.

¿Qué información obtuve con ella?La jerarquía física de discos, particiones, tamaños exactos, tipos de formatos de archivos y códigos de identificación únicos para la administración del sistema.

## 🥾 EFI / UEFI

¿Qué encontré?La partición de arranque nvme0n1p1 configurada con el sistema de archivos 

¿Qué función cumple?Almacena los archivos ejecutables y controladores necesarios para que el firmware UEFI cargue el gestor de arranque de Linux al encender la computadora.

## 🔄 GPT / MBR

¿Qué esquema utiliza mi equipo?Utiliza el esquema moderno GPT (GUID Partition Table) 

¿Qué investigué sobre él?Es el estándar moderno que reemplaza al antiguo MBR

## 🔐 BitLocker

¿Existe en mi equipo?NO

¿Qué investigué sobre él?Es una herramienta de cifrado de disco integrada en Windows para proteger la información contra accesos no autorizados

## 🗺️ Mapa del almacenamiento

Incluye aquí tu mapa.


OMAR8-NOTEBOOK (Equipo)
   │
   └── NVME0N1 (Disco principal - 476.9 GB)
        │
        ├── nvme0n1p1 (1 GB, vfat / FAT32) ── /boot/efi (Partición EFI)
        └── nvme0n1p2 (475.9 GB, ext4) ── / (Partición Raíz de Ubuntu)



## 💾 Laboratorio

Si utilizaste un disco virtual:

No se utilizó 
## 🧪 Antes y después


## ⚠️ Seguridad

¿Qué riesgos existen al trabajar con discos y particiones?El riesgo principal es la pérdida total de datos del sistema operativo 
## 💭 Reflexión final

¿Qué aprendí sobre la administración del almacenamiento?Aprendí que conocer a fondo la estructura de particiones








