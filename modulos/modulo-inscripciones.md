# Módulo 2: Inscripciones

## Información del Equipo
- **Equipo:** 2
- **Módulo:** Inscripciones
- **Integrantes:**
    - Pamela Concepción Abasolo Díaz
    - Héctor Jesús García Monroy
    - Kevin Alejandro Nabor Matias
    - Santiago Cuevas Sanchez
  
## 1. Resumen
El Módulo de Inscripciones se encarga de gestionar el proceso de registro y asignación de materias para los alumnos dentro del sistema. Permite validar los requisitos de inscripción, gestionar cupos por grupo y generar los comprobantes o registros correspondientes.

## 2. Objetivo y Alcance
- **Objetivo General:** Automatizar el proceso de inscripción para garantizar un registro ágil y sin duplicidades de alumnos en las asignaturas.
- **Alcance:**
  - Consulta de oferta académica disponible.
  - Selección y validación de asignaturas/horarios.
  - Registro en base de datos y generación de comprobantes.

## 3. Actores
* *Alumno:* Consulta materias disponibles, arma su horario preliminar y solicita su inscripción formal.
* *Administrador / Coordinador:* Abre o cierra los periodos de inscripción, define cupos y valida/ajusta inscripciones cuando sea necesario.
* *Sistema (automatizado):* Valida que no existan traslapes de horario y actualiza los lugares disponibles.

## 4. Requerimientos Funcionales (RF)
* *Consultar oferta académica:* El sistema debe permitir visualizar las materias ofertadas, grupos, docentes asignados y cupos disponibles en tiempo real.
* *Selección de materias:* El sistema debe permitir al alumno agregar o quitar materias a una lista preliminar de inscripción.
* *Validación de choques de horario:* El sistema debe impedir que el alumno inscriba dos materias que compartan el mismo bloque de horario y día.
* *Validación de cupo máximo:* El sistema no debe permitir inscribir a un alumno en un grupo cuyo cupo se encuentre en 0.
* *Confirmar inscripción:* El sistema debe registrar la selección final del alumno, descontar los cupos correspondientes y generar un folio/comprobante de inscripción.
* *Gestión de periodos de inscripción:* El administrador debe poder activar y desactivar las fechas hábiles para que los alumnos puedan inscribirse.
* *Generación de horario/comprobante:* El sistema debe generar una vista resumen imprimible o descargable con el horario final del alumno inscrito.

## 5. Contenido
### 5.1 Diagrama de casos de uso:
```plantuml
@startuml
left to right direction
actor Alumno
actor "Administrador / Coordinador" as Admin

rectangle "Módulo de Inscripciones" {
  usecase "Consultar oferta académica" as CU1
  usecase "Seleccionar asignaturas" as CU2
  usecase "Confirmar inscripción" as CU3
  usecase "Verificar ventana y turno\nde inscripción" as CU4
  usecase "Validar requisitos académicos" as CU5
  usecase "Validar cupo disponible" as CU6
  usecase "Validar traslape de horario" as CU7
  usecase "Descargar comprobante" as CU8
  usecase "Consultar estado de inscripción\no reinscripción" as CU9
  usecase "Consultar reporte de inscritos" as CU10
}

Alumno --> CU1
Alumno --> CU2
Alumno --> CU3
Alumno --> CU9
Admin --> CU10

CU3 ..> CU4 : <<include>>
CU3 ..> CU5 : <<include>>
CU3 ..> CU6 : <<include>>
CU3 ..> CU7 : <<include>>
CU8 ..> CU3 : <<extend>>
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_casos_uso.png)

### 5.2 diagramas de secuencia
5.2.1 consultar oferta académica
```plantuml
@startuml
title CU: Consultar oferta académica
actor Alumno
boundary "Pantalla\nOferta" as UI
control "ControlOferta" as C
entity PeriodoEscolar
entity Grupo
entity Asignatura
entity HorarioClase

