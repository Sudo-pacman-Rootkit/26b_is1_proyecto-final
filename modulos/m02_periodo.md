# Módulo Periodo

> **Equipo 2: Módulo de Periodo**

## Resumen

El Módulo Periodo permite administrar los periodos escolares del Sistema de Control Escolar. Facilita el registro de nuevos periodos, su activación y finalización, así como la configuración de las ventanas de captura de notas. También permite consultar el periodo escolar activo y los periodos históricos, de acuerdo con las funciones de cada usuario.

## Objetivo y Alcance

**Objetivo:** Gestionar el ciclo de vida de los periodos escolares para mantener organizada la actividad académica y controlar los tiempos destinados a la captura de calificaciones.

**Alcance:** El módulo contempla el registro de nuevos periodos escolares, su activación, finalización, configuración de ventanas de captura de notas y consulta de periodos activos e históricos. Las operaciones de administración corresponden al Administrador, mientras que el Docente y el Alumno pueden consultar la información de los periodos.

## Requerimientos funcionales

- **RF01. Registrar nuevo periodo:** El sistema debe permitir al Administrador registrar un nuevo periodo escolar.
- **RF02. Activar periodo escolar:** El sistema debe permitir al Administrador activar un periodo previamente registrado.
- **RF03. Consultar periodo activo e histórico:** El sistema debe permitir consultar el periodo escolar activo y los periodos anteriores.
- **RF04. Finalizar periodo:** El sistema debe permitir al Administrador finalizar un periodo escolar.
- **RF05. Gestionar ventanas de captura de notas:** El sistema debe permitir al Administrador configurar y gestionar los intervalos habilitados para la captura de calificaciones.
- **RF06. Restringir operaciones por actor:** El sistema debe permitir las operaciones administrativas únicamente a los usuarios autorizados, mientras que el Docente y el Alumno podrán consultar los periodos según sus permisos.

## Actores

- **Administrador:** Registra nuevos periodos, activa y finaliza periodos escolares, gestiona las ventanas de captura de notas y consulta los periodos registrados.
- **Docente:** Consulta el periodo escolar activo y los periodos históricos disponibles.
- **Alumno:** Consulta el periodo escolar activo y los periodos históricos disponibles.

## Diagrama de casos de uso

El siguiente diagrama representa las funciones y los actores del Módulo Periodo, de acuerdo con el diseño proporcionado.

### Código fuente UML

```plantuml
@startuml
left to right direction
skinparam packageStyle rectangle

actor "Control Escolar" as CE
actor "Profesor" as Profesor
actor "Alumno" as Alumno

rectangle "Módulo PeriodoEscolar" {
    usecase "Registrar Nuevo PeriodoEscolar" as UC1
    usecase "Activar PeriodoEscolar" as UC2
    usecase "Consultar PeriodoEscolar Activo / Histórico" as UC3
    usecase "Finalizar PeriodoEscolar" as UC4
    usecase "Gestionar VentanaCaptura" as UC5
    usecase "Gestionar VentanaInscripcion" as UC6
}

CE --> UC1
CE --> UC2
CE --> UC3
CE --> UC4
CE --> UC5
CE --> UC6

Profesor --> UC3
Alumno --> UC3
@enduml
```

### Imagen del diagrama

![Diagrama de Casos de Uso](resources/imgs/m02_periodo_casos_uso.png)

## Diagrama de Actividades

