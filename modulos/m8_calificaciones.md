# Módulo Registro de Calificaciones

> Equipo 6  
> **Integrantes:** Luis Fernando Guerrero Pedroza; Tendzin Angello Diaz Estrada; Erick Daniel Rosales González; Gabriell Quetzalli Santana Reyes

## Resumen
Permite a los profesores capturar calificaciones parciales y finales, generar y cerrar actas; a Control Escolar oficializarlas; y a alumnos consultar resultados y solicitar revisiones. Incluye correcciones autorizadas y reportes académicos.

## Objetivo y Alcance
**Objetivo:** Registrar y consultar calificaciones por alumno, asignatura y grupo con integridad, trazabilidad y aplicación uniforme de las reglas de evaluación.

**Alcance:** Captura de resultados parciales, ordinarios, extraordinarios y a título de suficiencia; validación de elegibilidad y periodos; actas; revisiones, rectificaciones y reportes. La programación de exámenes corresponde al módulo Calendario de Exámenes.

## Requerimientos funcionales
- **RF-01:** Capturar calificaciones parciales y finales por grupo.
- **RF-02:** Distinguir calificaciones ordinarias, extraordinarias y a título de suficiencia.
- **RF-03:** Validar el periodo de captura vigente.
- **RF-04:** Verificar requisitos y oportunidades del examen definidos en el catálogo.
- **RF-05:** Registrar NP (no presentó), cuando corresponda.
- **RF-06:** Generar, cerrar y firmar el acta de calificaciones.
- **RF-07:** Oficializar actas y actualizar el historial académico.
- **RF-08:** Solicitar y autorizar rectificaciones documentadas.
- **RF-09:** Consultar resultados y solicitar/resolver revisiones.
- **RF-10:** Emitir reportes de aprovechamiento por grupo o asignatura.

## Actores
| Actor | Función |
|---|---|
| Profesor | Captura resultados, cierra actas y solicita rectificaciones. |
| Personal de Control Escolar | Valida y oficializa actas; registra resultados oficiales. |
| Coordinador Académico | Autoriza rectificaciones, resuelve revisiones y consulta reportes. |
| Alumno | Consulta sus calificaciones y solicita revisiones. |

## Diagrama de casos de uso

```plantuml
@startuml m8_calificaciones_casos_uso
left to right direction
skinparam shadowing false
skinparam packageStyle rectangle
skinparam defaultFontSize 13
actor "Profesor" as PR
actor "Personal de\nControl Escolar" as CE
actor "Coordinador\nAcadémico" as CO
actor "Alumno" as AL
rectangle "Módulo de Registro de Calificaciones" {
  usecase "Capturar calificaciones\nparciales" as UC01
  usecase "Capturar calificación\nfinal" as UC02
  usecase "Capturar calificación\nordinaria" as UC02a
  usecase "Capturar calificación\nextraordinaria" as UC02b
  usecase "Capturar calificación a\ntítulo de suficiencia" as UC02c
  usecase "Validar periodo\nde captura" as UC03
  usecase "Validar derecho a examen\n(asistencia y oportunidades)" as UC04
  usecase "Registrar NP\n(no presentó)" as UC05
  usecase "Cerrar y firmar acta\nde calificaciones" as UC06
  usecase "Generar acta de\ncalificaciones" as UC07
  usecase "Solicitar rectificación\nde calificación" as UC08
  usecase "Validar y oficializar\nacta" as UC09
  usecase "Actualizar historial\nacadémico" as UC10
  usecase "Autorizar rectificación" as UC11
  usecase "Resolver revisión\nde calificación" as UC12
  usecase "Generar reporte de\naprovechamiento" as UC13
  usecase "Consultar mis\ncalificaciones" as UC14
  usecase "Solicitar revisión\nde calificación" as UC15
}
PR -- UC01
PR -- UC02
PR -- UC06
PR -- UC08
UC02 <|-- UC02a
UC02 <|-- UC02b
UC02 <|-- UC02c
UC01 ..> UC03 : <<include>>
UC02 ..> UC03 : <<include>>
UC02 ..> UC04 : <<include>>
UC02 <.. UC05 : <<extend>>
UC06 ..> UC07 : <<include>>
UC09 ..> UC10 : <<include>>
CE -- UC09
CO -- UC11
CO -- UC12
CO -- UC13
AL -- UC14
AL -- UC15
@enduml
```

![Diagrama de casos de uso de Registro de Calificaciones](./resources/imgs/m8_calificaciones_casos_uso.png)

## Glosario y Abreviaturas
- **NP:** No presentó.
- **Acta de calificaciones:** Registro formal de los resultados de un grupo.
- **Revisión:** Solicitud del alumno para verificar un resultado publicado.
- **Rectificación:** Corrección autorizada de una calificación.
- **Ordinario / extraordinario / título de suficiencia:** Modalidades de evaluación definidas por el catálogo.

## Requerimientos No funcionales
- **RNF-01:** Proteger las calificaciones mediante autenticación y autorización por rol.
- **RNF-02:** Registrar en bitácora las modificaciones, motivos, responsables y fechas.
- **RNF-03:** Impedir la edición directa de actas oficializadas; usar rectificación autorizada.
- **RNF-04:** Mantener consistencia entre calificaciones, inscripciones, asignaturas y actas.
- **RNF-05:** Mostrar los resultados en una interfaz legible y accesible.

## Reglas de negocio e integración
Las calificaciones deben vincularse a alumnos inscritos y profesores asignados. Los requisitos, porcentajes y oportunidades se consultan en el **Catálogo de Exámenes**, evitando duplicación. El **Calendario de Exámenes** determina las evaluaciones programadas. La escala numérica, mínimos aprobatorios, asistencias y plazos de revisión deben corresponder al reglamento académico aplicable.