Alumno -> UI : abrirOfertaAcademica()
UI -> C : obtenerOferta(matricula)
C -> PeriodoEscolar : obtenerPeriodoActivo()
PeriodoEscolar --> C : periodo
C -> Grupo : buscarPorPeriodo(periodo)
loop por cada grupo
  Grupo -> Asignatura : obtenerDatos()
  Asignatura --> Grupo : clave, nombre, créditos
  Grupo -> HorarioClase : obtenerHorarios()
  HorarioClase --> Grupo : día, horaInicio, horaFin
end
Grupo --> C : lista de grupos con cupo
C --> UI : oferta disponible
UI --> Alumno : muestra oferta académica
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c1.png)
5.2.2 seleccionar asignaturas
```plantuml
@startuml
title CU: Seleccionar materias
actor Alumno
boundary "Pantalla\nSelección" as UI
control "ControlSeleccion" as C
entity Grupo

Alumno -> UI : seleccionarGrupo(idGrupo)
UI -> C : agregarASeleccion(idGrupo)
C -> Grupo : consultarDatos(idGrupo)
Grupo --> C : asignatura, cupo, horarios
alt grupo ya seleccionado
  C --> UI : error "Materia ya seleccionada"
else selección válida
  C -> C : agregar a selección temporal
  C --> UI : selección actualizada
end
UI --> Alumno : muestra materias seleccionadas
Alumno -> UI : quitarGrupo(idGrupo)
UI -> C : quitarDeSeleccion(idGrupo)
C --> UI : selección actualizada
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c2.png)
5.2.3 confirmar inscripción
```plantuml
@startuml
title CU: Confirmar inscripción
actor Alumno
boundary "Pantalla\nInscripción" as UI
control "ControlInscripcion" as C
entity Grupo
entity Inscripcion
entity DetalleInscripcion

Alumno -> UI : confirmarInscripcion()
UI -> C : confirmar(matricula, selección)

group <<include>> Verificar ventana y turno de inscripción
  C -> C : verificarVentana(matricula)
end
alt no autorizado
  C --> UI : error (periodo / ventana / turno)
  UI --> Alumno : muestra error
else autorizado
  group <<include>> Validar requisitos académicos
    C -> C : validarRequisitos(matricula, selección)
  end
  group <<include>> Validar cupo disponible
    C -> Grupo : verificarCupo(idGrupo)
    Grupo --> C : cupoDisponible
  end
  group <<include>> Validar traslape de horario
    C -> C : detectarTraslapes(horarios)
  end

  alt alguna validación falla
    C --> UI : error con detalle del conflicto
    UI --> Alumno : muestra detalle
  else todas correctas
    C -> Inscripcion : crear(matricula, fecha, estado)
    Inscripcion --> C : folio
    loop por cada grupo
      C -> DetalleInscripcion : crear(inscripcion, grupo)
      C -> Grupo : descontarCupo()
    end
    C --> UI : inscripción confirmada (folio)
    UI --> Alumno : muestra confirmación
  end
