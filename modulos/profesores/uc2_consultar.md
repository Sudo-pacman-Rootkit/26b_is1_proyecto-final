# UC2. Consultar profesores

**Actor:** Administrador escolar. **Requerimientos relacionados:** RF-03, RNF-04.

Permite al administrador escolar ver el listado de profesores registrados y, opcionalmente, buscarlos o filtrarlos.

## Diagrama de secuencia

[Fuente UML](../resources/uml/m3_uc2_secuencia.uml)

![Diagrama de secuencia: Consultar profesores](../resources/imgs/m3_uc2_secuencia.png)

El administrador elige «Consultar profesores». La interfaz pide la lista al controlador, este al servicio y el servicio a la base de datos; el resultado se muestra en una tabla. De forma opcional, el administrador ingresa un nombre o filtro: el sistema repite la consulta con ese criterio y muestra los profesores encontrados o el mensaje «No se encontraron profesores».

| Participante | Tipo | Responsabilidad lógica |
|---|---|---|
| Administrador escolar | Actor | Inicia la consulta e ingresa criterios. |
| Interfaz web | Interfaz | Muestra la tabla y los mensajes. |
| ProfesorController | Control | Recibe las solicitudes y las dirige al servicio. |
| ProfesorService | Servicio | Aplica la búsqueda y el filtrado. |
| Base de datos | Registro | Devuelve la información de profesores. |

## Diagrama de actividades

[Fuente UML](../resources/uml/m3_uc2_actividades.uml)

![Diagrama de actividades: Consultar profesores](../resources/imgs/m3_uc2_actividades.png)

El administrador selecciona la consulta y el sistema obtiene y muestra la lista. Si desea buscar o filtrar, ingresa un criterio y el sistema consulta de nuevo: muestra los resultados o «Sin resultados». Al final el administrador revisa la información.

## Condiciones, resultados y alternativas

- **Resultado normal:** se muestra el listado de profesores.
- **Alternativa 1:** búsqueda o filtro con resultados; se muestran los profesores encontrados.
- **Alternativa 2:** búsqueda o filtro sin resultados; se muestra un mensaje de «Sin resultados».