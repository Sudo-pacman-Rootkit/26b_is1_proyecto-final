# Módulo Calendario de Exámenes

>
> Equipo 5
> (Jorge Uriel Vidal Morales, Melany Joana Toledo Escamilla, Alan Daniel Salas Gutierrez, Samuel Riveroll Vargas)
>

## Resumen
El **Módulo de Calendario de Exámenes** es la base para la planificación, organización y publicación de las evaluaciones académicas dentro del sistema de control escolar universitario. Su objetivo principal es automatizar y gestionar la programación de los diferentes tipos de evaluaciones (parciales, ordinarios, extraordinarios y título de suficiencia), garantizando la disponibilidad de aulas, evitando el traslape de horarios para alumnos y docentes, y facilitando la consulta centralizada en tiempo real.

## Objetivo y Alcance
### Objetivo
Automatizar y gestionar la planificación, organización y publicación de las evaluaciones académicas, asegurando la disponibilidad de espacios y evitando cruces de horarios para la comunidad estudiantil y docente.

### Alcance
Comprende la definición de periodos de evaluación, asignación de fechas, horarios y aulas, detección automática de conflictos por traslapes, publicación de calendarios y consultas personalizadas para alumnos, profesores y administradores.

## Requerimientos funcionales 
1. **RF-01 Programación de Periodos de Exámenes:** Permitir a la administración definir las ventanas de fechas hábiles para exámenes parciales y finales según el calendario lectivo.
2. **RF-02 Asignación de Horarios y Aulas:** Permitir la asignación de fecha, hora inicio/fin y aula física o virtual para cada asignatura.
3. **RF-03 Detección Automática de Conflictos (Validación de Traslape):** Validar que ningún estudiante o profesor tenga dos exámenes programados en el mismo bloque horario.
4. **RF-04 Consulta Personalizada del Estudiante:** Visualización del calendario de exámenes individual filtrado por la carga académica inscrita del alumno.
5. **RF-05 Consulta del Docente / Aplicador:** Visualización del calendario de aplicación de exámenes asignados y lista de asistencia.

### Actores
| Actor | Descripción |
|---|---|
| Administrador / Coordinador | Personal administrativo o académico autorizado para gestionar la programación de evaluaciones dentro del sistema de control escolar. Es responsable de organizar los calendarios de exámenes conforme a los periodos y lineamientos académicos establecidos. |
| Alumno | Estudiante inscrito en un programa educativo de la UAEMéx, con una carga académica registrada en el sistema. Es el usuario al que corresponde la información de las evaluaciones de sus asignaturas. |
| Profesor / Aplicador | Personal académico autorizado para aplicar o supervisar evaluaciones. Se encuentra vinculado a las asignaturas o grupos que tiene asignados y es responsable de la aplicación de los exámenes correspondientes. |


## Diagrama de casos de uso

```plantuml
@startuml m5_calendario_examenes_casos_uso
left to right direction
skinparam actorStyle awesome
skinparam packageStyle rectangle

actor "Alumno" as alumno
actor "Profesor /\nAplicador" as profesor
actor "Administrador /\nCoordinador" as admin

rectangle "MÓDULO DE CALENDARIO DE EXÁMENES" {
  usecase "Definir periodos de exámenes\n(parcial, ordinario,\nextraordinario y título)" as UC1
  usecase "Asignar fecha, horario y aula\na cada examen" as UC2
  usecase "Validar conflictos\n(sin traslapes de alumnos\nni profesores)" as UC3
  usecase "Modificar o cancelar\nprogramación de examen" as UC4
  usecase "Publicar calendario\nde exámenes" as UC5
  usecase "Consultar calendario\npersonal de exámenes" as UC6
  usecase "Consultar calendario de\naplicación de exámenes" as UC7
  usecase "Consultar lista\nde asistencia" as UC8

  UC2 ..> UC3 : <<include>>
  UC4 ..> UC3 : <<include>>
}

admin --> UC1
admin --> UC2
admin --> UC4
admin --> UC5
alumno --> UC6
profesor --> UC7
profesor --> UC8
@enduml
```
![Diagrama de casos de uso del Módulo 5. Calendario de Exámenes](resources/imgs/m5_calendarioExs_casos_uso.png)

## Descripción
- **UC1: Definir periodos de exámenes:** Permite al Administrador/Coordinador establecer las ventanas de fechas hábiles para las distintas evaluaciones académicas (parciales, ordinarios, extraordinarios y título de suficiencia).
  
- **UC2: Asignar fecha, horario y aula a cada examen:** Permite fijar los parámetros espacio-temporales para la aplicación de cada evaluación dentro del calendario lectivo.
  
- **UC3: Validar conflictos (sin traslapes) `<<include>>`:** Proceso automatizado ejecutado durante la asignación o modificación para garantizar que ningún alumno o docente tenga evaluaciones empalmadas en el mismo bloque de tiempo.
  
- **UC4: Modificar o cancelar programación de examen:** Permite ajustar parámetros o suspender evaluaciones previamente registradas actualizando el estado de la agenda.
  
- **UC5: Publicar calendario de exámenes:** Hace oficial la programación para que quede disponible para consulta pública dentro del sistema.
  
- **UC6: Consultar calendario personal de exámenes:** Permite al Alumno visualizar las fechas, horarios y aulas asignadas únicamente para las asignaturas en las que se encuentra inscrito.
  
- **UC7: Consultar calendario de aplicación de exámenes:** Muestra al Profesor/Aplicador el cronograma de los exámenes donde participará como responsable o aplicador.
  