end
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c3.png)
5.2.4 verificar ventana y turno de inscripción
```plantuml
@startuml
title CU: Verificar ventana y turno de inscripción
actor Alumno
boundary "Pantalla\nInscripción" as UI
control "ControlInscripcion" as C
entity Alumno as AlumnoE
entity PeriodoEscolar
entity VentanaCaptura

Alumno -> UI : confirmarInscripcion()
UI -> C : verificarVentana(matricula)
C -> PeriodoEscolar : obtenerPeriodoActivo()
PeriodoEscolar --> C : periodo (clave, estado)
alt periodo no abierto
  C --> UI : error "Periodo no abierto"
else periodo abierto
  C -> VentanaCaptura : permite(fechaActual)
  VentanaCaptura --> C : true/false
  alt fuera de ventana
    C --> UI : error "Fuera de la ventana de inscripción"
  else dentro de ventana
    C -> AlumnoE : obtenerEstatusAcademico()
    AlumnoE --> C : estatus
    C -> C : validarTurno(matricula, ventana)
    alt sin turno asignado para este momento
      C --> UI : error "Aún no es tu turno de inscripción"
    else turno válido
      C --> UI : autorizado
    end
  end
end
UI --> Alumno : muestra resultado
@enduml
``` 
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c4.png)
5.2.5 validar requisitos académicos
```plantuml
@startuml
title CU: Validar requisitos académicos
actor Alumno
boundary "Pantalla\nInscripción" as UI
control "ControlInscripcion" as C
entity Grupo
entity AsignaturaPlan
entity Asignatura
entity "HistorialAcademico\n<<vista derivada>>" as HA

Alumno -> UI : confirmarInscripcion()
UI -> C : validarRequisitos(matricula, selección)
C -> HA : consultarResultadosOficiales(matricula)
HA --> C : asignaturas aprobadas
loop por cada grupo seleccionado
  C -> Grupo : obtenerAsignatura()
  Grupo --> C : asignatura
  C -> AsignaturaPlan : obtenerPrerrequisitos(asignatura)
  AsignaturaPlan --> C : lista de prerrequisitos
  C -> C : comparar prerrequisitos vs. aprobadas
  opt hay prerrequisitos sin aprobar
    C -> Asignatura : obtenerNombre()
    Asignatura --> C : nombre
    C -> C : registrarIncumplimiento(asignatura, faltantes)
  end
end
alt hay incumplimientos
  C --> UI : error "Requisitos no cumplidos" (detalle)
  UI --> Alumno : muestra asignaturas y prerrequisitos faltantes
else todos cumplidos
  C --> UI : requisitos válidos
end
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c5.png)
5.2.6 validar cupo disponible
```plantuml
@startuml
title CU: Validar cupo disponible
control "ControlInscripcion" as C
entity Grupo
entity DetalleInscripcion

C -> C : validarCupo(selección)
loop por cada grupo seleccionado
  C -> Grupo : obtenerCupo(idGrupo)
  Grupo --> C : cupo
  C -> DetalleInscripcion : contarInscritosVigentes(idGrupo)
  DetalleInscripcion --> C : inscritos
  alt inscritos < cupo
    C -> C : marcarDisponible(idGrupo)
  else inscritos >= cupo
    C -> C : marcarSinCupo(idGrupo)
  end
end
alt algún grupo sin cupo
  C --> C : resultado "No válido" (lista de grupos sin cupo)
else todos con cupo
  C --> C : resultado "Válido"
end
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c6.png)
5.2.7 validar traslape de horario
```plantuml
@startuml
title CU: Validar traslape de horario
control "ControlInscripcion" as C
entity Grupo
entity HorarioClase

C -> C : validarTraslape(selección)
loop por cada grupo seleccionado
  C -> Grupo : obtenerHorarios(idGrupo)
  Grupo -> HorarioClase : consultar()
  HorarioClase --> Grupo : diaSemana, horaInicio, horaFin
  Grupo --> C : horarios del grupo
end
C -> C : ordenarPorDiaYHora(horarios)
loop por cada par de horarios del mismo día
  opt horaInicio < horaFin del otro y horaFin > horaInicio del otro
    C -> C : registrarTraslape(grupoA, grupoB)
  end
end
alt hay traslapes
  C --> C : resultado "No válido" (grupos en conflicto)
else sin traslapes
  C --> C : resultado "Válido"
end
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c7.png)
5.2.8 descargar comprobante
```plantuml
@startuml
title CU: Descargar comprobante
actor Alumno
boundary "Pantalla\nComprobante" as UI
control "ControlComprobante" as C
entity Inscripcion
entity DetalleInscripcion

Alumno -> UI : descargarComprobante()
UI -> C : generarComprobante(matricula, folio)
C -> Inscripcion : obtenerPorFolio(folio)
alt inscripción no existe o no es del alumno
  Inscripcion --> C : null
  C --> UI : error "No hay inscripción confirmada"
  UI --> Alumno : muestra error
