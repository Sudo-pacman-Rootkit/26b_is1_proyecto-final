# Bloque A. Registrar y consultar profesores

**Estado:** aportación de Danna para revisión del E3; no equivale a un PR enviado ni a reglas aprobadas. **Base oficial:** `develop`, commit `553104011ece92aeeeecfe54298173bfb88e102f`.

## Contexto y casos de uso

El administrador escolar da de alta a los profesores y consulta el listado de los ya registrados. La vista general conserva seis casos de uso: administración participa en UC1–UC4 y el profesor en UC5–UC6. La autenticación sigue como función compartida y condición de acceso.

| ID | Caso de uso, secuencia y actividades | Actor |
|---|---|---|
| UC1 | [Registrar profesor](uc1_registrar.md) | Administrador escolar |
| UC2 | [Consultar profesores](uc2_consultar.md) | Administrador escolar |

Cada caso enlaza su fuente UML y su imagen, e indica condiciones, resultados y alternativas. Las secuencias separan actor, interfaz, controlador, servicio y base de datos como responsabilidades lógicas propuestas; no fijan una tecnología ni prueban software implementado. UC1 incluye además un diagrama de estados del registro (Borrador, En validación, Registrado y Cancelado). UC2 representa la búsqueda o filtro opcional y el caso sin resultados.

## Acuerdos pendientes

Acordar con el bloque B los criterios de duplicado de UC1 (el diagrama usa correo o número de empleado) y los campos obligatorios del registro. Acordar con E1 los permisos del administrador escolar para registrar y consultar. Definir con el equipo qué criterios de búsqueda o filtro admite UC2. No declarar resueltas las reglas de validación por haberlas dibujado.

## Fuentes

- [Profesores en la base oficial](https://github.com/wichis/26b_is1_proyecto-final/blob/553104011ece92aeeeecfe54298173bfb88e102f/modulos/m3_profesores.md), con los seis casos UC1–UC6 y los RF/RNF de origen (UC1: RF-01, RF-02, RF-08; UC2: RF-03, RNF-04).
- Modelos propios expresados en [PlantUML](https://plantuml.com/), con fuente e imagen. La aceptación académica y los acuerdos del equipo siguen sujetos a revisión.