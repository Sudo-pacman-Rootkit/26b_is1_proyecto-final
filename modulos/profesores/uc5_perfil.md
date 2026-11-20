# UC5. Consultar perfil propio

| Elemento | Definición del modelo propuesto |
|---|---|
| Actor | Profesor. |
| Requisitos de origen | RF-06 y RNF-02 del módulo oficial. |
| Antes de comenzar | Sesión identificada. El mecanismo para vincular la cuenta con el perfil docente está pendiente de acuerdo con E1. |
| Resultado | Se muestra únicamente el perfil vinculado a la identidad de la sesión, o se informa por qué no está disponible. |
| Alternativas representadas | Acceso no autorizado; cuenta sin vínculo; vínculo sin registro; fallo de lectura. |

## Secuencia

El actor inicia la operación desde la vista del módulo. El control comprueba identidad y permiso mediante el acceso compartido. Si existe un vínculo con un perfil docente, consulta únicamente ese registro. La secuencia separa tres situaciones: falta de vínculo, vínculo que no encuentra registro y fallo de lectura. Las líneas de vida representan responsabilidades lógicas propuestas, no tecnologías o servicios ya implementados.

[Fuente UML](../resources/uml/m3_uc5_secuencia.uml)

![UC5: secuencia para consultar perfil propio](../resources/imgs/m3_uc5_secuencia.png)

## Actividades

El flujo separa la solicitud del actor y las acciones del módulo. Cada rombo formula una condición con salidas identificadas; un rechazo termina el intento y permite que el actor inicie otro con los datos corregidos. Una operación fallida no se muestra como confirmada.

[Fuente UML](../resources/uml/m3_uc5_actividades.uml)

![UC5: actividades para consultar perfil propio](../resources/imgs/m3_uc5_actividades.png)

## Acuerdos que afectan este caso

El vínculo entre cuenta y perfil debe acordarse con E1. Una cuenta sin vínculo no equivale a un identificador vinculado cuyo registro no existe; este último caso indicaría inconsistencia de datos. Las reglas particulares de perfiles dados de baja requieren validación con E3. Esta propuesta no fija identificadores, longitudes, tiempos de respuesta ni tecnología de almacenamiento.

En UC5/UC6 el identificador del perfil procede de la identidad de la sesión. No se acepta un identificador libre para consultar o modificar a otro profesor. La relación exacta entre cuenta y perfil se valida con E1.

[Consultar el módulo oficial de referencia](https://github.com/wichis/26b_is1_proyecto-final/blob/553104011ece92aeeeecfe54298173bfb88e102f/modulos/m3_profesores.md).
