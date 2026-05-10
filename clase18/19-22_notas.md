## Clase 19
**Resumen**
Crear un sistema de búsqueda web eficiente puede ser sencillo con las herramientas adecuadas. En este contenido aprenderás cómo desarrollar un servidor MCP usando SERP API, un servicio para búsqueda web que permite integraciones rápidas y accesibles. Este recurso gratuito y fácil de configurar aporta versatilidad y potencia a tus proyectos tecnológicos.

¿Qué es SERP API y cómo configurar tu cuenta?

SERP API es una herramienta gratuita que permite realizar búsquedas web con facilidad, generando resultados efectivos de forma rápida y accesible. Registrarte es sencillo mediante cuenta de GitHub, Gmail o número de teléfono; el proceso de obtención del API key es inmediato:

- Ingresa al sitio web de SERP API.
- Selecciona método de registro y autentícate.
- Copia la llave API proporcionada para empezar a usar el servicio.
¿Cómo configurar SERP API con tu proyecto?

Integra la API key en tu entorno de desarrollo a través de un archivo .env para garantizar seguridad y accesibilidad en tu configuración:

- Crea un archivo .env y agrega tu llave SERP API entre comillas.
- Usa un archivo env-sample como referencia para compartir configuraciones.
Esta práctica te permite mantener ordenado tu proyecto y salvaguardar tus datos sensibles.

¿Cómo construir un servidor web MCP con Fast MCP y SERP API?

El desarrollo del servidor incluye módulos básicos y avanzados que potenciarán tus aplicaciones:

- Importación de módulos fundamentales como OS, json, HTTPX, Fast MCP Server, y manejo del ambiente.
- Implementación de un método específico para peticiones (make SERP API request).
- Integración de todos los parámetros necesarios para búsquedas generales: consulta, resultados esperados y contexto.

La importancia del registro (logging) radica en asegurar una transparencia operacional que identifique aciertos y fallos, brindando claridad en todo momento sobre el rendimiento del sistema.

Además, es recomendable considerar documentación multilingüe (español e inglés) para ampliar el alcance de tus aplicaciones y mejorar su accesibilidad global.

Al implementar estas recomendaciones simples y estructuradas, tu servidor MCP estará listo para realizar búsquedas web efectivas con una herramienta poderosa como SERP API.

¿Quieres compartir alguna experiencia implementando un MCP con SERP API o tienes dudas sobre el proceso? ¡Déjanos tu comentario abajo!
---
<!-- Andrea Alexandra Mora Vega -->

Idea clave (antes de escribir código)

> El LLM no navega la web. > El MCP Server navega la web por él, con reglas.

- El LLM pregunta
- El MCP Server busca
- La SERP API responde
- El sistema decide qué información entra al contexto
- Los logs son la memoria objetiva del sistema. Sin logs solo tienes suposiciones.

Cuando algo falla, el sistema no te avisa con palabras. Te avisa con comportamiento. Los logs son la forma de leer ese comportamiento.
---
<!-- Oscar Giovanni Bocanegra Hurtado -->

Problemas identificados:

- Falta from __future__ import annotations
- Tipos de retorno incorrectos: Las herramientas MCP deben devolver str, no Dict[str, Any]
- Falta el bloque if __name__ == "__main__": mcp.run()
- Error tipográfico en el mensaje de error
- Funciones incompletas (qna no tiene return en algunos casos)

El error indica que FastMCP no tiene un decorador @mcp.action(). En FastMCP, solo existen @mcp.tool() y @mcp.resource().

Para corregir el servidor web search, cambia @mcp.action() por @mcp.tool():

---
## Clase 20

**Resumen**

Implementar búsquedas eficaces y variadas dentro de un proyecto requiere contar con herramientas adecuadas y flexibles. SERP API ofrece múltiples funcionalidades que te permiten realizar búsquedas generales, obtener noticias, localizar productos específicos y responder preguntas con resultados precisos. Te explicamos claramente cómo usar estos servicios desde tu MCP.

¿Qué tipos de búsqueda puedes implementar con SERP API?

SERP API cuenta con diferentes tipos de resultados que pueden integrarse en tu proyecto:

- Resultados generales de búsqueda con Google.
- Resultados específicos de noticias y artículos recientes.
- Resultados de productos con precios y enlaces directos.
- Preguntas y respuestas específicas y directas (Q&A).

Cada uno tiene características particulares que facilitan su uso según las necesidades.

¿Cómo agregar la función de búsqueda de noticias en tu servidor MCP?

La integración de noticias permite recibir artículos actualizados relacionados con un término específico. El método requiere parámetros definidos como la consulta, número de elementos y contexto.

```
def busqueda_noticias(consulta, cantidad, contexto):
    contexto.guardar(consulta)
    resultados = make_serp_api_request(contexto, parametros)
    return resultados_si_existen(resultados)
```
Este método entrega:

- Fuente del artículo.
- Fecha de publicación.
- Enlace directo.
- Resumen breve.

¿De qué se trata la búsqueda de productos y cómo agregarla?

La búsqueda de productos utiliza SERP API para ofrecer resultados comerciales mediante parámetros similares, pero enfocados a productos específicos.

``` 
def busqueda_productos(consulta, cantidad, contexto):
    contexto.guardar(consulta)
    parametros['tipo_busqueda'] = 'shopping'
    resultados = make_serp_api_request(contexto, parametros)
    return resultados_si_existen(resultados)
```

Este método proporciona:

- Nombre del producto.
- Precio.
- Enlace directo hacia la tienda.

¿Cómo implementar una herramienta de preguntas y respuestas?

El método Q&A permite obtener respuestas directas según una pregunta ingresada. Implica el uso adicional de un grafo de conocimiento para asegurar precisión y coherencia.

``` 
def preguntas_y_respuestas(consulta, contexto):
    contexto.guardar(consulta)
    respuesta = make_serp_api_request(contexto, parametros)
    if respuesta:
        formatear_respuesta(respuesta)
    else:
        mostrar_error()
```

Ofrece flexibilidad para ajustar distintas condiciones, expandiendo o reduciendo el formato y estilo del contenido.

¿Cómo documentar automáticamente las herramientas utilizando un recurso Readme?

Generar documentación automática es posible creando archivos markdown, permitiendo compartir información descriptiva sobre el uso del servidor y sus funcionalidades integradas.


```
## servidor MCP

Este servidor integra búsquedas web utilizando SERP API con modelos LLM. Herramientas disponibles:
- Búsqueda general.
- Noticias.
- Productos.
- Preguntas y respuestas.
```

Esta herramienta ofrece a los usuarios orientación directa sobre cómo aprovechar los recursos disponibles y operarlos correctamente.

Esperamos que esta información sea útil para optimizar e integrar fácilmente SERP API en tus proyectos, facilitando búsquedas potentes y completas. ¿Estás listo para llevar tu servidor MCP al siguiente nivel? ¡Déjanos tus preguntas u opiniones en los comentarios!

---
<!-- Cristian Pisco Intriago -->

La clase18 tiene algunos errores de typo: 1. 
la línea donde se configura el logging le hace falta una coma. 
```
logging.basicConfig(
  level=logging.INFO,
  format="%(asctime)s - %(name)s - %(levelname)s - %(message)s"
  )
```
El argumento params del método make_serpapi_request debe ser Dict no Dic.
Cuando intento conectarme al MCP server desde el inspector me sale que el @mcp.action() no es correcto, lo cambié a @mcp.tool()

---

<!-- Andrea Alexandra Mora Vega -->
Idea clave (qué problema resolvemos)

El LLM no debe navegar internet. Debe pedirle a un servidor controlado que busque por él.

Con MCP:

- El LLM decide cuándo buscar
- El MCP Server ejecuta la búsqueda
- SerpAPI devuelve resultados estructurados
- El sistema filtra lo que entra al contexto

---
<!-- Nery Alberto Cano Ortigoza-->

- ¿Qué pasa si no hay resultados de productos?

Cuando una consulta de productos (Shopping Intent) no devuelve resultados, tu servidor MCP debe manejar esta excepción de forma elegante para evitar que el cliente o el LLM colapsen. En el mundo real, esto es como ir a una tienda buscando un artículo descontinuado; el vendedor no se queda en silencio, te informa que no hay stock.

A nivel técnico, debes implementar bloques try-catch o validaciones condicionales que verifiquen si el arreglo de resultados está vacío. Si es así, el servidor debe retornar un mensaje claro y formateado, por ejemplo: "No se encontraron productos para esta consulta". Esto permite que el agente de IA entienda la situación y pueda tomar decisiones alternativas, como reformular la búsqueda con sinónimos, ampliar el rango de términos o simplemente informar al usuario final que el artículo específico no está disponible en el mercado actual.


- ¿Por qué es mejor documentar usando recursos?

Utilizar recursos internos, como un archivo ReadMe en formato Markdown, transforma tu servidor MCP de una simple caja negra a una herramienta autodescriptiva. Imagina que compras un electrodoméstico complejo; sin un manual, no sabrías usar todos sus botones. El recurso ReadMe actúa exactamente como ese manual integrado.

Al exponer la documentación directamente desde el servidor, cualquier cliente o agente de IA que se conecte puede leer instantáneamente qué herramientas están disponibles (búsqueda web, noticias, productos) y cómo invocarlas correctamente. Esto reduce drásticamente los errores de integración y facilita el mantenimiento. Si en el futuro agregas una nueva capacidad a tu MCP, solo actualizas este recurso centralizado y todos los clientes conectados sabrán inmediatamente cómo aprovechar la nueva función sin necesidad de buscar documentación externa.

