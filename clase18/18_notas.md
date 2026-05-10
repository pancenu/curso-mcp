Lecturas recomendadas
SerpApi: Google Search API

Resumen
¿Te has preguntado cómo optimizar el uso de múltiples herramientas en un servidor MCP para mejorar tus aplicaciones y procesos? El enrutamiento con MCP Server te permite acceder a diferentes herramientas según las necesidades específicas de tu trabajo, mejorando eficiencia y organización.

¿Qué es el enrutamiento en MCP Server?

El enrutamiento es una funcionalidad del MCP Server que organiza y administra la manera en que accedes a diversas herramientas o recursos desde una misma interfaz. Funcionan de manera similar a una API, donde tú eliges cuál método utilizar según lo que necesites.

¿Cómo utilizar las herramientas del MCP Server?

Las herramientas incluidas en MCP Server se configuran fácilmente a través de aplicaciones Python que usan Fast MCP. Puedes crear distintos endpoints, configurar parámetros personalizados y obtener resultados al interactuar con herramientas diversas.

Entre los ejemplos prácticos se encuentran:

- Herramienta de chequeo de estado de servidor (get status).
- Recuperación de información de usuario (get user info).
- Función de cálculo de operaciones matemáticas (como raíces cuadradas).
```
# Ejemplo de servidor MCP con Fast MCP
from fast_mcp import MCPServer

server = MCPServer(name='ServidorMCP')

@server.tool
def get_status():
    return "El servidor está ejecutándose correctamente."

@server.tool
def get_user_info(user_id):
    return f"Información del usuario {user_id}."

# Herramienta para raíces cuadradas
@server.tool
def calcular_raiz(numero):
    return numero ** 0.5
```

¿Cómo definir múltiples endpoints en MCP Server?

Si trabajas con varios servicios externos como bases de datos, servicios climáticos u otras APIs, puedes utilizarlos todos desde un mismo servidor mediante el enrutamiento por endpoints.

Cada uno de estos endpoints está vinculado a una URL específica:

- weather service: para información climática.
- database service: para consultas de base de datos.
```
# Configuración de endpoints múltiples
tools_endpoints = {
    "weather_tool": "weather.service.example.com",
    "database_tool": "database.service.example.com"
    }
```
Esto permite con facilidad y eficacia decidir a qué servicio se dirige una solicitud específica, optimizando el trabajo con diferentes recursos, aun cuando la ejecución se realice desde una sola herramienta.

¿Cómo comprobar el enrutamiento en MCP Server?

Puedes verificar fácilmente que tu enrutamiento funcione con diferentes pruebas para cada herramienta y endpoint. Por ejemplo, si pruebas una herramienta llamada "Weather Tool":
``` 
tool_name = "Weather Tool" parameters = { "city": "New York" }
# El resultado debería mostrar conexión a weather.service.example.com
``` 
Para una base de datos:
```
tool_name = "Database Tool" parameters = { "ID_user": "Amin Espinoza" }
# El resultado indicaría conexión a database.service.example.com
```
Estos resultados te permiten confirmar que MCP Server dirige correctamente tus solicitudes según sus configuraciones no importa si surge un error puntual por conexión, lo importante es verificar que la dirección aplicada sea la adecuada.

---

<!-- andres reyes -->

Este articulo es muy bueno 
https://thetalkingapp.medium.com/optimizing-api-output-for-use-as-tools-in-model-context-protocol-mcp-07d93a084fbc

relacionado con enrutamiento para tambien endpoits tambien seria bueno hablar que pasaría si un endpoint te retorna texto que es muy denso +1000 palabras

En el blog mencionan que también es muy bueno rapear las respuestas con código y si luego pasarlas por LLM

---

