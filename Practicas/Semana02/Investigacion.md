Investigación  - Sistemas Operativos
1. ¿Qué es un programa?
Un programa son instrucciones escritas en un lenguaje de programación, guardadas en el almacenamiento secundario como el disco duro o SSD. Es un archivo pasivo que por sí solo no realiza ninguna acción hasta que se le indica que se ejecute.

2. ¿Qué es un proceso?
Un proceso es un programa en estado dinámico de ejecución. Ocurre cuando el sistema operativo toma el archivo estático del programa, lo carga en la memoria principal RAM y le asigna recursos del sistema como espacio de memoria

3. ¿Qué diferencia existe entre un programa y un proceso?
Programa: Es pasivo, estático, reside en el disco y no consume recursos de CPU ni memoria mientras no se ejecute.
Proceso: Es activo, dinámico, se encuentra en la memoria RAM y consume recursos del sistema operativo como CPU y memoria mientras está en marcha. 

4. ¿Por qué un mismo programa puede generar varios procesos?
Porque el sistema operativo permite abrir múltiples instancias de una misma aplicación de forma independiente. Cada instancia que se ejecuta obtiene su propio espacio de memoria y su propio identificador único, lo que evita que interfieran entre sí, por ejemplo, al abrir varias terminales o ejecutar múltiples comandos sleep al mismo tiempo.

5. ¿Qué es un PID?
Es un número de identificación único que el núcleo del sistema operativo asigna a cada proceso en el momento de su creación. Sirve para distinguir a cada proceso activo dentro del sistema y permite administrarlo.

6. ¿Qué es un PPID?
Es el Identificador del Proceso Padre, conocido en inglés como Parent Process ID. Indica el PID del proceso que creó o invocó al proceso actual. 

7. Concepto de Multiprogramación
Es una técnica de administración en los sistemas operativos que permite que múltiples procesos residan simultáneamente en la memoria principal y se alternen el uso de la CPU. 

8. Prioridades (NI / nice) y Estados del Proceso (STAT)
Prioridad / Nice (NI): Es la prioridad con el que se ejecuta un proceso. 
Estados del Proceso (STAT): Letras o códigos que indican la situación actual del ciclo de vida del proceso en el sistema, por ejemplo, R para procesos en ejecución y S para procesos en suspensión.
