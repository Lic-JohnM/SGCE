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

**Creación de vistas**

Una vez se suben los archivos a la base de datos, se generan vistas para ver el id_uniforme y la fecha según la asignatura

CREATE VIEW vista_inasistencias_matematica AS
SELECT IdFallasuniforme, Fecha, Materia
FROM fallasuniforme
WHERE Materia = 'Matemática';

CREATE VIEW vista_inasistencias_ingles AS
SELECT IdFallasuniforme, Fecha, Materia
FROM fallasuniforme
WHERE Materia = 'Ingles';

**Creación de funciones**

Se crea una función donde  según el número del ID se obtenga la fecha, la creación se realiza de la siguiente manera

CREATE FUNCTION obtenerFechaInasistencia(Id INT)
RETURNS VARCHAR(255)
DETERMINISTIC
BEGIN
    DECLARE resultado VARCHAR(255);

    SELECT Fecha INTO resultado
    FROM Fallasuniforme
    WHERE IdFallasuniforme = Id;

    RETURN resultado;
END

y la siguiente función donde apartir del identificador se obtiene la materia

CREATE FUNCTION obtenerMateriaInasistencia(Id INT)
RETURNS VARCHAR(255)
DETERMINISTIC
BEGIN
    DECLARE resultado VARCHAR(45);

    SELECT Materia INTO resultado
    FROM Fallasuniforme
    WHERE IdFallasuniforme = Id;

    RETURN resultado;
END

**Creación de procedimientos**

A continuación se crea un procedimiento donde se insertan nuevos datos en la tabla:

CREATE DEFINER=`root`@`localhost` PROCEDURE `InsertarFallaUniforme`(
  IN Id_u int8(255),
  IN p_Fecha VARCHAR(255),
   IN p_Materia VARCHAR(45))
BEGIN

    INSERT INTO fallasuniforme (IdFallasuniforme,Fecha, Materia)
    VALUES (Id_u,p_Fecha, p_Materia);

    -- Seleccionar un mensaje de confirmación
    SELECT 'Registro insertado exitosamente' AS Mensaje;
END


**Creación de una transacción**

A continuación se ingresa un valor en la base de datos en donde si le quitamos el commit no se vera reflejado en el mismo y si existe un error el ROLLBACK lo dejara como estaba y vuelve a dejar la base de datos en donde se creo el savepoint.

USE mydb;

START TRANSACTION;

-- Definir un SAVEPOINT
SAVEPOINT mi_savepoint;

-- Inserción de datos
INSERT INTO evasiones (IdEvasiones, name, Materia)
VALUES ('199', 'Alberto', 'Español');

-- Confirmar la transacción
COMMIT;
-- Si ocurre algun error
ROLLBACK TO mi_savepoint;