else inscripción válida
  Inscripcion --> C : datos de inscripción
  C -> DetalleInscripcion : obtenerDetalle(folio)
  DetalleInscripcion --> C : materias y grupos
  C -> C : generarPDF()
  C --> UI : archivo PDF
  UI --> Alumno : descarga comprobante
end
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c8.png)
5.2.9 consultar estado de inscripción o reinscripción 
```plantuml
@startuml
title CU: Consultar estado de inscripción o reinscripción
actor Alumno
boundary "Pantalla\nEstado de trámite" as UI
control "ControlInscripcion" as C
entity PeriodoEscolar
entity Inscripcion
entity DetalleInscripcion

Alumno -> UI : consultarEstadoInscripcion()
UI -> C : obtenerEstado(matricula)
C -> PeriodoEscolar : obtenerPeriodoActivo()
PeriodoEscolar --> C : periodo
C -> Inscripcion : buscarPorAlumnoYPeriodo(matricula, periodo)
alt no existe inscripción
  Inscripcion --> C : null
  C --> UI : "Sin inscripción registrada"
else existe
  Inscripcion --> C : folio, fecha, estado
  C -> DetalleInscripcion : obtenerDetalle(folio)
  DetalleInscripcion --> C : asignaturas, grupos y estado de cada una
  C --> UI : estado del trámite
end
UI --> Alumno : muestra estado, folio y detalle
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c9.png)
5.2.10 consultar reporte de inscritos
```plantuml
@startuml
title CU: Consultar reporte de inscritos
actor "Administrador / Coordinador" as Admin
boundary "Pantalla\nReporte" as UI
control "ControlReportes" as C
entity Usuario
entity Rol
entity Grupo
entity Inscripcion
entity DetalleInscripcion

Admin -> UI : solicitarReporte(periodo, filtros)
UI -> C : generarReporte(idUsuario, periodo, filtros)
C -> Usuario : obtenerRol()
Usuario -> Rol : tienePermiso("consultar_reporte")
Rol --> Usuario : true/false
Usuario --> C : autorizado
alt sin permiso
  C --> UI : error "Acceso denegado"
else con permiso
  C -> Grupo : buscarPorPeriodo(periodo)
  Grupo --> C : grupos
  loop por cada grupo
    C -> DetalleInscripcion : contarInscritos(grupo)
    DetalleInscripcion -> Inscripcion : validarEstado()
    DetalleInscripcion --> C : inscritos
  end
  C --> UI : reporte consolidado
  UI --> Admin : muestra reporte (opción de exportar)
end
@enduml
``` 
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_secuencia_c10.png)
### 5.3 diagramas de actividades
5.3.1 consultar oferta academica
```plantuml
@startuml
title Actividad: Consultar oferta académica
|Alumno|
start
:Abrir módulo de oferta académica;
|Sistema|
:Obtener PeriodoEscolar activo;
if (¿Existe periodo activo?) then (no)
  :Mostrar mensaje\n"No hay periodo disponible";
  |Alumno|
  stop
else (sí)
endif
|Sistema|
:Buscar Grupos del periodo;
while (¿Hay más grupos?) is (sí)
  :Obtener datos de Asignatura\n(clave, nombre, créditos);
  :Obtener HorarioClase\n(día, hora inicio, hora fin);
  :Calcular cupo disponible;
