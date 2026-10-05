

---

### 🧠 Preguntas de investigación

**1. ¿Qué diferencia existe entre autenticación y autorización?**

* **Autenticación:** Es el proceso de **verificar quién eres** (por ejemplo, cuando introduces tu usuario y contraseña). Demuestra que eres la persona o cuenta que dices ser.
* **Autorización:** Es el proceso de **verificar qué puedes hacer** una vez que ya entraste al sistema. Define los permisos y accesos que tienes sobre archivos, comandos o recursos.

**2. ¿Qué diferencia existe entre usuario y grupo?**

* **Usuario:** Es una cuenta individual dentro del sistema (como `msfadmin` o `root`) que tiene un identificador único (UID) y un entorno personal.
* **Grupo:** Es una colección o conjunto de usuarios. Sirve para administrar los permisos de forma colectiva; si varios usuarios necesitan acceder a los mismos archivos, en lugar de dárselos uno por uno, se les asigna a un grupo.

**3. ¿Qué significa UID?**

* Significa *User Identifier* (Identificador de Usuario). Es un número único que el sistema operativo Linux asigna internamente a cada cuenta de usuario para distinguirla, en lugar de depender únicamente del nombre de texto.

**4. ¿Qué significa GID?**

* Significa *Group Identifier* (Identificador de Grupo). Es un número único asignado a cada grupo dentro del sistema para controlarlos y administrarlos a nivel de kernel y permisos.

**5. ¿Qué usuario tiene UID 0?**

* El usuario **`root`** (el superadministrador del sistema), el cual cuenta con privilegios totales y sin restricciones sobre todo el sistema operativo.

**6. ¿Qué significa `rwx`?**

* Son los tres permisos básicos en Linux:
* **`r`** (*read* / lectura): Permite ver el contenido de un archivo o listar los elementos de un directorio.
* **`w`** (*write* / escritura): Permite modificar, crear o eliminar archivos y carpetas.
* **`x`** (*execute* / ejecución): Permite ejecutar archivos como programas/scripts o ingresar a un directorio.



**7. ¿Qué diferencia existe entre `chmod`, `chown` y `chgrp`?**

* **`chmod`:** Modifica los **permisos** de lectura, escritura y ejecución de un archivo o directorio.
* **`chown`:** Cambia al **propietario** (*owner*) de un archivo o directorio.
* **`chgrp`:** Cambia el **grupo** al que pertenece un archivo o directorio.

**8. ¿Qué es SUID?**

* Significa *Set User ID*. Es un permiso especial de Linux que se le aplica a los archivos ejecutables. Cuando un usuario ejecuta un archivo con SUID activado, el programa se ejecuta temporalmente con los **privilegios del propietario del archivo** (por ejemplo, `root`) en lugar de los privilegios del usuario común que lo lanzó.

**9. ¿Por qué `/etc/shadow` debe estar protegido?**

* Porque contiene las **contraseñas de los usuarios en formato cifrado (hash)**. Si tuviera permisos de lectura públicos, cualquier atacante podría copiar los hashes e intentar descifrarlos fuera de línea mediante ataques de fuerza bruta o diccionarios.

**10. ¿Por qué `chmod 777` puede ser peligroso?**

* Porque otorga permisos totales de lectura, escritura y ejecución **a todo el mundo** (*propietario, grupo y otros*). Cualquier usuario local o proceso malicioso podría modificar, borrar o reemplazar archivos críticos del sistema, comprometiendo la seguridad por completo.

**11. ¿Por qué Telnet representa un riesgo frente a SSH?**

* Porque **Telnet transmite toda la información (incluyendo el usuario y la contraseña) en texto plano** a través de la red, lo que permite que cualquier atacante interceptando el tráfico (*sniffing*) pueda robar las credenciales fácilmente. **SSH**, en cambio, cifra toda la comunicación de extremo a extremo.

**12. ¿Qué significa "principio de mínimo privilegio"?**

* Es una regla fundamental de seguridad que consiste en darle a cada usuario, programa o servicio **únicamente los permisos estrictamente necesarios** para realizar su trabajo, y nada más, reduciendo el impacto en caso de un compromiso.