- **UC8: Consultar lista de asistencia:** Permite al Profesor/Aplicador obtener la relación oficial de alumnos habilitados para presentar la evaluación en el aula correspondiente.


##  Diagramas de Contexto (Nivel 0 y Nivel 1)
```plantuml
@startuml Modulo5_Calendario_Examenes_Completo
title Módulo 5: Calendario de Exámenes (Nivel 0 y Nivel 1)

skinparam defaultTextAlignment center
skinparam packageStyle rectangle
skinparam shadowing false

package "Nivel 0 (Contexto)" {
    rectangle "Modulo de Aulas" as aulas0
    rectangle "Modulo de Inscripciones" as insc0
    rectangle "Servicio de Autenticación" as auten0
    
    storage "Sistema de Control Escolar\n[Calendario de Exámenes]" as sistema0
    
    rectangle "Servicio de Notificaciones" as notif0

    aulas0 --> sistema0 : Solicita Disponibilidad
    sistema0 --> aulas0 : Aulas Disponibles
    insc0 --> sistema0 : Carga académica y Grupos\npor Alumno/Profesor
    auten0 --> sistema0 : Identidad y rol del usuario

    sistema0 --> notif0 : Avisos
}

package "Nivel 1 (Contenedores / Subsistemas)" {
    rectangle "Profesor/Aplicador" as prof1
    rectangle "Administrador/Coordinador" as admin1
    rectangle "ALUMNOS" as alum1

    rectangle "Frontend Web" as app1

    storage "Módulo de Aulas" as modAulas1

    package "API Backend" {
        storage "Servicio de Programación" as prog1
        storage "Servicio de Consulta" as consulta1
        storage "Servicios de Administración" as adminSvc1
        storage "Validador de Conflicto" as valid1
    }

    database "Cola de Mensajes" as queue1
    database "Cache" as cache1
    database "Base de Datos Relacional" as db1

    storage "Servicio de Notificaciones" as notifSvc1
    storage "Módulo de Inscripciones" as modInsc1

    ' Conexiones Nivel 1
    prof1 --> app1 : HTTP/S
    admin1 --> app1 : HTTP/S
    alum1 --> app1 : HTTP/S

    app1 --> prog1 : HTTP/REST
    app1 --> consulta1 : HTTP/REST
    app1 --> adminSvc1 : HTTP/REST

    prog1 --> valid1
    modAulas1 --> valid1

    prog1 --> queue1
    consulta1 --> cache1
    consulta1 --> db1
    adminSvc1 --> db1

    queue1 --> notifSvc1
    notifSvc1 --> modInsc1
}
@enduml
```
![ Diagramas de Contexto (Nivel 0 y Nivel 1) Módulo 5. Calendario de Exámenes](resources/imgs/m5_CalendarioExs_contexto.png)

## Descripción
### Diagrama de Nivel 0 (Contexto)

- **Módulo de Aulas:** Mantiene una comunicación bidireccional solicitando disponibilidad de espacios y devolviendo el catálogo de aulas activas e instaladas.
  
- **Módulo de Inscripciones:** Provee la carga académica de asignaturas y la lista de grupos asociados tanto a alumnos como a docentes.
  
- **Servicio de Autenticación:** Transmite la validación de identidad y el rol activo del usuario para determinar los permisos de operación dentro del sistema.
  
- **Sistema de Control Escolar (Calendario de Exámenes):** Procesa los datos recibidos para programar las evaluaciones y coordinar los recursos.
  
- **Servicio de Notificaciones:** Recibe la emisión de avisos y alertas del calendario para distribuirlos a los usuarios involucrados.
  

### Diagrama de Nivel 1 (Contenedores / Subsistemas)

- **Actores (Profesor/Aplicador, Administrador/Coordinador, ALUMNOS):** Interfazan con el sistema mediante peticiones seguras (`HTTP/S`) hacia la capa cliente.
  
- **Frontend Web:** Captura las interacciones de los usuarios y distribuye las peticiones mediante llamadas `HTTP/REST` hacia los servicios backend.
  
- **Servicios de Administración:** Gestionan la persistencia de configuraciones, modificaciones de periodos y parámetros.
  
  
- **Servicio de Programación:** Registra los eventos de examen, coordina la validación de espacios y publica eventos hacia la Cola de Mensajes.
  
- **Validador de Conflicto:** Proceso automatizado que interactúa con el Módulo de Aulas para asegurar la disponibilidad sin cruce de horarios.
  
- **Cola de Mensajes & Notificaciones:** Asincroniza la entrega de eventos hacia el Servicio de Notificaciones.

## Diagrama de componentes

Nota: Se incluye un diagrama por caso de uso.

### UC1. Definir periodos de exámenes

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC1. El administrador configura las ventanas de evaluación dentro de un periodo escolar existente. El gestor consulta el periodo vigente y los tipos de examen, valida las fechas y conserva la ventana de evaluación; no crea ni modifica periodos escolares.

**Código fuente UML:** [Ver archivo m5_uc1_componentes.puml](../docs/architecture/diagrams/m5_uc1_componentes.puml)

```plantuml
@startuml m5_uc1_componentes
title UC1 - Definir periodos de exámenes - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Administrador / Coordinador" as N0
package "Calendario de Exámenes" {
  component "Gestor de ventanas" as N2
  component "Interfaz de administración" as N3
  database "Repositorio Calendario" as N5
  component "Validador de fechas" as N6
}
component "Módulo de Catálogo de Exámenes" as N1
component "Módulo de Periodos Escolares" as N4
N0 --> N3 : Configura fechas
N3 --> N2 : Envía datos
N2 --> N4 : Consulta periodo
N2 --> N1 : Consulta tipos
N2 --> N6 : Valida ventana
N2 --> N5 : Guarda ventana
@enduml
```

