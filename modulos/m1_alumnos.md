# Modulo: Alumnos 

>
>Equipo 1 
>


## 1. Resumen 
El Módulo de Alumnos comprende la interfaz y lógica del sistema orientada al uso directo por parte del estudiante.
Este módulo centraliza las herramientas académicas y administrativas esenciales para el alumno, permitiéndole consultar su 
rendimiento escolar, gestionar su información personal, realizar trámites de inscripción/reinscripción y solicitar 
cambios o documentos oficiales sin necesidad de acudir presencialmente a las oficinas administrativas.
## 2. Objetivos y Alcance 
### 2.1. Objetivo General
Proporcionar a los alumnos un punto de acceso unificado, intuitivo y seguro para autogestionar su perfil, consultar su 
información académica en tiempo real y realizar trámites administrativos institucionales.

### 2.2. Objetivos Específicos
* Permitir la autenticación y actualización de datos personales, de contacto y datos del alumno.
* Facilitar la consulta en tiempo real de calificaciones, horarios, materias inscritas e historial académico.
* Automatizar la generación y descarga de documentos oficiales como la constancia de estudios.
* Agilizar los procesos de reinscripción.


### 2.3. Alcance
* **Dentro del alcance:**
  * Autenticación y gestión de perfil del alumno (cambio de contraseña, datos personales, domicilio y contacto).
  * Consultas académicas (calificaciones del periodo, historial, horario de clases Y materias inscritas).
  * Trámites y solicitudes (proceso de reinscripción, estado del trámite).
  * Descargas de constancias en formato digital (PDF).
* **Fuera del alcance:**
  * Captura de calificaciones (función del Módulo de Profesores).
* **RF-07 (Consultar horario):** El sistema permitirá visualizar la distribución de clases asignadas por día y hora para el ciclo vigente.
* **RF-08 (Consultar materias inscritas):** El alumno podrá verificar la lista de asignaturas asignadas en su carga académica actual.
* **RF-09 (Descargar constancia de estudios):** El sistema generará un documento PDF oficial descargable que certifique la condición académica del alumno.
* **RF-10 (Cambiar contraseña):** El alumno podrá actualizar su clave de acceso previa validación de su contraseña actual.
* **RF-11 (Consultar estado de reinscripción):** El alumno podrá verificar el estatus del trámite de su reinscripción (En proceso, Aprobado, Rechazado o Con observaciones).

## 4. ACTORES
1. Alumnos
2. Profesores
3. Coordinador
4. Personal administrativo




## 5. DIAGRAMA DE CASOS DE USO

left to right direction
actor "Alumno" as A

rectangle "Módulo de Alumnos" {
    usecase "Iniciar sesión" as UC1
    usecase "Editar datos generales" as UC2
    usecase "Iniciar reinscripción" as UC3
    usecase "Consultar datos personales" as UC4
    usecase "Consultar calificaciones" as UC5
    usecase "Consultar historial académico" as UC6
    usecase "Consultar horario" as UC7
    usecase "Consultar materias inscritas" as UC8
    usecase "Descargar constancia de estudios" as UC9
    usecase "Cambiar contraseña" as UC10
    usecase "Consultar estado de reinscripción" as UC11
}
A --> UC1
A --> UC2
A --> UC3
A --> UC4
A --> UC5
A --> UC6
A --> UC7
A --> UC8
A --> UC9
A --> UC10
A --> UC11

## 6. GLOSARIO DE ABREVIATURAS 
* **RF:** Requerimiento Funcional.
* **RNF:** Requerimiento No Funcional.
* **UC:** Caso de Uso.
* **UI:** Interfaz de Usuario.
* **PDF:** Formato de Documento Portable.

## 7. REQUERIMIENTOS NO FUNCIONALES 
* **RNF-01 (Autenticación):** El sistema debe requerir credenciales válidas para el acceso al módulo.
* **RNF-02 (Cifrado de Contraseñas):** Las contraseñas almacenadas deben ser encriptadas mediante algoritmos de cifrado seguros.
* **RNF-03 (Sesiones Expiradas):** La sesión del alumno se cerrará automáticamente tras 15 minutos de inactividad.
* **RNF-04 (Tiempo de Respuesta):** Las peticiones de consulta deben responder en un tiempo menor a 5 segundos en condiciones normales de tráfico.
* **RNF-05 (Generación de PDF):** La descarga de la constancia de estudios en formato PDF no debe tomar más de 3 segundos.
* **RNF-06 (Disponibilidad):** El módulo debe mantener una disponibilidad mínima del 99% en períodos de reinscripción crítica.
* **RNF-07 (Diseño Responsivo):** La interfaz debe adaptarse correctamente a dispositivos móviles, tabletas y computadoras de escritorio.
* **RNF-07 (Accesibilidad):** Los elementos visuales deben cumplir con un contraste adecuado y navegación clara para facilitar su uso.