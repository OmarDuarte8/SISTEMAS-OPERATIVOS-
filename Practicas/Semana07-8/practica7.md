# 🔗 PRÁCTICA 07 — JOIN Y RELACIONES ENTRE TABLAS

**Asignatura:** Taller de Base de Datos 
**Alumno:** Omar Said Duarte Ruiz 
**Matrícula:** 242240010 
**Base de Datos:** `control_escolar` 


---

## 🛠️ 1. ESTRUCTURA Y RELACIONES

Esta práctica conecta las tablas de la base de datos `control_escolar` (`alumnos`, `grupos`, `materias` y `calificaciones`) a través de sus llaves primarias y foráneas.

---

## 🔗 2. CONSULTAS DE LA PRÁCTICA 07

### 1️⃣ Coincidencias Exactas (`INNER JOIN`)
Relaciona a los alumnos con la tabla de grupos para mostrar únicamente los estudiantes que tienen asignado un grupo.

```sql
USE control_escolar;

SELECT 
    a.nombre_completo AS alumno, 
    g.nombre_grupo, 
    g.turno
FROM alumnos AS a
INNER JOIN grupos AS g 
    ON a.grupo_id = g.id;
```

---

### 2️⃣ Conservar Todos los Registros de la Tabla Izquierda (`LEFT JOIN`)
Muestra la lista completa de alumnos, indicando su grupo o mostrando `NULL` si no tienen asignado ninguno.

```sql
SELECT 
    a.nombre_completo AS alumno, 
    g.nombre_grupo
FROM alumnos AS a
LEFT JOIN grupos AS g 
    ON a.grupo_id = g.id;
```

---

### 3️⃣ Detección de Registros Sin Relación (`IS NULL`)
Filtra la consulta anterior para identificar explícitamente a los alumnos que **no** tienen asignado un grupo.

```sql
SELECT 
    a.nombre_completo AS alumno_sin_grupo
FROM alumnos AS a
LEFT JOIN grupos AS g 
    ON a.grupo_id = g.id
WHERE g.id IS NULL;
```

---

### 4️⃣ Unión de Tres Tablas (`INNER JOIN` Múltiple)
Conecta las tablas `alumnos`, `calificaciones` y `materias` para obtener la calificación final de cada alumno en sus asignaturas.

```sql
SELECT 
    a.nombre_completo AS alumno,
    m.nombre_materia,
    c.calificacion
FROM alumnos AS a
INNER JOIN calificaciones AS c ON a.id = c.alumno_id
INNER JOIN materias AS m ON c.materia_id = m.id;
```

---

## 💻 3. COMANDO DE EJECUCIÓN DESDE TERMINAL

```bash
mysql -u root -p control_escolar < 07_joins/joins.sql
```