- ¿Cuándo debo usar la herramienta de preguntas?

Debes usar la herramienta de preguntas y respuestas (Q&A) cuando necesites extraer datos precisos y directos basados en un grafo de conocimiento, en lugar de una simple lista de enlaces web. Imagina la diferencia entre preguntar "¿A qué temperatura hierve el agua?" y recibir un "100°C" directo, frente a recibir 10 links a artículos de Wikipedia y blogs de ciencia.

Esta herramienta es ideal para asistentes virtuales que requieren respuestas conversacionales rápidas y concretas. Al implementarla, configuras tu MCP para interpretar la intención directa del usuario y devolver un formato estructurado. Es especialmente útil cuando el LLM necesita un dato duro para continuar un razonamiento complejo sin tener que navegar, leer y resumir múltiples páginas web. Ajustar el formato de salida te permite controlar si quieres una respuesta concisa o una explicación más detallada dependiendo del contexto de tu aplicación.

---
<!--Oscar Giovanni Bocanegra Hurtado-->

El error indica que FastMCP no tiene un decorador @mcp.action(). 

En FastMCP, solo existen @mcp.tool() y @mcp.resource().

Para corregir el servidor web search, cambia @mcp.action() por @mcp.tool():

---

## Clase 21

https://github.com/PrefectHQ/fastmcp

--- 

## Clase 22

Resumen
Explorar nuevas tecnologías como MCP (protocolo de contexto) es fundamental en un mundo en constante evolución como el de la inteligencia artificial. Este protocolo ha comenzado a consolidarse como alternativa efectiva a las APIs tradicionales, al permitir la centralización y unificación de gran cantidad de recursos en un solo punto de acceso.

¿Cómo puede MCP sustituir a una API?

Una API centraliza diferentes fuentes y permite la interacción entre ellas, facilitando así la integración. Del mismo modo, un MCP se posiciona como tecnología capaz de centralizar información y recursos múltiples:

- Si utilizas procesos tipo Raj, probablemente recurras a un MCP.
- Si manejas muchos agentes en un entorno empresarial, la opción será un MCP Server.
- Con numerosas fuentes informativas, el MCP también resulta la mejor alternativa.
La ventaja principal del protocolo MCP radica en agrupar diversos elementos, agentes y recursos en un único punto, semejante a las APIs, pero con funcionalidades potenciadas gracias al contexto que maneja.

¿Qué aspectos importantes considerar sobre MCP?

Aunque MCP representa una solución prometedora y poderosa que permite centralizar recursos con eficiencia, todavía es una tecnología en desarrollo, en constante mejora y actualización. Algunos factores clave a considerar durante su implementación incluyen:

- Seguridad.
- Rendimiento del sistema.
- Escalabilidad.
Prestar atención a estos elementos asegura que la adopción de MCP sea exitosa y productiva.

¿Por qué iniciarse ahora en MCP?

Aprender acerca de MCP es una decisión estratégica para mantenerse actualizado ante las nuevas exigencias tecnológicas y empresariales. El dominio de este protocolo da claridad sobre a dónde dirigir esfuerzos técnicos y permite anticiparse a futuras actualizaciones.

Práctica constante y aprendizaje continúo van de la mano con esta sofisticada tecnología. Aprovechar un curso introductorio completo aporta una base sólida para profundizar en MCP y para preparar la integración exitosa de próximas mejoras y novedades tecnológicas.

Si tienes inquietudes o experiencias con MCP, no dudes en comentar y compartir tus perspectivas.

---
<!--Andrea Alexandra Mora Vega-->

MCP no reemplaza a las APIs tradicionales; las abstrae, gobierna y hace seguras para que la IA las use sin romper el sistema.

Mientras una API está pensada para desarrolladores humanos, MCP está pensado para modelos de lenguaje y agentes que necesitan decidir, enrutar y operar con límites claros.

---
<!--Luis Elicané Tiu Paz-->

Excelente Curso! By the way SerperAPI cambio organic_results por organic, 
la mala noticia es que :( Habilitaron platzi por este fin de semana, pero no me deja hacer mi examen T.T

Crear el archivo README.md al nivel de la clase 18

```
@mcp.resource('readme://')
def get_readme() -> str:
    """
    Regresa el contenido del archivo README.md
    """
    try:
        with open("README.md", "r", encoding="utf-8") as file:
            content = file.read()
            return content
    except FileNotFoundError:
        logger.error("El archivo README.md no fue encontrado.")
        return "El archivo README.md no fue encontrado."
    except Exception as e:
        logger.error(f"Error al leer el archivo README.md: {str(e)}")
        return f"Error al leer el archivo README.md: {str(e)}"
```