**Imagen visible:**

![Componentes UC1 - Definir periodos de exámenes](resources/imgs/m5_uc1_componentes.png)

---

### UC2. Asignar fecha, horario y aula

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC2. El administrador selecciona materia, grupo, tipo de examen, profesor, horario y aula. El gestor consulta los módulos académicos y ejecuta UC3; únicamente cuando los datos son válidos solicita la asignación del espacio y registra el examen como programado.

**Código fuente UML:** [Ver archivo m5_uc2_componentes.puml](../docs/architecture/diagrams/m5_uc2_componentes.puml)

```plantuml
@startuml m5_uc2_componentes
title UC2 - Asignar fecha, horario y aula - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Administrador / Coordinador" as N0
package "Calendario de Exámenes" {
  component "Gestor de programación" as N3
  component "Interfaz de programación" as N5
  database "Repositorio Calendario" as N7
  component "Validador de conflictos" as N8
}
component "Módulo de Asignaturas" as N1
component "Módulo de Aulas y Grupos" as N2
component "Módulo de Inscripciones" as N4
component "Módulo de Profesores" as N6
N0 --> N5 : Captura examen
N5 --> N3 : Envía programación
N3 --> N1 : Consulta materia
N3 --> N4 : Consulta grupo
N3 --> N6 : Consulta aplicador
N3 --> N8 : Ejecuta UC3
N8 --> N2 : Consulta disponibilidad
N3 --> N2 : Solicita reserva
N3 --> N7 : Guarda examen
@enduml
```

**Imagen visible:**

![Componentes UC2 - Asignar fecha, horario y aula](resources/imgs/m5_uc2_componentes.png)

---

### UC3. Validar conflictos de programación

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC3. El validador cruza el intervalo propuesto con los exámenes vigentes del mismo alumnado y profesorado. Solicita al Módulo de Aulas y Grupos la comprobación de disponibilidad y aforo, y devuelve los motivos concretos de rechazo, si existen. Este caso se invoca desde UC2 y UC4.

**Código fuente UML:** [Ver archivo m5_uc3_componentes.puml](../docs/architecture/diagrams/m5_uc3_componentes.puml)

```plantuml
@startuml m5_uc3_componentes
title UC3 - Validar conflictos de programación - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
package "Calendario de Exámenes" {
  component "Gestor de programación" as N1
  database "Repositorio Calendario" as N3
  component "Validador de conflictos" as N4
}
component "Módulo de Aulas y Grupos" as N0
component "Módulo de Inscripciones" as N2
N1 --> N4 : Solicita UC3
N4 --> N3 : Consulta programación
N4 --> N2 : Consulta alumnos
N4 --> N0 : Valida aula y aforo
N4 --> N1 : Devuelve resultado
@enduml
```

**Imagen visible:**

![Componentes UC3 - Validar conflictos de programación](resources/imgs/m5_uc3_componentes.png)

---

### UC4. Modificar programación de examen

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC4. El administrador recupera una programación existente y propone cambios. El gestor comprueba permisos y estado, vuelve a ejecutar UC3, solicita la actualización de la reserva al Módulo de Aulas y Grupos y confirma el cambio en el calendario. La actualización debe preservar la consistencia entre ambos módulos.

**Código fuente UML:** [Ver archivo m5_uc4_componentes.puml](../docs/architecture/diagrams/m5_uc4_componentes.puml)

```plantuml
@startuml m5_uc4_componentes
title UC4 - Modificar programación de examen - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Administrador / Coordinador" as N0
package "Calendario de Exámenes" {
  component "Gestor de programación" as N2
  component "Interfaz de programación" as N4
  database "Repositorio Calendario" as N5
  component "Validador de conflictos" as N6
}
component "Módulo de Aulas y Grupos" as N1
component "Módulo de Inscripciones" as N3
N0 --> N4 : Propone cambio
N4 --> N2 : Envía modificación
N2 --> N5 : Lee examen
N2 --> N6 : Ejecuta UC3
N6 --> N3 : Consulta inscritos
N6 --> N1 : Consulta disponibilidad
N2 --> N1 : Actualiza reserva
N2 --> N5 : Actualiza examen
@enduml
```

**Imagen visible:**

![Componentes UC4 - Modificar programación de examen](resources/imgs/m5_uc4_componentes.png)

---

### UC5. Cancelar programación de examen

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC5. El administrador selecciona un examen vigente y confirma la cancelación. El sistema marca el registro como cancelado, solicita liberar la reserva vinculada en Aulas y Grupos y conserva el historial. No se necesita ejecutar la validación de traslapes para cancelar.

**Código fuente UML:** [Ver archivo m5_uc5_componentes.puml](../docs/architecture/diagrams/m5_uc5_componentes.puml)

```plantuml
@startuml m5_uc5_componentes
title UC5 - Cancelar programación de examen - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Administrador / Coordinador" as N0
package "Calendario de Exámenes" {
  component "Gestor de programación" as N2
  component "Interfaz de programación" as N3
  database "Repositorio Calendario" as N4
}
component "Módulo de Aulas y Grupos" as N1
N0 --> N3 : Confirma cancelación
N3 --> N2 : Solicita cancelación
N2 --> N4 : Consulta y marca cancelación
N2 --> N1 : Libera reserva
@enduml
```

**Imagen visible:**

![Componentes UC5 - Cancelar programación de examen](resources/imgs/m5_uc5_componentes.png)

---

