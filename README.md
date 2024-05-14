# SGCE
SGCE(Sistema de gestión convivencia escolar) es un proyecto que busca registrar información acerca de los estudiantes respecto a su comportamiento convivencial para hacer procesos de mejora.

**Autor: John Muñoz**

**Descripción del sistema**

El sistema se centra en el estudiante el cual pertenece a un colegio que a su vez pertenece a un DILE(Dirección local de educación), por este motivo se relacionan de *uno a muchos*. Como responsables del proceso eductivo se encuentran los docentes y acudientes los cuales pueden tener uno o varios estudiantes por eso se relaciona como de *muchos a muchos*. Para finalizar se encuentran las dificultades convivenciales que un estudiante puede cometer, retardos, evasiones, fallas y falta de uniforme. A continuación se relaciona el diagrama inicial:

![Modelo entidad relación](https://github.com/Lic-JohnM/SGCE/blob/main/Modelo-entidad-relacion.png)

*Descripción de tablas*

Usando MySQL Workbench se generan las tablas como se muestra a continuación:

![Estructuras tablas](https://github.com/Lic-JohnM/SGCE/blob/main/Estructuras-tabla.png)

Todas las tablas tienen un ID que representa la llave primaria, en el caso de instituciones como el colegio o la DILE, tiene su ubicación, teléfono y nombre. En el caso de las personas como lo son los estudiantes, docentes o acudientes tienen el nombre, apellido, en el caso de acudientes número de celular. 

Las faltas convivenciales tienen una fecha así como la asignatura en la que se cometio.

*Adjunto en el respositorio se encuentra el sql de la actividad.*