endwhile (no)
:Mostrar oferta académica;
|Alumno|
:Revisar oferta;
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c1.png)
5.3.2 selecconar materias
```plantuml
@startuml
title Actividad: Seleccionar materias
|Alumno|
start
:Elegir un Grupo de la oferta;
|Sistema|
if (¿Grupo ya está en la selección?) then (sí)
  :Mostrar error\n"Materia ya seleccionada";
else (no)
  if (¿Misma Asignatura ya seleccionada\nen otro grupo?) then (sí)
    :Mostrar error\n"Asignatura duplicada";
  else (no)
    :Agregar grupo a la selección temporal;
  endif
endif
:Mostrar materias seleccionadas;
|Alumno|
if (¿Desea quitar o agregar otra materia?) then (sí)
  :Modificar selección;
  note right: Repite el flujo
else (no)
  :Conservar selección\npara confirmar inscripción;
endif
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c2.png)
5.3.3 confirmar inscripción
```plantuml
@startuml
title Actividad: Confirmar inscripción
|Alumno|
start
:Solicitar confirmar inscripción;
|Sistema|
partition "<<include>> Verificar ventana y turno" {
  :Verificar periodo abierto, ventana y turno;
}
if (¿Autorizado?) then (no)
  :Mostrar error de autorización;
  |Alumno|
  stop
else (sí)
endif
|Sistema|
partition "<<include>> Validar requisitos académicos" {
  :Comprobar prerrequisitos aprobados;
}
partition "<<include>> Validar cupo disponible" {
  :Verificar cupo de cada grupo;
}
partition "<<include>> Validar traslape de horario" {
  :Verificar traslapes entre grupos;
}
if (¿Alguna validación falló?) then (sí)
  :Informar detalle del conflicto;
  |Alumno|
  :Ajustar selección;
  stop
else (no)
  |Sistema|
  :Crear Inscripcion (folio, fecha, estado);
  while (¿Hay más grupos seleccionados?) is (sí)
    :Crear DetalleInscripcion;
    :Descontar cupo del Grupo;
  endwhile (no)
  :Mostrar confirmación con folio;
endif
|Alumno|
:Recibir confirmación;
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c3.png)
5.3.4 verificar ventana y turno de inscripción
```plantuml
@startuml
title Actividad: Verificar ventana y turno de inscripción
|Sistema|
start
:Obtener PeriodoEscolar activo;
if (¿Periodo abierto?) then (no)
  :Retornar "No autorizado"\n(periodo no abierto);
  stop
else (sí)
endif
:Consultar VentanaCaptura.permite(fecha actual);
if (¿Fecha dentro de la ventana?) then (no)
  :Retornar "No autorizado"\n(fuera de ventana);
  stop
else (sí)
endif
:Consultar estatus académico del Alumno;
if (¿Alumno activo y con turno vigente?) then (no)
  :Retornar "No autorizado"\n(sin turno);
  stop
else (sí)
  :Retornar "Autorizado";
endif
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c4.png)
5.3.5 validar requisitos académicos
```plantuml
@startuml
title Actividad: Validar requisitos académicos
|Sistema|
start
:Recibir asignaturas seleccionadas;
:Consultar resultados oficiales del Alumno\n(HistorialAcademico);
while (¿Hay más asignaturas por validar?) is (sí)
  :Obtener prerrequisitos en AsignaturaPlan;
  if (¿Tiene prerrequisitos?) then (sí)
    if (¿Todos aprobados en el historial?) then (no)
      :Registrar incumplimiento\n(asignatura y prerrequisitos faltantes);
    else (sí)
    endif
  else (no)
  endif
endwhile (no)
if (¿Hay incumplimientos?) then (sí)
  :Retornar "No válido"\ncon detalle;
else (no)
  :Retornar "Válido";
endif
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c5.png)
5.3.6 validar cupo disponible
```plantuml
@startuml
title Actividad: Validar cupo disponible
|Sistema|
start
:Recibir grupos seleccionados;
while (¿Hay más grupos por validar?) is (sí)
  :Consultar Grupo.cupo;
  :Contar DetalleInscripcion activos del grupo;
  if (¿Inscritos < cupo?) then (sí)
    :Marcar grupo como disponible;
  else (no)
    :Marcar grupo como SIN CUPO;
  endif
endwhile (no)
if (¿Algún grupo sin cupo?) then (sí)
  :Retornar resultado "No válido"\ncon lista de grupos sin cupo;
else (no)
  :Retornar resultado "Válido";
endif
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c6.png)
5.3.7 validar traslape de horario
```plantuml
@startuml
title Actividad: Validar traslape de horario
|Sistema|
start
:Recibir grupos seleccionados;
:Obtener HorarioClase de cada grupo\n(diaSemana, horaInicio, horaFin);
:Ordenar horarios por día y hora de inicio;
while (¿Hay más pares de horarios a comparar?) is (sí)
  if (¿Mismo día y\nhoraInicio < horaFin del otro\ny horaFin > horaInicio del otro?) then (sí)
    :Registrar traslape entre los dos grupos;
  else (no)
  endif
