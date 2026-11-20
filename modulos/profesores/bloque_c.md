# Bloque C. Consultar y actualizar el perfil propio de profesores

**Estado:** aportación de Ariel para revisión del E3; no equivale a un PR enviado ni a reglas aprobadas. **Base oficial:** `develop`, commit `553104011ece92aeeeecfe54298173bfb88e102f`.

## Contexto y casos de uso

El profesor consulta y actualiza exclusivamente el perfil asociado con su sesión. No elige libremente el identificador de otro docente. La vista general conserva seis casos de uso: administración participa en UC1–UC4 y el profesor en UC5–UC6. La autenticación sigue como función compartida y condición de acceso.

| ID | Caso de uso, secuencia y actividades | Actor |
|---|---|---|
| UC5 | [Consultar perfil propio](uc5_perfil.md) | Profesor |
| UC6 | [Actualizar datos personales](uc6_actualizar.md) | Profesor |

Cada caso enlaza su fuente UML y su imagen, e indica condiciones, resultados y alternativas. Las secuencias separan actor, vista, control, acceso compartido y registro como responsabilidades lógicas propuestas; no fijan una tecnología ni prueban software implementado. Distinguen una cuenta sin vínculo de un vínculo que no encuentra registro, que representa una inconsistencia de datos. En UC6 también se representa el fallo al consultar el perfil antes de modificarlo.

## Casos de uso del módulo Profesores

[Fuente UML](../resources/uml/m3_casos_uso.uml)

![Casos de uso del módulo Profesores](../resources/imgs/m3_casos_uso.png)

## Acuerdos pendientes

Resolver D07: campos de autoedición frente a datos administrados por Control Escolar; RF-07 y RNF-01 no están conciliados por campo. Acordar con E1 el vínculo cuenta/perfil, incluidos los casos de vínculo ausente o inconsistente, y con el bloque B los estados de una actualización. No declarar resueltos los permisos por haber dibujado una validación.

## Fuentes

- [Profesores en la base oficial](https://github.com/wichis/26b_is1_proyecto-final/blob/553104011ece92aeeeecfe54298173bfb88e102f/modulos/m3_profesores.md), con los seis casos UC1–UC6 y los RF/RNF de origen.
- Decisión D07 del [plan de trabajo del repositorio personal](https://github.com/rodriguezarielc2/IngenieriaDelSoftware1-2026B-UAEMex/blob/ed8c4dfee6e00004498a733202db00ace86dd14f/Proyecto%20final%20SCE/Plan%20de%20trabajo.md): los campos de autoedición y el vínculo cuenta/perfil siguen pendientes.
- Aviso «Completar diagramas» aportado por el usuario: avances antes de las 9:00 a. m. del 20 de noviembre de 2026, fecha confirmada en este chat.
- Modelos propios expresados en [PlantUML](https://plantuml.com/), con fuente e imagen. La aceptación académica y los acuerdos de E1/E3 siguen sujetos a revisión.