### UC6. Publicar calendario de exámenes

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC6. El administrador solicita publicar programaciones revisadas. El gestor verifica que los registros sean válidos y publicables, cambia su estado a publicado y habilita su consulta para alumnos y profesores autorizados. No se presupone un servicio de notificaciones.

**Código fuente UML:** [Ver archivo m5_uc6_componentes.puml](../docs/architecture/diagrams/m5_uc6_componentes.puml)

```plantuml
@startuml m5_uc6_componentes
title UC6 - Publicar calendario de exámenes - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Administrador / Coordinador" as N0
package "Calendario de Exámenes" {
  component "Gestor de publicación" as N1
  component "Interfaz de administración" as N2
  database "Repositorio Calendario" as N3
  component "Validador de publicación" as N4
}
N0 --> N2 : Solicita publicar
N2 --> N1 : Envía solicitud
N1 --> N3 : Consulta programaciones
N1 --> N4 : Verifica estados
N1 --> N3 : Marca publicados
@enduml
```

**Imagen visible:**

![Componentes UC6 - Publicar calendario de exámenes](resources/imgs/m5_uc6_componentes.png)

---

### UC7. Consultar calendario personal de exámenes

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC7. El alumno autenticado solicita su calendario. El servicio de consultas obtiene su carga inscrita desde Inscripciones, filtra los exámenes publicados asociados a esos grupos y muestra fecha, horario, asignatura y aula. No permite consultar el calendario privado de otros alumnos.

**Código fuente UML:** [Ver archivo m5_uc7_componentes.puml](../docs/architecture/diagrams/m5_uc7_componentes.puml)

```plantuml
@startuml m5_uc7_componentes
title UC7 - Consultar calendario personal de exámenes - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Alumno" as N0
package "Calendario de Exámenes" {
  component "Interfaz de consulta" as N2
  database "Repositorio Calendario" as N3
  component "Servicio de consulta" as N4
}
component "Módulo de Inscripciones" as N1
N0 --> N2 : Consulta agenda
N2 --> N4 : Pide calendario
N4 --> N1 : Obtiene carga inscrita
N4 --> N3 : Filtra publicados
@enduml
```

**Imagen visible:**

![Componentes UC7 - Consultar calendario personal de exámenes](resources/imgs/m5_uc7_componentes.png)

---

### UC8. Consultar calendario de aplicación de exámenes

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC8. El profesor autenticado solicita los exámenes en los que figura como responsable. El servicio consulta sus asignaciones y recupera las programaciones publicadas, mostrando materia, grupo, fecha, horario y aula. Solo presenta sus propias asignaciones.

**Código fuente UML:** [Ver archivo m5_uc8_componentes.puml](../docs/architecture/diagrams/m5_uc8_componentes.puml)

```plantuml
@startuml m5_uc8_componentes
title UC8 - Consultar calendario de aplicación de exámenes - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Profesor / Aplicador" as N1
package "Calendario de Exámenes" {
  component "Interfaz de consulta" as N0
  database "Repositorio Calendario" as N2
  component "Servicio de consulta" as N3
}
N1 --> N0 : Consulta agenda
N0 --> N3 : Pide asignaciones
N3 --> N2 : Filtra profesor y estado
@enduml
```

**Imagen visible:**

![Componentes UC8 - Consultar calendario de aplicación de exámenes](resources/imgs/m5_uc8_componentes.png)

---

### UC9. Consultar lista de asistencia

**Descripción:** El diagrama de componentes identifica los participantes y dependencias necesarios para el caso UC9. El profesor elige uno de sus exámenes asignados. El sistema verifica esa asignación, obtiene los alumnos inscritos en el grupo y consulta los criterios de elegibilidad aplicables desde Catálogo de Exámenes o el módulo responsable. Muestra una lista para consulta, sin capturar asistencia ni calificaciones.

**Código fuente UML:** [Ver archivo m5_uc9_componentes.puml](../docs/architecture/diagrams/m5_uc9_componentes.puml)

```plantuml
@startuml m5_uc9_componentes
title UC9 - Consultar lista de asistencia - Componentes
left to right direction
skinparam componentStyle rectangle
skinparam shadowing false
actor "Profesor / Aplicador" as N3
package "Calendario de Exámenes" {
  component "Interfaz de consulta" as N2
  database "Repositorio Calendario" as N4
  component "Servicio de listas" as N5
}
component "Módulo de Catálogo de Exámenes" as N0
component "Módulo de Inscripciones" as N1
N3 --> N2 : Selecciona examen
N2 --> N5 : Solicita lista
N5 --> N4 : Verifica asignación
N5 --> N1 : Consulta inscritos
N5 --> N0 : Consulta requisitos
@enduml
```

**Imagen visible:**

![Componentes UC9 - Consultar lista de asistencia](resources/imgs/m5_uc9_componentes.png)

---

## Diagrama de secuencia

Nota: Se incluye un diagrama por caso de uso.

### UC1. Definir periodos de exámenes

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC1. El administrador configura las ventanas de evaluación dentro de un periodo escolar existente. El gestor consulta el periodo vigente y los tipos de examen, valida las fechas y conserva la ventana de evaluación; no crea ni modifica periodos escolares. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc1_secuencia.puml](../docs/architecture/diagrams/m5_uc1_secuencia.puml)

