Informe de Diagnostico de Procesos
1. Monitoreo en tiempo real con top

Para observar el comportamiento dinamico del sistema operativo, se ejecuto la herramienta top en la terminal. 

    Permitio visualizar de forma inmediata que procesos estaban consumiendo mas porcentaje de CPU y memoria RAM segundo a segundo.

    Mostro el estado actual de los procesos, el usuario propietario, el PID y el tiempo de ejecucion acumulado.

    Se comprobo que al abrir o cerrar nuevas pestanas y comandos en la terminal, los valores de los recursos fluctuan de manera dinamica.

2. Inspeccion del sistema de archivos virtual /proc

Linux organiza la informacion de todos los procesos activos a traves del directorio virtual /proc.
aparicion:

    Al listar el contenido de /proc (ls /proc), se encuentran multiples directorios nombrados con numeros. Cada uno de estos numeros coincide exactamente con el PID de un proceso activo en el sistema.

3. Reto: Diagnostico de procesos ante un sistema lento

Imaginando el escenario donde un usuario reporta que su computadora esta lenta y se requiere realizar un diagnostico profesional sin caer en respuestas vagas, se establece la siguiente metodologia de analisis basada en los comandos aprendidos:

    Paso 1: Localizar los procesos con mayor consumo mediante ps aux --sort=-%cpu o ps aux --sort=-%mem.

    Paso 2: Identificar los datos clave del proceso problematico: PID, usuario propietario, comando ejecutado, estado actual (STAT), prioridad (NI) y proceso padre (PPID).

    Paso 3: Verificar la relacion jerarquica utilizando pstree para comprender si el proceso depende de alguna aplicacion.

    Paso 4: Tomar una decision segura. Si se trata de un proceso del usuario, se procede a su finalizacion segura utilizando kill  despues de haber comprobado su PID real.