### UC01: Registrar Nuevo PeriodoEscolar
```plantuml
@startuml
title Diagrama de Actividad - UC01: Registrar Nuevo PeriodoEscolar
skinparam shadowing false

|Control Escolar|
start
:Abrir formulario de registro de PeriodoEscolar;
:Ingresar clave, nombre y fechas de inicio/fin;

|Sistema (Módulo PeriodoEscolar)|
:Validar formato y coherencia de fechas;
if (¿Fechas válidas y sin traslape?) then (Sí)
  :Verificar si la clave del PeriodoEscolar ya existe;
  if (¿Clave disponible?) then (Sí)
    :Guardar PeriodoEscolar con estatus PLANIFICADO;
    |Control Escolar|
    :Visualizar confirmación de registro;
    stop
  else (No)
    |Control Escolar|
    :Mostrar error de clave duplicada;
    stop
  endif
else (No)
  |Control Escolar|
  :Mostrar error en rango de fechas;
  stop
endif
@enduml
```
![Diagrama de actividad](resources/imgs/m02_periodo_actividad_UC01.png)
### UC02: Activar PeriodoEscolar
```plantuml
@startuml
title Diagrama de Actividad - UC02: Activar PeriodoEscolar
skinparam shadowing false

|Control Escolar|
start
:Seleccionar PeriodoEscolar en estatus PLANIFICADO;
:Solicitar activación del PeriodoEscolar;

|Sistema (Módulo PeriodoEscolar)|
:Consultar si existe otro PeriodoEscolar en estatus ACTIVO;
if (¿Ya existe un PeriodoEscolar activo?) then (Sí)
  |Control Escolar|
  :Mostrar advertencia (debe finalizarse el periodo vigente);
  stop
else (No)
  :Verificar configuración mínima del PeriodoEscolar;
  if (¿Configuración completa?) then (Sí)
    :Cambiar estatus del PeriodoEscolar a ACTIVO;
    |Control Escolar|
    :Confirmar que el PeriodoEscolar está vigente;
    stop
  else (No)
    |Control Escolar|
    :Solicitar completar datos o calendarios;
    stop
  endif
endif
@enduml
```
![Diagrama de actividad](resources/imgs/m02_periodo_actividad_UC02.png)
### UC03: Consultar PeriodoEscolar Activo / Histórico
```plantuml
@startuml
title Diagrama de Actividad - UC03: Consultar PeriodoEscolar Activo / Histórico
skinparam shadowing false

|Control Escolar / Profesor / Alumno|
start
:Acceder a la consulta de PeriodoEscolar;
:Seleccionar filtro (activo o histórico);

|Sistema (Módulo PeriodoEscolar)|
:Consultar registros de PeriodoEscolar y ventanas asociadas;
if (¿Existen registros para el filtro seleccionado?) then (Sí)
  |Control Escolar / Profesor / Alumno|
  :Visualizar clave, fechas, estatus y ventanas del PeriodoEscolar;
  stop
else (No)
  |Control Escolar / Profesor / Alumno|
  :Mostrar mensaje de ausencia de registros;
  stop
endif
@enduml
```
![Diagrama de actividad](resources/imgs/m02_periodo_actividad_UC03.png)
### UC04: Finalizar PeriodoEscolar
```plantuml
@startuml
title Diagrama de Actividad - UC04: Finalizar PeriodoEscolar
skinparam shadowing false

|Control Escolar|
start
:Seleccionar PeriodoEscolar ACTIVO;
:Solicitar cierre definitivo del PeriodoEscolar;

|Sistema (Módulo PeriodoEscolar)|
:Verificar estado de VentanaCaptura y VentanaInscripcion;
if (¿Hay ventanas o procesos abiertos?) then (Sí)
  |Control Escolar|
  :Bloquear cierre y mostrar ventanas pendientes;
  stop
else (No)
  |Control Escolar|
  :Confirmar cierre definitivo;
  |Sistema (Módulo PeriodoEscolar)|
  :Cambiar estatus del PeriodoEscolar a FINALIZADO;
  :Archivar en historial académico;
  |Control Escolar|
  :Mostrar confirmación de PeriodoEscolar finalizado;
  stop
endif
@enduml
```
![Diagrama de actividad](resources/imgs/m02_periodo_actividad_UC04.png)
### UC05: Gestionar VentanaCaptura
```plantuml
@startuml
title Diagrama de Actividad - UC05: Gestionar VentanaCaptura
skinparam shadowing false

|Control Escolar|
start
:Seleccionar PeriodoEscolar;
:Abrir gestión de VentanaCaptura;
:Definir o modificar tipo de evaluación y fechas de captura;

|Sistema (Módulo PeriodoEscolar)|
:Consultar fechas de inicio y fin del PeriodoEscolar;
if (¿VentanaCaptura está dentro del PeriodoEscolar?) then (Sí)
  if (¿Hay conflicto con otra VentanaCaptura del mismo tipo?) then (Sí)
    |Control Escolar|
    :Mostrar aviso de traslape de fechas;
    stop
  else (No)
    :Guardar o actualizar VentanaCaptura;
    |Control Escolar|
    :Confirmar calendario de captura de calificaciones;
    stop
  endif
else (No)
  |Control Escolar|
  :Mostrar error de fechas fuera del PeriodoEscolar;
  stop
endif
@enduml
```
![Diagrama de actividad](resources/imgs/m02_periodo_actividad_UC05.png)
### UC06: Gestionar VentanaInscripcion
```plantuml
@startuml
title Diagrama de Actividad - UC06: Gestionar VentanaInscripcion
skinparam shadowing false

|Control Escolar|
start
:Seleccionar PeriodoEscolar;
:Abrir gestión de VentanaInscripcion;
:Definir o modificar modalidad y fechas/horas de inscripción;

|Sistema (Módulo PeriodoEscolar)|
:Consultar fechas de inicio y fin del PeriodoEscolar;
if (¿VentanaInscripcion está dentro del PeriodoEscolar?) then (Sí)
  if (¿Existen traslapes inválidos en VentanaInscripcion?) then (Sí)
    |Control Escolar|
    :Mostrar error de traslape de ventanas;
    stop
  else (No)
    :Guardar o actualizar VentanaInscripcion;
    |Control Escolar|
    :Confirmar ventanas de inscripción disponibles;
    stop
  endif
else (No)
  |Control Escolar|
  :Mostrar error de fechas fuera del PeriodoEscolar;
  stop
endif
@enduml
```
![Diagrama de actividad](resources/imgs/m02_periodo_actividad_UC06.png)

