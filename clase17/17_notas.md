Resumen
El contexto es clave al hablar de agentes con inteligencia artificial, especialmente cuando diferenciamos un simple bot conversacional de un agente verdaderamente inteligente. La capacidad para mantener el contexto es fundamental, pues permite relacionar preguntas previas con conversaciones actuales, mejorando significativamente la precisión y relevancia de las respuestas.

¿Por qué el contexto mejora un agente de IA?

El contexto permite que un agente relacione información actual con interacciones anteriores, brindando respuestas más naturales y coherentes. Por ejemplo, si primero hablas de Superman y luego introduces a Batman en la conversación, un agente que maneja contexto adecuadamente podría señalar la relación amistosa entre ambos personajes, añadiendo valor y naturalidad a la interacción.

¿Cómo implementar contexto dinámico con MCP y Python?

Implementar una gestión sencilla del contexto en tu agente inteligente utilizando MCP (Modelo de Contexto Persistente) en Python implica tres sencillos pasos prácticos:

Creando servidor y contexto raíz

1. Primero debes crear una carpeta denominada clase16 sin espacios y dentro de esta un archivo del servidor MCP con extensión .py.
2. Importa la librería MCP, inicializando después el servidor junto al contexto raíz (root context). Este último comienza acumulando información desde el principio.
```
import mcp.server.fastmcpy

# Inicialización del servidor
root_context = {"bienvenida": "Bienvenido"}
```
Creando herramienta para actualizar el contexto

Define una herramienta específica, denominada `UpdateContext`, que actualiza dinámicamente tu contexto raíz cada vez que se recibe información nueva del usuario.
Los datos proporcionados se almacenan automáticamente en el diccionario.
```
def UpdateContext(user_data):
    root_context.update(user_data)
```
Ejecutando y visualizando el contexto actualizado

1. Ejecuta tu servidor MCP y observa cómo el contexto se actualiza con cada entrada del usuario.
2. Al utilizar la herramienta desde el explorador, verás cómo el contenido se va acumulando secuencialmente, proporcionando a tu modelo de lenguaje avanzado información más robusta y detallada para sus respuestas.
Este método es sencillo para implementar y provee una interacción más natural y precisa entre usuarios y agentes de inteligencia artificial avanzados, permitiendo contextos más completos y respuestas acordes con las expectativas del usuario.

¿Tienes experiencia manejando contextos en tus aplicaciones de inteligencia artificial? ¡Comparte tus comentarios abajo!

---
<!-- Nery Alberto Cano Ortigoza -->
- ¿Es posible inyectar este historial en LLMs?

Absolutamente, y de hecho, ese es el objetivo final de gestionar el contexto. Una vez que has acumulado la información en tu servidor, puedes empaquetar ese diccionario y enviarlo como parte del prompt del sistema o del historial de mensajes hacia el LLM. Al integrar esto, el modelo de lenguaje no solo recibe la pregunta actual, sino todo el bagaje de la sesión. Esto se logra fácilmente conectando los métodos de obtención de contexto justo antes de hacer la petición a la API del modelo que utilices. Es esta inyección de datos estructurados lo que permite que el LLM actúe con una inteligencia contextual superior, recordando detalles específicos sin que el usuario tenga que repetirlos constantemente.

- ¿Cómo estructurar el contexto dinámico en Python?

Para gestionar la memoria de tu agente, la estructura ideal es utilizar diccionarios de datos (dict en Python). Esta estructura clave-valor te permite almacenar información estructurada, como un user_id asociado a sus mensajes o preferencias. En lugar de tener una simple cadena de texto larga, un diccionario facilita la búsqueda, actualización y organización de la información. Por ejemplo, puedes tener un contexto raíz que inicie con configuraciones base y se vaya poblando con las interacciones del usuario. Al usar herramientas modernas, puedes crear funciones específicas que reciban nuevos datos y los inyecten directamente en este diccionario global, asegurando que la memoria del agente esté siempre estructurada y lista para ser consultada en cualquier momento de la ejecución.

- ¿Cuándo debería actualizar la información del usuario?

Deberías actualizar el contexto cada vez que el usuario introduzca un dato nuevo, relevante o cambie la intención de la conversación. No se trata de guardar cada saludo genérico, sino de capturar variables clave: preferencias, nombres, selecciones de productos o estados de ánimo. Implementar una herramienta específica para esta tarea te permite interceptar la entrada del usuario, extraer el valor importante y guardarlo en tu diccionario antes de generar la respuesta final. Esta actualización constante asegura que cuando el agente deba tomar una decisión o ejecutar una acción compleja más adelante, tenga a su disposición la radiografía exacta y actualizada de lo que el usuario necesita en ese preciso instante.

---

<!---Andrea Alexandra Mora Vega--->

Idea central (qué es contexto dinámico)

El contexto no es todo lo que ha pasado.

Es solo lo que el sistema decide que es relevante ahora.


En MCP:

El LLM no gestiona el contexto

El MCP Server sí

El contexto se construye, se filtra y se descarta

---

<!--Juan Sebastián Martinez Caldas-->

Por si alguien le es util el código:
```
from mcp.server.fastmcp import FastMCP

mcp = FastMCP("my-mcp-server")
root_context = {"message": "Bienvenido", "user_data": {}}

@mcp.tool()
def update_user_data(user_id: str, user_data: str) -> dict:
    root_context["user_data"][user_id] = user_data
    return root_context

@mcp.tool()
def get_user_data() -> dict:
    return root_context
    
if __name__ == "__main__":
    mcp.run()
```
