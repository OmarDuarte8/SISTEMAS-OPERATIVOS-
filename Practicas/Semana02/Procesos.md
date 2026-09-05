Analisis y Observacion de Procesos
1. Identificacion del proceso de la terminal

Para conocer el contexto actual de trabajo en la terminal de Ubuntu, se utilizaron comandos basicos para identificar al usuario activo y el identificador de la shell actual.
Comando utilizado para el usuario: whoami
Resultado obtenido: El usuario actual en sesion es omar8.
Comando utilizado para la shell: echo $$
Resultado obtenido: El PID de la terminal o shell en ejecucion corresponde al proceso actual activo.
2. Observacion general con ps y ps aux

Mediante el uso del comando ps fue posible visualizar los procesos directamente asociados a la terminal actual, mostrando principalmente la shell bash y el propio comando de consulta.
Al utilizar el comando ps aux se obtuvo una fotografia global y detallada de todos los procesos activos en el sistema operativo, abarcando las columnas correspondientes al usuario propietario, el PID, los porcentajes de consumo de CPU y memoria RAM, el estado del proceso, la hora de inicio y el comando exacto que los origino.
3. Analisis de consumo de recursos

Utilizando los filtros de ordenamiento en ps aux:

    Al ordenar por mayor consumo de CPU (ps aux --sort=-%cpu), se identificaron los procesos que demandan mayor procesamiento por parte del nucleo.

    Al ordenar por mayor consumo de memoria (ps aux --sort=-%mem), se visualizaron las aplicaciones que retienen mayor espacio en la memoria RAM principal.
    Conclusion del analisis: El proceso que consume mayor cantidad de CPU no necesariamente es el que mas memoria consume, ya que esto depende totalmente de la naturaleza de la aplicacion, ya sea un calculo pesado o una interfaz grafica cargada.

4. Relacion entre procesos padre e hijo (pstree)

Mediante el uso de pstree se logro observar la estructura jerarquica del sistema operativo. Los procesos no actuan de manera aislada; existe un proceso padre que genera o bifurca procesos hijos. En la salida se observo como la shell bash funciona como proceso padre que contiene y administra los comandos ejecutados debajo de ella.
5. Creacion y administracion de un proceso de laboratorio (sleep)

Para experimentar de forma segura y controlada, se ejecuto un proceso de laboratorio utilizando el comando sleep 300, el cual simula una pausa prolongada.
Abriendo una pestana adicional en la terminal, se localizo el proceso mediante la busqueda filtrada:
Comando utilizado: ps aux | grep sleep
Resultado: Se identifico el PID exacto asignado por el sistema operativo al proceso sleep creado por el usuario.
Finalizacion segura: Una vez identificado con certeza el PID propio de laboratorio, se procedio a solicitar su finalizacion mediante el comando kill seguido del numero de PID correspondiente, verificando posteriormente con ps aux que el proceso habia desaparecido de la lista de ejecucion de manera exitosa.
6. Multiprogramacion

Para comprobar el concepto de multiprogramacion, se ejecutaron multiples instancias del comando sleep de forma simultanea utilizando el simbolo de segundo plano y abriendo ejecuciones independientes.
Al revisar con ps aux | grep sleep, se comprobo que cada ejecucion genera un proceso totalmente independiente, cada uno con su propio PID unico, coexistiendo al mismo tiempo y compartiendo de forma alternada los recursos del procesador y del sistema operativo.