## Diagrama de secuencia

### UC01: Registrar Nuevo PeriodoEscolar
```plantuml
@startuml
title Diagrama de Secuencia - UC01: Registrar Nuevo PeriodoEscolar
skinparam shadowing false

actor "Control Escolar" as CE
boundary "Interfaz Web\n(Front-end)" as UI
control "API REST\n(Módulo Periodo)" as API
entity "PeriodoEscolar" as Entidad
database "Base de Datos" as DB

CE -> UI : 1. Seleccionar "Nuevo PeriodoEscolar"
activate UI
UI --> CE : 2. Mostrar formulario (clave, nombre, fechas inicio/fin)

CE -> UI : 3. Enviar datos del PeriodoEscolar
UI -> API : 4. POST /periodos-escolares (JSON)
activate API

API -> Entidad : 5. validarFechasYClave(datos)
activate Entidad
Entidad --> API : 6. Datos válidos (estatus = PLANIFICADO)
deactivate Entidad

API -> DB : 7. SELECT COUNT(*) WHERE clave = :clave
activate DB
DB --> API : 8. 0 (No duplicado)

alt Clave disponible y fechas válidas
API -> DB : 9. INSERT INTO PeriodoEscolar
DB --> API : 10. Registro confirmado (idPeriodo)
deactivate DB
API --> UI : 11. HTTP 201 Created
UI --> CE : 12. Mostrar confirmación de registro
else Clave duplicada o fechas traslapadas
API --> UI : 11. HTTP 400 Bad Request
UI --> CE : 12. Mostrar mensaje de error
end

deactivate API
deactivate UI
@enduml
```
![Diagrama de secuencia](resources/imgs/m02_periodo_secuencia_UC01.png)
### UC02: Activar PeriodoEscolar
```plantuml
@startuml
title Diagrama de Secuencia - UC02: Activar PeriodoEscolar
skinparam shadowing false

actor "Control Escolar" as CE
boundary "Interfaz Web\n(Front-end)" as UI
control "API REST\n(Módulo Periodo)" as API
entity "PeriodoEscolar" as Entidad
database "Base de Datos" as DB

CE -> UI : 1. Solicitar activación de PeriodoEscolar
activate UI
UI -> API : 2. PATCH /periodos-escolares/{id}/activar
activate API

API -> DB : 3. SELECT * FROM PeriodoEscolar WHERE estatus = 'ACTIVO'
activate DB
DB --> API : 4. Resultado de periodo activo actual

alt No existe otro PeriodoEscolar activo
    API -> Entidad : 5. cambiarEstatus(ACTIVO)
    activate Entidad
    Entidad --> API : 6. Transición válida
    deactivate Entidad

    API -> DB : 7. UPDATE PeriodoEscolar SET estatus = 'ACTIVO'
    DB --> API : 8. Actualización exitosa
    deactivate DB

    API --> UI : 9. HTTP 200 OK
    UI --> CE : 10. Notificar que el PeriodoEscolar está activo
else Ya existe un PeriodoEscolar activo
    API --> UI : 9. HTTP 409 Conflict
    UI --> CE : 10. Advertir que debe finalizarse el periodo vigente primero
end

deactivate API
deactivate UI
@enduml
```
![Diagrama de secuencia](resources/imgs/m02_periodo_secuencia_UC02.png)
### UC03: Consultar PeriodoEscolar Activo / Histórico
```plantuml
@startuml
title Diagrama de Secuencia - UC03: Consultar PeriodoEscolar Activo / Histórico
skinparam shadowing false

actor "Control Escolar / Profesor / Alumno" as Usuario
boundary "Interfaz Web\n(Front-end)" as UI
control "API REST\n(Módulo Periodo)" as API
database "Base de Datos" as DB

Usuario -> UI : 1. Acceder a consulta de PeriodoEscolar
activate UI
UI -> API : 2. GET /periodos-escolares?filtro={activo|historico}
activate API

API -> DB : 3. SELECT * FROM PeriodoEscolar (con ventanas asociadas)
activate DB
DB --> API : 4. Lista de PeriodoEscolar
deactivate DB

alt Existen registros
    API --> UI : 5. HTTP 200 OK (JSON con PeriodoEscolar)
    UI --> Usuario : 6. Mostrar detalles (clave, fechas, estatus y ventanas)
else Sin registros disponibles
    API --> UI : 5. HTTP 404 Not Found
    UI --> Usuario : 6. Mostrar mensaje "No hay periodos registrados"
end

deactivate API
deactivate UI
@enduml
```
![Diagrama de secuencia](resources/imgs/m02_periodo_secuencia_UC03.png)
### UC04: Finalizar PeriodoEscolar
```plantuml
@startuml
title Diagrama de Secuencia - UC04: Finalizar PeriodoEscolar
skinparam shadowing false

actor "Control Escolar" as CE
boundary "Interfaz Web\n(Front-end)" as UI
control "API REST\n(Módulo Periodo)" as API
entity "PeriodoEscolar" as Entidad
database "Base de Datos" as DB

CE -> UI : 1. Seleccionar "Finalizar PeriodoEscolar"
activate UI
UI --> CE : 2. Solicitar confirmación de cierre definitivo

CE -> UI : 3. Confirmar cierre
UI -> API : 4. PATCH /periodos-escolares/{id}/finalizar
activate API

API -> DB : 5. Verificar cierre de VentanaCaptura y VentanaInscripcion
activate DB
DB --> API : 6. Estado de ventanas

alt Ventanas cerradas y condiciones cumplidas
    API -> Entidad : 7. cambiarEstatus(FINALIZADO)
    activate Entidad
    Entidad --> API : 8. Estado actualizado a histórico
    deactivate Entidad

    API -> DB : 9. UPDATE PeriodoEscolar SET estatus = 'FINALIZADO'
    DB --> API : 10. Confirmación de cierre
    deactivate DB

    API --> UI : 11. HTTP 200 OK
    UI --> CE : 12. PeriodoEscolar cerrado y movido a histórico
else Existen ventanas o procesos abiertos
    API --> UI : 11. HTTP 409 Conflict
    UI --> CE : 12. Mostrar error: aún hay ventanas vigentes
end

deactivate API
deactivate UI
@enduml
```
![Diagrama de secuencia](resources/imgs/m02_periodo_secuencia_UC04.png)
### UC05: Gestionar VentanaCaptura
```plantuml
@startuml
title Diagrama de Secuencia - UC05: Gestionar VentanaCaptura
skinparam shadowing false

actor "Control Escolar" as CE
boundary "Interfaz Web\n(Front-end)" as UI
control "API REST\n(Módulo Periodo)" as API
entity "VentanaCaptura" as VC
database "Base de Datos" as DB

CE -> UI : 1. Abrir configuración de VentanaCaptura del PeriodoEscolar
activate UI
UI --> CE : 2. Mostrar formulario (tipo evaluación, fecha apertura/cierre)

CE -> UI : 3. Guardar fechas de VentanaCaptura
UI -> API : 4. POST/PUT /periodos-escolares/{id}/ventanas-captura
activate API

API -> DB : 5. SELECT fechaInicio, fechaFin FROM PeriodoEscolar WHERE id = :id
activate DB
DB --> API : 6. Rango del PeriodoEscolar

API -> VC : 7. validarRangoDentroDePeriodo(inicio, fin)
activate VC
VC --> API : 8. Validación exitosa
deactivate VC

alt Fechas válidas dentro del PeriodoEscolar
    API -> DB : 9. INSERT/UPDATE VentanaCaptura
    DB --> API : 10. Registro guardado
    deactivate DB
    API --> UI : 11. HTTP 200 OK
    UI --> CE : 12. Mostrar VentanaCaptura actualizada
else Fechas fuera del PeriodoEscolar
    API --> UI : 11. HTTP 422 Unprocessable Entity
    UI --> CE : 12. Indicar error en el rango de fechas
end

deactivate API
deactivate UI
@enduml
```
![Diagrama de secuencia](resources/imgs/m02_periodo_secuencia_UC05.png)
### UC06: Gestionar VentanaInscripcion
```plantuml
@startuml
title Diagrama de Secuencia - UC06: Gestionar VentanaInscripcion
skinparam shadowing false

actor "Control Escolar" as CE
boundary "Interfaz Web\n(Front-end)" as UI
control "API REST\n(Módulo Periodo)" as API
entity "VentanaInscripcion" as VI
database "Base de Datos" as DB

CE -> UI : 1. Seleccionar configuración de VentanaInscripcion
activate UI
UI --> CE : 2. Mostrar formulario (tipo inscripción, fecha/hora inicio y fin)

CE -> UI : 3. Registrar fechas de VentanaInscripcion
UI -> API : 4. POST/PUT /periodos-escolares/{id}/ventanas-inscripcion
activate API

API -> DB : 5. SELECT fechaInicio, fechaFin FROM PeriodoEscolar WHERE id = :id
activate DB
DB --> API : 6. Rango del PeriodoEscolar

API -> VI : 7. validarRangoYNoTraslape(inicio, fin)
activate VI
VI --> API : 8. Validación exitosa
deactivate VI

alt Fechas válidas
    API -> DB : 9. INSERT/UPDATE VentanaInscripcion
    DB --> API : 10. VentanaInscripcion guardada
    deactivate DB
    API --> UI : 11. HTTP 200 OK
    UI --> CE : 12. Confirmar calendario de inscripciones
else Fechas inválidas o traslapadas
    API --> UI : 11. HTTP 422 Unprocessable Entity
    UI --> CE : 12. Mostrar error de validación
end

deactivate API
deactivate UI
@enduml
```
![Diagrama de secuencia](resources/imgs/m02_periodo_secuencia_UC06.png)


