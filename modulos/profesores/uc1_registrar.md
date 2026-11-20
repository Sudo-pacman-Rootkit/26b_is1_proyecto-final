# UC1. Registrar profesor

**Actor:** Administrador escolar. **Requerimientos relacionados:** RF-01, RF-02, RF-08.

Permite al administrador escolar dar de alta a un profesor, validando que los datos estén completos y que no exista un registro duplicado.

## Diagrama de secuencia

[Fuente UML](../resources/uml/m3_uc1_secuencia.uml)

![Diagrama de secuencia: Registrar profesor](../resources/imgs/m3_uc1_secuencia.png)

El administrador abre el formulario, captura los datos y presiona «Guardar». La interfaz envía la información al controlador, que pide al servicio validarla. Si los datos son inválidos o incompletos, se muestra un error para corregirlos. Si son válidos, el servicio busca en la base de datos si el profesor ya existe (por correo o número de empleado): en ese caso informa el duplicado; si es nuevo, lo guarda y se muestra la confirmación.

| Participante | Tipo | Responsabilidad lógica |
|---|---|---|
| Administrador escolar | Actor | Inicia la operación y captura los datos. |
| Interfaz web | Interfaz | Muestra formularios y mensajes. |
| ProfesorController | Control | Recibe las solicitudes y las dirige al servicio. |
| ProfesorService | Servicio | Valida datos y detecta duplicados. |
| Base de datos | Registro | Guarda y devuelve la información de profesores. |

## Diagrama de actividades

[Fuente UML](../resources/uml/m3_uc1_actividades.uml)

![Diagrama de actividades: Registrar profesor](../resources/imgs/m3_uc1_actividades.png)

El flujo se divide entre el administrador escolar (selecciona la opción, captura y guarda) y el sistema (valida, verifica que no exista, guarda y confirma). Tiene dos decisiones: si los datos son válidos y si el profesor ya existe.

## Diagrama de estados

[Fuente UML](../resources/uml/m3_uc1_estados.uml)

![Diagrama de estados: Registro de profesor](../resources/imgs/m3_uc1_estados.png)

El registro inicia en *Borrador*. Al guardar pasa a *En validación*; si los datos son inválidos o hay duplicado regresa a *Borrador*, y si son válidos queda *Registrado*. Desde *Borrador* también puede pasar a *Cancelado*.

| Estado | Al entrar | Durante |
|---|---|---|
| Borrador | Mostrar formulario vacío | Capturar datos del profesor |
| En validación | Recibir datos capturados | Validar campos y verificar duplicado |
| Registrado | Guardar profesor en la BD | Mostrar confirmación |
| Cancelado | Descartar datos capturados | — |

## Condiciones, resultados y alternativas

- **Resultado normal:** el profesor queda registrado y se muestra la confirmación.
- **Alternativa 1:** datos inválidos o incompletos; se muestran los campos a corregir.
- **Alternativa 2:** profesor duplicado; se informa que ya está registrado.
- **Alternativa 3:** el administrador cancela y se descartan los datos.