endwhile (no)
if (¿Se registró algún traslape?) then (sí)
  :Retornar resultado "No válido"\ncon grupos en conflicto;
else (no)
  :Retornar resultado "Válido";
endif
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c7.png)
5.3.8 descargar comprobante
```plantuml
@startuml
title Actividad: Descargar comprobante
|Alumno|
start
:Solicitar descarga de comprobante;
|Sistema|
:Buscar Inscripcion del alumno por folio;
if (¿Existe inscripción confirmada\ny pertenece al alumno?) then (no)
  :Mostrar error\n"No hay inscripción confirmada";
  |Alumno|
  stop
else (sí)
endif
|Sistema|
:Obtener DetalleInscripcion\n(materias y grupos);
:Generar documento PDF\ncon folio, fecha y materias;
:Enviar archivo al alumno;
|Alumno|
:Descargar comprobante;
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c8.png)

5.3.9 consultar estado de inscripción
```plantuml
@startuml
title Actividad: Consultar estado de inscripción o reinscripción
|Alumno|
start
:Solicitar consulta del estado del trámite;
|Sistema|
:Obtener PeriodoEscolar activo;
:Buscar Inscripcion del alumno en el periodo;
if (¿Existe inscripción?) then (no)
  :Mostrar "Sin inscripción registrada";
else (sí)
  :Obtener estado y folio;
  :Obtener DetalleInscripcion\n(asignaturas y grupos);
  :Mostrar estado, folio y detalle;
endif
|Alumno|
:Revisar información;
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c9.png)
5.3.10 consultar reporte de inscritos
```plantuml
@startuml
title Actividad: Consultar reporte de inscritos
|Administrador / Coordinador|
start
:Seleccionar periodo y filtros\n(grupo, asignatura);
|Sistema|
:Verificar Rol y permisos del Usuario;
if (¿Tiene permiso de consulta?) then (no)
  :Mostrar error "Acceso denegado";
  |Administrador / Coordinador|
  stop
else (sí)
endif
|Sistema|
:Buscar Grupos del periodo según filtros;
while (¿Hay más grupos?) is (sí)
  :Contar DetalleInscripcion\ncon estado vigente;
  :Acumular inscritos y cupo restante;
endwhile (no)
:Generar reporte consolidado;
:Mostrar reporte;
|Administrador / Coordinador|
if (¿Desea exportar?) then (sí)
  :Exportar reporte;
else (no)
endif
stop
@enduml
```
![Diagrama de casos de uso](resources/imgs/m2_inscripciones_actividades_c10.png)


## 6. Abreviaciones
- **BD:** Base de Datos.
- **UI:** Interfaz de Usuario (*User Interface*).

## 7. Requerimientos No Funcionales (RNF)
* *Usabilidad:* La interfaz debe ser intuitiva, permitiendo completar el proceso de inscripción en no más de 4 pasos guiados.
* *Rendimiento:* El tiempo de respuesta del sistema para confirmar y guardar la inscripción no debe superar los 2 segundos.
* *Integridad de datos:* El sistema debe garantizar que dos alumnos no puedan ocupar el último cupo disponible al mismo tiempo (control de concurrencia básico).
* *Disponibilidad:* El módulo debe estar disponible de forma continua durante los periodos oficiales de inscripción establecidos por la coordinación.
* *Compatibilidad:* La interfaz debe ser accesible y funcional desde cualquier navegador web moderno (Chrome, Firefox, Edge).