```plantuml
@startuml m5_uc1_secuencia
title UC1 - Definir periodos de exámenes - Secuencia
autonumber
actor "Administrador / Coordinador" as Admin
boundary "Interfaz de administración" as UI
control "Gestor de ventanas" as Gestor
participant "Periodos Escolares" as Periodos
participant "Catálogo de Exámenes" as Catalogo
control "Validador de fechas" as Validador
database "Repositorio Calendario" as BD
Admin -> UI : Abrir configuración de ventanas
UI -> Gestor : Solicitar datos iniciales
Gestor -> Periodos : Consultar periodo vigente
Periodos --> Gestor : Periodo y fechas permitidas
Gestor -> Catalogo : Consultar tipos activos
Catalogo --> Gestor : Tipos de examen
Gestor --> UI : Mostrar opciones
Admin -> UI : Capturar tipo y fechas
UI -> Gestor : Guardar ventana
Gestor -> Validador : Comprobar fechas y límites
Validador --> Gestor : Resultado
alt Fechas correctas
 Gestor -> BD : Registrar ventana
 BD --> Gestor : Identificador
 Gestor --> UI : Confirmar registro
else Fechas incorrectas
 Gestor --> UI : Explicar error y solicitar corrección
end
@enduml
```

**Imagen visible:**

![Secuencia UC1 - Definir periodos de exámenes](resources/imgs/m5_uc1_secuencia.png)

---

### UC2. Asignar fecha, horario y aula

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC2. El administrador selecciona materia, grupo, tipo de examen, profesor, horario y aula. El gestor consulta los módulos académicos y ejecuta UC3; únicamente cuando los datos son válidos solicita la asignación del espacio y registra el examen como programado. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc2_secuencia.puml](../docs/architecture/diagrams/m5_uc2_secuencia.puml)

```plantuml
@startuml m5_uc2_secuencia
title UC2 - Asignar fecha, horario y aula - Secuencia
autonumber
actor "Administrador / Coordinador" as Admin
boundary "Interfaz de programación" as UI
control "Gestor de programación" as Gestor
participant "Asignaturas" as Asignaturas
participant "Inscripciones" as Inscripciones
participant "Profesores" as Profesores
control "Validador de conflictos" as Validador
participant "Aulas y Grupos" as Aulas
database "Repositorio Calendario" as BD
Admin -> UI : Registrar examen con grupo, tipo, fecha, hora, aula y aplicador
UI -> Gestor : Enviar solicitud
Gestor -> Asignaturas : Verificar asignatura
Asignaturas --> Gestor : Datos
Gestor -> Inscripciones : Consultar grupo e inscritos
Inscripciones --> Gestor : Grupo e inscritos
Gestor -> Profesores : Verificar aplicador
Profesores --> Gestor : Profesor
Gestor -> Validador : Validar conflictos (UC3)
Validador -> BD : Consultar exámenes vigentes
BD --> Validador : Programaciones
Validador -> Aulas : Verificar disponibilidad y aforo
Aulas --> Validador : Resultado
Validador --> Gestor : Sin conflicto / motivos
alt Sin conflictos
 Gestor -> Aulas : Solicitar reserva de aula
 alt Reserva aceptada
  Aulas --> Gestor : Id de reserva
  Gestor -> BD : Guardar examen programado y reserva
  alt Registro exitoso
   BD --> Gestor : Confirmación
   Gestor --> UI : Programación registrada
  else Fallo al registrar
   Gestor -> Aulas : Liberar reserva recién creada
   Gestor --> UI : Operación no completada
  end
 else Reserva rechazada
  Aulas --> Gestor : Motivo
  Gestor --> UI : Solicitar otro horario o aula
 end
else Conflictos detectados
 Gestor --> UI : Mostrar motivos y pedir corrección
end
@enduml
```

**Imagen visible:**

![Secuencia UC2 - Asignar fecha, horario y aula](resources/imgs/m5_uc2_secuencia.png)

---

### UC3. Validar conflictos de programación

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC3. El validador cruza el intervalo propuesto con los exámenes vigentes del mismo alumnado y profesorado. Solicita al Módulo de Aulas y Grupos la comprobación de disponibilidad y aforo, y devuelve los motivos concretos de rechazo, si existen. Este caso se invoca desde UC2 y UC4. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc3_secuencia.puml](../docs/architecture/diagrams/m5_uc3_secuencia.puml)

```plantuml
@startuml m5_uc3_secuencia
title UC3 - Validar conflictos de programación - Secuencia
autonumber
control "Gestor de programación" as Gestor
control "Validador de conflictos" as Validador
database "Repositorio Calendario" as BD
participant "Inscripciones" as Inscripciones
participant "Aulas y Grupos" as Aulas
Gestor -> Validador : Validar propuesta y excluir registro editado si aplica
Validador -> BD : Consultar programaciones vigentes solapadas
BD --> Validador : Exámenes coincidentes
Validador -> Inscripciones : Consultar alumnos del grupo
Inscripciones --> Validador : Lista de alumnos
Validador -> Aulas : Comprobar disponibilidad y aforo
Aulas --> Validador : Disponible / conflicto
Validador -> Validador : Comparar intervalos de alumnos y aplicadores
alt Existe conflicto
 Validador --> Gestor : Rechazar y detallar coincidencias
else Sin conflictos
 Validador --> Gestor : Programación académicamente viable
end
@enduml
```

**Imagen visible:**

![Secuencia UC3 - Validar conflictos de programación](resources/imgs/m5_uc3_secuencia.png)

---

### UC4. Modificar programación de examen

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC4. El administrador recupera una programación existente y propone cambios. El gestor comprueba permisos y estado, vuelve a ejecutar UC3, solicita la actualización de la reserva al Módulo de Aulas y Grupos y confirma el cambio en el calendario. La actualización debe preservar la consistencia entre ambos módulos. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc4_secuencia.puml](../docs/architecture/diagrams/m5_uc4_secuencia.puml)

