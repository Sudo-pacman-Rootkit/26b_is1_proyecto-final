# Módulo Alumnos

> Equipo 1: Alumnos

# Resumen

El Módulo de Alumnos comprende la interfaz y lógica del sistema orientada al uso directo por parte del estudiante.

Centraliza las herramientas académicas y administrativas que permiten al alumno consultar su rendimiento escolar, gestionar su información personal, realizar trámites de inscripción y reinscripción, y solicitar documentos oficiales sin acudir presencialmente a las oficinas administrativas.

# Objetivo y Alcance

## Objetivo general

Proporcionar a los alumnos un acceso unificado, intuitivo y seguro para gestionar su perfil, consultar su información académica y realizar trámites administrativos institucionales.

## Objetivos específicos

- Permitir la autenticación y actualización de datos personales y de contacto.
- Facilitar la consulta de calificaciones, horarios, materias inscritas e historial académico.
- Generar y permitir la descarga de documentos oficiales, como la constancia de estudios.
- Agilizar los procesos de reinscripción.

## Alcance

**Dentro del alcance:**

- Inicio de sesión y gestión del perfil (contraseña, datos personales, domicilio y contacto).
- Consultas académicas (calificaciones, historial, horario y materias inscritas).
- Trámites de reinscripción y consulta de su estado.
- Descarga de constancias de estudios en PDF.

**Fuera del alcance:**

- Captura de calificaciones (correspondiente al Módulo de Profesores).

# Requerimientos funcionales

- **RF-07 (Consultar horario):** El alumno podrá visualizar sus clases por día y hora del ciclo vigente.
- **RF-08 (Consultar materias inscritas):** El alumno podrá consultar las materias de su carga académica actual.
- **RF-09 (Descargar constancia):** El sistema generará una constancia oficial de estudios en PDF para su descarga.
- **RF-10 (Cambiar contraseña):** El alumno podrá cambiar su contraseña validando primero la actual.
- **RF-11 (Consultar reinscripción):** El alumno podrá consultar el estado de su reinscripción: en proceso, aprobado, rechazado o con observaciones.

# Actores

| Actor | Descripción y funciones |
|---|---|
| Alumno | Usuario del sistema que puede iniciar sesión, gestionar sus datos personales, consultar información académica, realizar trámites de reinscripción y descargar constancias de estudios. |

# Diagrama de casos de uso

**Código fuente UML:**

```@startuml
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

@enduml

```

**Imagen del diagrama:**

[Diagrama de casos de uso - Módulo Alumnos](resources/imgs/m1_alumnos_casos_de_uso.png)

# Diagrama de componentes

**Nota:** Generar un diagrama de componentes por cada caso de uso.

**Código fuente UML:**

<!-- Agregar aqui el enlace al archivo del codigo fuente UML -->

**Imagen de los diagramas:**

<!-- Agregar aqui las imagenes de los diagramas de componentes -->

# Diagrama de secuencia

**Nota:** Generar un diagrama de secuencia por cada caso de uso.

**Código fuente UML:**

<!-- Agregar aqui el enlace al archivo del codigo fuente UML -->

**Imagen de los diagramas:**

<!-- Agregar aqui las imagenes de los diagramas de secuencia -->

# Diagrama de estados



**Código fuente UML:**

<!-- Agregar aqui el enlace al archivo del codigo fuente UML -->

**Imagen del diagrama:**

<!-- Agregar aquí la imagen del diagrama de estados. -->

# Diagrama de clases

**Código fuente UML:**

<!-- Agregar aquí el enlace al archivo del código fuente UML. -->

**Imagen del diagrama:**

<!-- Agregar aquí la imagen del diagrama de clases. -->

# Glosario y Abreviaturas

- **RF:** Requerimiento Funcional.
- **RNF:** Requerimiento No Funcional.
- **UC:** Caso de Uso.
- **UI:** Interfaz de Usuario.
- **PDF:** Formato de Documento Portable.

# Requerimientos No Funcionales

- **RNF-01 (Autenticación):** El sistema debe requerir credenciales válidas para acceder al módulo.
- **RNF-02 (Cifrado de contraseñas):** Las contraseñas almacenadas deben protegerse mediante algoritmos de cifrado seguros.
- **RNF-03 (Sesiones expiradas):** La sesión del alumno se cerrará automáticamente después de 15 minutos de inactividad.
- **RNF-04 (Tiempo de respuesta):** Las consultas deben responder en menos de 5 segundos en condiciones normales.
- **RNF-05 (Generación de PDF):** La descarga de constancias no debe tardar más de 3 segundos.
- **RNF-06 (Disponibilidad):** El módulo debe mantener una disponibilidad mínima del 99% durante los periodos críticos de reinscripción.
- **RNF-07 (Diseño responsivo):** La interfaz debe adaptarse a celulares, tabletas y computadoras.
- **RNF-08 (Accesibilidad):** Los elementos visuales deben tener contraste adecuado y una navegación clara.