<!-- Gagandeep Dass -->
#server.py
```
from mcp.server.fastmcp import FastMCP

mcp = FastMCP(name="Servidor de enrutamiento con herramientas")

@mcp.tool()
def get_status() -> dict:
    return {"status": "Running", "version": "1.0.0"}

@mcp.tool()
def get_user_info(user_id: str) -> dict:
    users = {
        "1": {"name": "Alice", "role": "Teacher"},
        "2": {"name": "Felipe", "role": "Course Director"},
        "3": {"name": "Jhon", "role": "ML Engineer"},
        "4": {"name": "Eva", "role": "Product Owner"},
    }

    return users.get(user_id, {"error": "User not found"})

@mcp.tool()
def calculate_square(number: int) -> dict:
    return {"number": number, "square": number ** 2}

if __name__ == "__main__":
    mcp.run()
```
---
<!-- Andrea Alexandra Mora Vega -->
El enrutamiento de herramientas (tool routing) en un MCP Server es el mecanismo que decide qué herramienta se ejecuta, cuándo y bajo qué reglas, a partir de una intención del LLM. Aquí se gana control, seguridad y eficiencia.

---

<!--Juan Sebastian Ramirez Leyva-->

No se si solo me pasa a mi , 
pero cuando se desea importar la librería de mcp.server.fastmcp, 
y a continuación instalar el paquete 
este no te lo instale porque necesitas de pyproject.toml 
que viene siendo el archivo de configuración para eso debemos colocar en el promt del ejecutar en consola lo siguiente

```
uv init
uv add mcp
pip install mcp
``` 
---
<!-- Nery Alberto Cano Ortigoza-->

- ¿Es posible conectar múltiples bases de datos?

Absolutamente, el enrutamiento te permite conectar tantas bases de datos o servicios externos como necesites bajo un mismo servidor MCP de manera estructurada. Puedes tener un endpoint apuntando a una base de datos relacional como PostgreSQL para extraer información de usuarios, otro hacia MongoDB para consultar registros no estructurados, y un tercero hacia un motor de búsqueda como Elasticsearch. La clave del éxito está en definir claramente estas rutas en tu arreglo de endpoints. Cuando la herramienta central recibe la petición, simplemente extrae el identificador del servicio deseado y canaliza los parámetros dinámicos (como un user_id o un término de búsqueda específico) hacia la cadena de conexión correspondiente. Esta arquitectura convierte a tu servidor MCP en un orquestador de datos increíblemente poderoso, capaz de unificar múltiples fuentes de información.

- ¿Cómo enrutar dinámicamente hacia diferentes servicios?

Para lograr un enrutamiento dinámico efectivo, debes configurar una única herramienta principal en tu servidor MCP que reciba un parámetro identificador clave, como tool_name o service_type. Dentro de la lógica interna de esta herramienta, implementas un diccionario o una estructura de control que mapee ese nombre directamente con la URL o el endpoint del servicio real. Por ejemplo, si el parámetro recibido es weather_tool, el código redirige la petición hacia la API meteorológica; si es database_tool, apunta a tu base de datos interna. Al pasar los parámetros adicionales (como kwargs) directamente a esa ruta seleccionada, logras que una sola función actúe como un puente universal hacia múltiples destinos. Esto simplifica enormemente la interacción para el modelo de lenguaje, ya que solo necesita entender el uso de una herramienta maestra.

- ¿Cuándo debo usar una herramienta centralizada?

Debes optar por una herramienta centralizada cuando tienes múltiples servicios que comparten una estructura de petición similar, o cuando necesitas ocultar la complejidad de tu infraestructura al cliente final. Es una estrategia ideal en arquitecturas de microservicios. Por ejemplo, si tu asistente de IA necesita consultar información de ventas, inventario y recursos humanos, en lugar de registrar tres herramientas distintas que saturen el límite de contexto del LLM, le proporcionas una sola herramienta llamada consultar_datos_empresa. El servidor MCP se encargará de leer los parámetros proporcionados por la IA y enrutar la petición al microservicio adecuado de forma invisible. Esto no solo ahorra valiosos tokens al momento de declarar las herramientas, sino que también centraliza la autenticación, el registro de logs y el manejo de errores en un único lugar.