```plantuml
@startuml m5_uc4_secuencia
title UC4 - Modificar programación de examen - Secuencia
autonumber
actor "Administrador / Coordinador" as Admin
boundary "Interfaz de programación" as UI
control "Gestor de programación" as Gestor
database "Repositorio Calendario" as BD
control "Validador de conflictos" as Validador
participant "Inscripciones" as Inscripciones
participant "Aulas y Grupos" as Aulas
Admin -> UI : Seleccionar examen y proponer cambios
UI -> Gestor : Solicitar modificación
Gestor -> BD : Obtener examen y reserva actuales
BD --> Gestor : Datos y estado
alt Examen modificable
 Gestor -> Validador : Validar cambios (UC3, excluyendo examen actual)
 Validador -> Inscripciones : Consultar alumnos del grupo
 Inscripciones --> Validador : Inscritos
 Validador -> Aulas : Verificar nueva disponibilidad y aforo
 Aulas --> Validador : Resultado
 Validador --> Gestor : Viable / conflictos
 alt Viable
  Gestor -> Aulas : Solicitar cambio de reserva
  alt Reserva actualizada
   Aulas --> Gestor : Confirmación
   Gestor -> BD : Actualizar programación y referencia de reserva
   alt Actualización exitosa
    BD --> Gestor : Confirmación
    Gestor --> UI : Cambio guardado
   else Error de persistencia
    Gestor -> Aulas : Solicitar restauración de reserva anterior
    Gestor --> UI : No se completó el cambio
   end
  else Reserva rechazada
   Aulas --> Gestor : Motivo
   Gestor --> UI : Informar rechazo
  end
 else Conflictos
  Gestor --> UI : Mostrar causas
 end
else Estado no permite modificación
 Gestor --> UI : Denegar y explicar
end
@enduml
```

**Imagen visible:**

![Secuencia UC4 - Modificar programación de examen](resources/imgs/m5_uc4_secuencia.png)

---

### UC5. Cancelar programación de examen

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC5. El administrador selecciona un examen vigente y confirma la cancelación. El sistema marca el registro como cancelado, solicita liberar la reserva vinculada en Aulas y Grupos y conserva el historial. No se necesita ejecutar la validación de traslapes para cancelar. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc5_secuencia.puml](../docs/architecture/diagrams/m5_uc5_secuencia.puml)

```plantuml
@startuml m5_uc5_secuencia
title UC5 - Cancelar programación de examen - Secuencia
autonumber
actor "Administrador / Coordinador" as Admin
boundary "Interfaz de programación" as UI
control "Gestor de programación" as Gestor
database "Repositorio Calendario" as BD
participant "Aulas y Grupos" as Aulas
Admin -> UI : Seleccionar examen y confirmar cancelación
UI -> Gestor : Cancelar examen
Gestor -> BD : Consultar registro, reserva y estado
BD --> Gestor : Datos actuales
alt Examen vigente y cancelable
 Gestor -> BD : Registrar estado cancelado e historial
 BD --> Gestor : Confirmación
 Gestor -> Aulas : Liberar reserva vinculada
 alt Liberación confirmada
  Aulas --> Gestor : Reserva liberada
  Gestor --> UI : Cancelación completada
 else Error al liberar aula
  Aulas --> Gestor : Fallo
  Gestor --> UI : Cancelación registrada, liberación pendiente de conciliación
 end
else Cancelado o no autorizado
 Gestor --> UI : Rechazar solicitud y explicar
end
@enduml
```

**Imagen visible:**

![Secuencia UC5 - Cancelar programación de examen](resources/imgs/m5_uc5_secuencia.png)

---

### UC6. Publicar calendario de exámenes

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC6. El administrador solicita publicar programaciones revisadas. El gestor verifica que los registros sean válidos y publicables, cambia su estado a publicado y habilita su consulta para alumnos y profesores autorizados. No se presupone un servicio de notificaciones. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc6_secuencia.puml](../docs/architecture/diagrams/m5_uc6_secuencia.puml)

```plantuml
@startuml m5_uc6_secuencia
title UC6 - Publicar calendario de exámenes - Secuencia
autonumber
actor "Administrador / Coordinador" as Admin
boundary "Interfaz de administración" as UI
control "Gestor de publicación" as Gestor
database "Repositorio Calendario" as BD
control "Validador de publicación" as Validador
Admin -> UI : Seleccionar programaciones para publicar
UI -> Gestor : Solicitar publicación
Gestor -> BD : Consultar registros seleccionados
BD --> Gestor : Exámenes y estados
Gestor -> Validador : Comprobar programados, completos y con reserva
Validador --> Gestor : Validación
alt Todos publicables
 Gestor -> BD : Cambiar estado a publicado
 BD --> Gestor : Confirmación
 Gestor --> UI : Calendario publicado
else Existen registros inválidos
 Gestor --> UI : Detallar impedimentos
end
@enduml
```

**Imagen visible:**

![Secuencia UC6 - Publicar calendario de exámenes](resources/imgs/m5_uc6_secuencia.png)

---

### UC7. Consultar calendario personal de exámenes

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC7. El alumno autenticado solicita su calendario. El servicio de consultas obtiene su carga inscrita desde Inscripciones, filtra los exámenes publicados asociados a esos grupos y muestra fecha, horario, asignatura y aula. No permite consultar el calendario privado de otros alumnos. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc7_secuencia.puml](../docs/architecture/diagrams/m5_uc7_secuencia.puml)