## Diagrama de estados

### Diagrama 1: Estados de ventana de Captura de calificaiones
```plantuml
@startuml
!pragma layout smetana
title Estados de la Ventana de Captura de Calificaciones

skinparam shadowing false
skinparam defaultFontName Arial
skinparam defaultFontSize 13
skinparam roundCorner 12
skinparam ArrowColor #444444
skinparam stateBackgroundColor #EAF2FB
skinparam stateBorderColor #2F5D8A
skinparam noteBackgroundColor #FFF7E6
skinparam noteBorderColor #B7791F

state "Programada" as Programada
state "Habilitada" as Habilitada
state "Cerrada" as Cerrada

[*] --> Programada : el Administrador configura\nla ventana de captura

Programada --> Habilitada : inicia la captura\n[fecha actual >= inicio]

Habilitada --> Cerrada : termina el plazo\n[fecha actual >= fin]

Cerrada --> [*]

note right of Programada
  El intervalo de captura
  ya se encuentra definido.
end note

note right of Habilitada
  Los profesores autorizados
  pueden registrar
  calificaciones.
end note

note right of Cerrada
  La captura ordinaria
  queda restringida.
end note
@enduml
```
![Diagrama de estados](resources/imgs/m02_periodo_estados_1.png)
### Diagrama 2: Estados de ventana de Inscripción
```plantuml
@startuml
!pragma layout smetana
title Estados de la Ventana de Inscripción

skinparam shadowing false
skinparam defaultFontName Arial
skinparam defaultFontSize 13
skinparam roundCorner 12
skinparam ArrowColor #444444
skinparam stateBackgroundColor #EAF2FB
skinparam stateBorderColor #2F5D8A
skinparam noteBackgroundColor #FFF7E6
skinparam noteBorderColor #B7791F

state "Programada" as Programada
state "Abierta" as Abierta
state "Cerrada" as Cerrada

[*] --> Programada : el Administrador configura\nlas fechas de inscripción

Programada --> Abierta : inicia la ventana\n[fecha actual >= inicio]

Abierta --> Cerrada : termina la ventana\n[fecha actual >= fin]

Cerrada --> [*]

note right of Programada
  Las fechas están definidas,
  pero todavía no se permiten
  inscripciones.
end note

note right of Abierta
  El módulo Inscripciones
  puede aceptar solicitudes
  de alumnos autorizados.
end note

note right of Cerrada
  No se permiten nuevas
  inscripciones ordinarias
  dentro de esta ventana.
end note
@enduml
```
![Diagrama de estados](resources/imgs/m02_periodo_estados_2.png)
## Diagrama de clases

