# UC6. Actualizar datos personales

| Elemento | Definición del modelo propuesto |
|---|---|
| Actor | Profesor. |
| Requisitos de origen | RF-07, RF-02, RF-08 y RNF-02 del módulo oficial. |
| Antes de comenzar | Sesión identificada. El vínculo cuenta-perfil y los campos editables deben acordarse con E1 y E3. |
| Resultado | Se actualizan únicamente los datos propios autorizados; el rechazo no confirma cambios. |
| Alternativas representadas | Acceso no autorizado; cuenta sin vínculo; registro vinculado inexistente; fallo de lectura del perfil o de la consulta de duplicados; datos inválidos; campos no autorizados; duplicado; fallo de guardado. |

## Secuencia

El actor envía los cambios desde la vista del módulo. El control comprueba identidad y permiso, resuelve el vínculo docente desde la sesión y lee el registro antes de validarlo. Si hay vínculo ausente, registro inexistente, fallo al leer el perfil, fallo al consultar duplicados, datos inválidos, campos no autorizados, duplicados o error al guardar, la secuencia no presenta la actualización como confirmada. Las líneas de vida representan responsabilidades lógicas propuestas, no tecnologías o servicios ya implementados.

[Fuente UML](../resources/uml/m3_uc6_secuencia.uml)

![UC6: secuencia para actualizar datos personales](../resources/imgs/m3_uc6_secuencia.png)

## Actividades

El flujo separa la solicitud del actor y las acciones del módulo. Cada rombo formula una condición con salidas identificadas; un rechazo termina el intento y permite que el actor inicie otro con los datos corregidos. Una operación fallida no se muestra como confirmada.

[Fuente UML](../resources/uml/m3_uc6_actividades.uml)

![UC6: actividades para actualizar datos personales](../resources/imgs/m3_uc6_actividades.png)

## Acuerdos que afectan este caso

Los campos de autoedición y sus validaciones deben acordarse entre E1 y E3. La regla para detectar duplicados debe ajustarse a RF-02 y excluir el registro docente actual. El diagrama distingue una cuenta sin vínculo del caso inconsistente en que existe vínculo, pero no el registro. Esta propuesta no fija formatos, identificadores, longitudes, tiempos de respuesta ni tecnología de almacenamiento.

En UC5/UC6 el identificador del perfil procede de la identidad de la sesión. No se acepta un identificador libre para consultar o modificar a otro profesor. La relación exacta entre cuenta y perfil se valida con E1.

[Consultar el módulo oficial de referencia](https://github.com/wichis/26b_is1_proyecto-final/blob/553104011ece92aeeeecfe54298173bfb88e102f/modulos/m3_profesores.md).