```plantuml
@startuml m5_uc7_secuencia
title UC7 - Consultar calendario personal de exámenes - Secuencia
autonumber
actor "Alumno" as Alumno
boundary "Interfaz de consulta" as UI
control "Servicio de consulta" as Consulta
participant "Inscripciones" as Inscripciones
database "Repositorio Calendario" as BD
Alumno -> UI : Abrir mi calendario
UI -> Consulta : Consultar calendario del usuario autenticado
Consulta -> Inscripciones : Consultar grupos inscritos del alumno
Inscripciones --> Consulta : Grupos y asignaturas
Consulta -> BD : Buscar exámenes publicados para esos grupos
BD --> Consulta : Exámenes publicados
alt Hay exámenes
 Consulta --> UI : Mostrar materia, fecha, hora y aula
else Sin exámenes publicados
 Consulta --> UI : Mostrar calendario vacío
end
@enduml
```

**Imagen visible:**

![Secuencia UC7 - Consultar calendario personal de exámenes](resources/imgs/m5_uc7_secuencia.png)

---

### UC8. Consultar calendario de aplicación de exámenes

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC8. El profesor autenticado solicita los exámenes en los que figura como responsable. El servicio consulta sus asignaciones y recupera las programaciones publicadas, mostrando materia, grupo, fecha, horario y aula. Solo presenta sus propias asignaciones. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc8_secuencia.puml](../docs/architecture/diagrams/m5_uc8_secuencia.puml)

```plantuml
@startuml m5_uc8_secuencia
title UC8 - Consultar calendario de aplicación de exámenes - Secuencia
autonumber
actor "Profesor / Aplicador" as Profesor
boundary "Interfaz de consulta" as UI
control "Servicio de consulta" as Consulta
database "Repositorio Calendario" as BD
Profesor -> UI : Abrir calendario de aplicación
UI -> Consulta : Consultar agenda del profesor autenticado
Consulta -> BD : Buscar exámenes publicados con profesor responsable
BD --> Consulta : Programaciones asignadas
alt Hay exámenes
 Consulta --> UI : Mostrar grupos, asignaturas, fechas y aulas
else Sin exámenes
 Consulta --> UI : Mostrar agenda vacía
end
@enduml
```

**Imagen visible:**

![Secuencia UC8 - Consultar calendario de aplicación de exámenes](resources/imgs/m5_uc8_secuencia.png)

---

### UC9. Consultar lista de asistencia

**Descripción:** El diagrama de secuencia ordena las solicitudes, respuestas y decisiones del caso UC9. El profesor elige uno de sus exámenes asignados. El sistema verifica esa asignación, obtiene los alumnos inscritos en el grupo y consulta los criterios de elegibilidad aplicables desde Catálogo de Exámenes o el módulo responsable. Muestra una lista para consulta, sin capturar asistencia ni calificaciones. Las ramas alternativas muestran cuándo se rechaza o no se completa una operación.

**Código fuente UML:** [Ver archivo m5_uc9_secuencia.puml](../docs/architecture/diagrams/m5_uc9_secuencia.puml)

```plantuml
@startuml m5_uc9_secuencia
title UC9 - Consultar lista de asistencia - Secuencia
autonumber
actor "Profesor / Aplicador" as Profesor
boundary "Interfaz de consulta" as UI
control "Servicio de listas" as Listas
database "Repositorio Calendario" as BD
participant "Inscripciones" as Inscripciones
participant "Catálogo de Exámenes" as Catalogo
Profesor -> UI : Seleccionar examen asignado
UI -> Listas : Solicitar lista del examen
Listas -> BD : Verificar examen y aplicador responsable
BD --> Listas : Datos de programación
alt Profesor autorizado
 Listas -> Inscripciones : Obtener alumnos inscritos en el grupo
 Inscripciones --> Listas : Lista de alumnos
 Listas -> Catalogo : Consultar requisitos vigentes del tipo de examen
 Catalogo --> Listas : Criterios aplicables
 Listas -> Listas : Determinar habilitados con datos disponibles
 alt Datos suficientes
  Listas --> UI : Mostrar lista y estado de elegibilidad
 else Datos incompletos
  Listas --> UI : Mostrar lista con elegibilidad por verificar
 end
else Examen no asignado al profesor
 Listas --> UI : Denegar consulta
end
@enduml
```

**Imagen visible:**

![Secuencia UC9 - Consultar lista de asistencia](resources/imgs/m5_uc9_secuencia.png)

---

## Diagrama de estados

### Ciclo de vida de ExamenProgramado

**Descripción:** Se representan los estados Programado, Publicado, Realizado y Cancelado. Las modificaciones conservan un estado vigente y solo se permiten con autorización; la liberación de espacios pertenece al Módulo de Aulas y Grupos.

**Código fuente UML:** [Ver archivo m5_estados_examen.puml](../docs/architecture/diagrams/m5_estados_examen.puml)

```plantuml
@startuml m5_estados_examen
title Estados - Examen programado
[*] --> Programado : Crear examen y reservar aula
Programado --> Publicado : Publicar calendario
Programado --> Programado : Modificar y validar
Publicado --> Publicado : Modificar, validar y republicar [autorizado]

Programado --> Cancelado : Cancelar
Publicado --> Cancelado : Cancelar [autorizado]
Publicado --> Realizado : Fecha concluida y examen aplicado
Realizado --> [*]
Cancelado --> [*]
note right of Programado
Se conserva el historial de modificaciones.
La reserva es gestionada por Aulas y Grupos.
end note
@enduml
```

**Imagen visible:**

![Ciclo de vida de ExamenProgramado](resources/imgs/m5_estados_examen.png)

### Ciclo de vida de VentanaEvaluacion