{codigo fuente uml, coloca un link que haga referencia al archvio que contiene el codigo fuente}

{imagen visible}
## Glosario y Abreviaturas

| Término | Definición |
|---|---|
| Periodo escolar | Intervalo en el que se desarrollan las actividades académicas. |
| Periodo activo | Periodo escolar habilitado como vigente dentro del sistema. |
| Periodo histórico | Periodo escolar anterior que se conserva para consulta. |
| Ventana de captura de notas | Intervalo durante el cual se permite registrar o capturar calificaciones. |
| UML | Lenguaje Unificado de Modelado (*Unified Modeling Language*). |
| RF | Requerimiento funcional. |
| RNF | Requerimiento no funcional. |

## Requerimientos no funcionales

- **RNF01. Seguridad:** Las funciones administrativas deben estar disponibles únicamente para usuarios con los permisos correspondientes.
- **RNF02. Integridad de la información:** El sistema debe validar los datos de cada periodo antes de guardarlos.
- **RNF03. Consistencia temporal:** Las fechas de inicio y término de un periodo deben ser válidas y coherentes.
- **RNF04. Control de captura:** El sistema debe respetar las ventanas de captura de notas configuradas para el periodo.
- **RNF05. Usabilidad:** La interfaz debe presentar claramente el periodo activo y los periodos históricos disponibles para consulta.
- **RNF06. Disponibilidad de consulta:** La información de los periodos debe poder consultarse cuando el sistema esté disponible y el usuario cuente con los permisos necesarios.

## Referencias

- Diagrama de casos de uso del Módulo Periodo proporcionado para el proyecto.
- Especificaciones y reglas de operación del Sistema de Control Escolar.



