**Resumen**

Crear un Large Language Model (LLM) usando la plataforma Azure AI Foundry facilita integrar inteligencia artificial a proyectos como MCP Server. Al desplegar este modelo en Azure, evitarás utilizar el crédito dado aprovechando las capas gratuitas disponibles, garantizando eficiencia en costos y agilidad en implementación.

¿Qué necesitas para desplegar tu modelo en Azure AI Foundry?

Para desplegar efectivamente un LLM con Azure Foundry, debes tener previamente:
- Una suscripción activa en Azure.
- Un grupo de recursos configurado (por ejemplo, Platzi RG).

Dentro del portal de Azure AI Foundry, es clave guardar estos datos esenciales:
- API version.
- Endpoint.
- Llave de acceso (key).

Estos datos serán necesarios al configurar posteriormente tu entorno local.

¿Cómo configurar el archivo ENV para usar tu modelo en BSCode?

El archivo de configuración en BSCode esencial para comunicar tu entorno local con Azure AI Foundry es el archivo `.env.` Para prepararlo, sigue este procedimiento:

- Renombra el archivo proporcionado como `env-sample`, quitando "sample" para dejarlo como `.env`.
- Añade los tres datos guardados del portal Azure AI Foundry: versión del API, endpoint, y llave de acceso.
- Verifica que el modelo a utilizar también se encuentre definido en este archivo, evitando configuraciones adicionales.

¿Cuáles son las consideraciones claves en tu código Python?

En el archivo Python principal del proyecto, debes importar paquetes esenciales como `async IO`, os y bibliotecas como `agents.mcp`. Asegúrate de:

- Definir claramente la función para obtener clientes de Azure OpenAI con tus credenciales.
- Crear una función de ejecución que combine servidor y cliente MCP.
- Establecer claramente los prompts que enviarán consultas específicas al LLM.

¿Cómo generar y personalizar los prompts para mejores resultados?

El ejemplo sugiere prompts como:
- "Lee los archivos en la carpeta sample files y enlista los nombres de los archivos."
- "¿Cuál es mi libro favorito? Mira el archivo favorite_books.txt."

Optimiza estos mensajes ajustándolos claramente a consultas precisas para mejores resultados. Experimenta utilizando ciclos o listas de mensajes diferentes.

¿Cómo preparar y ejecutar correctamente el entorno local?

Para asegurarte que tu aplicación local funciona correctamente, sigue estas pautas:
- Usa un entorno virtual (virtual environment) en Python.
- Instala todas las dependencias de forma adecuada con `pip install -r requirements.txt`.
- Ejecuta tu archivo principal con el entorno virtual activo mediante comandos claros y concisos.

Esto confirmará rápidamente la comunicación efectiva entre tu aplicación local y Azure AI Foundry.

Te invito a explorar más incorporando tus propios archivos y ajustando los comandos para personalizar los resultados entregados por tu MCP Server y el LLM en Azure.

---
<!-- Miguel Gutierrez -->

Para los que quieran hacer el ejercicio usando OpenAi directamente, aquí les dejo un script de ejemplo:

``` 
import asyncio
import os
import shutil

from agents import Agent, OpenAIChatCompletionsModel, Runner, set_tracing_disabled
from agents.mcp import MCPServer, MCPServerStdio
from dotenv import load_dotenv
from openai import AsyncOpenAI

load_dotenv()


def get_open_ai_client():
    """
    Crea y regresa una instancia de cliente de OpenAI.

    Returns:
        OpenAI: Configura al cliente de OpenAI
    """

    return AsyncOpenAI(
        api_key=os.getenv("OPENAI_API_KEY"),
    )


async def run(mcp_server: MCPServer):

    open_ai_client = get_open_ai_client()
    set_tracing_disabled(disabled=True)

    agent = Agent(
        name="Asistente de Archivos",
        instructions="Usa las herramientas para leer los archivos del sistema y responder preguntas basadas en esos archivos.",
        model=OpenAIChatCompletionsModel(
            model=os.getenv("OPENAI_CHAT_MODEL"), openai_client=open_ai_client
        ),
        mcp_servers=[mcp_server],
    )

    message = "Lee los archivos en la carpeta `sample_files` y enlista los nombres de los archivos."
    print(f"Ejecutando: {message}")
    result = await Runner.run(starting_agent=agent, input=message)
    print(result.final_output)

    message = "Cuál es mi libro favorito? Mira el archivo `sample_files/favorite_books.txt`."
    print(f"\nEjecutando: {message}")
    result = await Runner.run(starting_agent=agent, input=message)
    print(result.final_output)

    message = "Mira mis canciones favoritas. Sugiere una canción que podría gustarme."
    print(f"\nEjecutando: {message}")
    result = await Runner.run(starting_agent=agent, input=message)
    print(result.final_output)


async def main():
    current_dir = os.path.dirname(os.path.abspath(__file__))
    samples_dir = os.path.join(current_dir, "sample_files")

    async with MCPServerStdio(
        name="Filesystem Server, via npx",
        params={
            "command": "npx",
            "args": ["-y", "@modelcontextprotocol/server-filesystem", samples_dir],
        },
    ) as server:
        await run(server)


if __name__ == "__main__":
    if not shutil.which("npx"):
        raise RuntimeError(
            "npx is not installed. Please install it with `npm install -g npx`."
        )

    asyncio.run(main())
``` 

Tener en cuenta estas versiones en las dependencias: 
``` <u>python-dotenv</u>==1.1.1 <u>openai-agents</u>==0.2.3 <u>openai</u>==1.97.1 ```

---

<!-- Andrea Alexandra Mora Vega -->

Idea clave (qué rol juega Azure AI Foundry)

> Azure AI Foundry no es el MCP Server. > Es el proveedor del modelo. > El MCP Server es el intermediario controlado.

Roles claros:

- Azure AI Foundry → aloja y factura el LLM
- MCP Server (Python) → decide cómo y cuándo se usa el modelo
- LLM → genera texto / razonamiento
- VS Code / Copilot / App → Host MCP

---

<!-- Por Javier Medel -->

Para configurar el File System MCP y restringir el acceso solo a la carpeta sample_files, debes asegurarte de que al crear tu MCP server, especifiques el directorio de trabajo y limites los permisos de acceso. En el código, al implementar la función get Azure OpenAI Client, puedes definir el contexto del sistema de archivos, especificando que solo se puede acceder a la carpeta sample_files. Esto se realiza mediante la configuración adecuada en el código del agente, asegurando que el entorno esté controlado y que los prompts solo hagan referencia a esa carpeta.