**Descripción:** Una ventana de evaluación se configura con fechas coherentes, pasa a habilitada al comenzar su intervalo y finaliza al terminar. Su ciclo de vida es diferente al de un periodo escolar, administrado por el equipo 2.

**Código fuente UML:** [Ver archivo m5_estados_ventana.puml](../docs/architecture/diagrams/m5_estados_ventana.puml)

```plantuml
@startuml m5_estados_ventana
title Estados - Ventana de evaluación
[*] --> Configurada : Registrar fechas validas
Configurada --> Habilitada : Inicia intervalo
Configurada --> Configurada : Ajustar fechas validas
Habilitada --> Finalizada : Termina intervalo
Finalizada --> [*]
@enduml
```

**Imagen visible:**

![Ciclo de vida de VentanaEvaluacion](resources/imgs/m5_estados_ventana.png)

## Diagrama de clases

### Estructura del módulo de Calendario de Exámenes

**Descripción:** El modelo identifica `ExamenProgramado` y `VentanaEvaluacion` como entidades propias del módulo, con servicios para programar, validar y consultar. Las asignaturas, grupos, docentes, reservas y tipos de examen aparecen como referencias externas, pues su información es administrada por otros equipos. Atributos, operaciones y estados son propuestas de diseño documental, no una descripción de clases ya implementadas.

**Código fuente UML:** [Ver archivo m5_clases_calendario.puml](../docs/architecture/diagrams/m5_clases_calendario.puml)

```plantuml
@startuml m5_clases_calendario
title Diagrama de clases - Calendario de Exámenes
skinparam classAttributeIconSize 0
class VentanaEvaluacion {
  +idVentana: UUID
  +fechaInicio: Date
  +fechaFin: Date
  +estado: EstadoVentana
  +validarFechas(): boolean
}
class ExamenProgramado {
  +idExamen: UUID
  +idAsignatura: UUID
  +idGrupo: UUID
  +idProfesor: UUID
  +idTipoExamen: UUID
  +fecha: Date
  +horaInicio: Time
  +horaFin: Time
  +idReservaAula: UUID
  +estado: EstadoExamen
  +programar(): void
  +modificar(): void
  +cancelar(): void
  +publicar(): void
}
class GestorProgramacion {
  +programarExamen(datos): Resultado
  +modificarExamen(id, datos): Resultado
  +cancelarExamen(id): Resultado
}
class ValidadorConflictos {
  +verificarAlumnos(grupo, intervalo): boolean
  +verificarProfesor(profesor, intervalo): boolean
  +verificarAula(aula, intervalo): boolean
}
class ServicioConsulta {
  +calendarioAlumno(id): Lista
  +calendarioProfesor(id): Lista
  +listaExamen(id): Lista
}
class ReferenciaAsignatura <<externo>> {
  +idAsignatura: UUID
}
class ReferenciaGrupo <<externo>> {
  +idGrupo: UUID
}
class ReferenciaProfesor <<externo>> {
  +idProfesor: UUID
}
class ReferenciaReservaAula <<externo>> {
  +idReserva: UUID
}
class ReferenciaTipoExamen <<externo>> {
  +idTipo: UUID
}
enum EstadoExamen {
  Programado
  Publicado
  Realizado
  Cancelado
}
enum EstadoVentana {
  Configurada
  Habilitada
  Finalizada
}
VentanaEvaluacion "1" o-- "0..*" ExamenProgramado : delimita
GestorProgramacion ..> ExamenProgramado : administra
GestorProgramacion ..> ValidadorConflictos : utiliza
ServicioConsulta ..> ExamenProgramado : consulta
ExamenProgramado --> EstadoExamen
VentanaEvaluacion --> EstadoVentana
ExamenProgramado --> ReferenciaAsignatura : id
ExamenProgramado --> ReferenciaGrupo : id
ExamenProgramado --> ReferenciaProfesor : id
ExamenProgramado --> ReferenciaReservaAula : id
ExamenProgramado --> ReferenciaTipoExamen : id
note bottom of ReferenciaGrupo
Referencias a información administrada por otros módulos.
No se duplican sus entidades completas.
end note
@enduml
```

**Imagen visible:**

![Diagrama de clases del módulo](resources/imgs/m5_clases_calendario.png)

---

## Consideraciones de integración

- **UC3** es el caso incluido desde UC2 y UC4; no existe un actor humano exclusivo para dispararlo.
- La reserva y liberación de espacios pertenece al módulo de Aulas y Grupos.
- La definición de periodos escolares pertenece a Periodos Escolares; UC1 administra solo ventanas de evaluación.
- Los tipos y criterios de examen pertenecen al Catálogo de Exámenes.
- Alumnos inscritos y carga académica se obtienen desde Inscripciones.
- La publicación y las consultas se restringen a usuarios autorizados.
- Los cambios de reserva y registro requieren una estrategia de consistencia y recuperación ante errores, por tratarse de dos módulos separados.



## Glosario y Abreviaturas
| Abreviatura / Término | Definición |
| :--- | :--- |
| **EX-PAR** | Examen Parcial: Evaluación periódica durante el semestre. |
| **EX-ORD** | Examen Ordinario: Evaluación final del curso regular. |
| **EX-EXT** | Examen Extraordinario: Evaluación de regularización para alumnos no acreditados. |
| **EX-TS** | Examen a Título de Suficiencia: Evaluación integral extraordinaria. |

## Requerimientos No funcionales
1. **RNF-01 Concurrencia:** Soportar la alta demanda de consultas por parte de los estudiantes al momento de la publicación del calendario.
