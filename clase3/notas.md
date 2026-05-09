## Información previa

Puntos clave:

- Python 3: base del desarrollo. En el ejemplo se reporta 3.13.7.
- pip: gestor de paquetes de Python. Se menciona pip en versión veinticinco.
- Uvicorn: mini servidor web útil con FastAPI para pruebas locales.
- npx (npm/Node): necesario para ejecutar el inspector gráfico de MCP.

¿Para qué sirve cada herramienta en MCP?

- Python: lenguaje elegido para el desarrollo en el curso.
- pip: instalación de dependencias puntuales cuando haga falta.
- UV: creación de entornos virtuales para aislar paquetes y evitar conflictos.
- Uvicorn: ejecución de servicios web de prueba, común con FastAPI.
- npx: ejecución del inspector de MCP sin instalaciones globales.

¿Por qué usar WSL o Linux en el entorno?

El trabajo se hará en una terminal de Ubuntu usando WSL en Windows. Esto facilita una experiencia similar a Linux incluso en equipos Windows. Si usas macOS o Linux, puede haber variaciones menores, pero la lógica es la misma: terminal y herramientas al día.

¿Qué conocimientos previos y contexto recomienda el instructor?

MCP se ubica dentro de la especialidad de AI Engineer. Para aprovechar mejor, se sugiere haber completado los fundamentos de LLM y inteligencia artificial, además de programar con soltura en el lenguaje elegido.

¿Qué conocimientos previos de LLM e IA se sugieren?

- Conceptos base de modelos de lenguaje.
- Fundamentos de inteligencia artificial aplicados.
- Vocabulario y práctica mínima para entender la terminología del curso.

¿Qué nivel de programación necesitas?

- Manejo de sintaxis y módulos del lenguaje que usarás.
- Capacidad para instalar paquetes y entender errores.
- Confort con terminal y comandos básicos.

Ventaja: si dominas estos fundamentos, avanzarás con fluidez y sin frenarte por temas básicos.

¿Cómo verificar el entorno y preparar el despliegue?

- El objetivo inmediato es comprobar que el equipo está listo antes de construir. Se valida en una máquina “limpia” para confirmar que nada falta y que todo fluye desde la terminal.

¿Cómo verificar instalaciones con comandos?

- Confirmar versiones de Python y pip.
- Instalar Uvicorn con pip si hace falta.
- Asegurar npx operativo para el inspector de MCP.

Sugerencias prácticas:

- Limpiar la terminal al cambiar de tarea.
- Documentar versiones para reproducibilidad.

¿Qué pasa con pip y los entornos virtuales?

En Linux o WSL, Python restringe instalaciones globales con pip por seguridad. La solución es trabajar dentro de un entorno virtual. Con UV se crearán y gestionarán estos entornos para instalar dependencias sin afectar el sistema.

Beneficios:

- Aislamiento de paquetes por proyecto.
- Menos conflictos de versiones.
- Configuración más segura.

¿Se necesita suscripción de Azure para MCP?

No es obligatoria, pero es altamente deseable para desplegar el servidor de MCP en la nube y usar recursos asociados. Puedes crear una suscripción gratuita con cerca de 200 dólares de crédito. El consumo estimado para el curso es de 3 a 4 dólares bien administrados.

Te leo: ¿qué sistema operativo usarás y qué dudas tienes al preparar tu entorno para MCP?


Instalar uvicorn
```
python3 --version
pip3 --version
pip3 install uvicorn
npx --version
``` 

## Correspondientes a esta clase

Dentro del directorio de trabajo ejecutar:
``` 
pip install mcp-cli
pip install "mcp[cli]"
``` 
e invocar allí code 
```
code .
```
Recomendación para instalar 
```   
  npm install -g @modelcontextprotocol/server-everything
```
Documentación que puede ser útil: [https://modelcontextprotocol.io/docs/tools/inspector](https://modelcontextprotocol.io/docs/tools/inspector)


# Clase 3

**Resumen**

Al iniciar en el desarrollo con MCP, es clave entender cómo preparar un entorno de trabajo funcional y adaptable. Este proceso no tiene que ser complicado y permite elegir entre diferentes herramientas según tus preferencias. Aquí, te mostramos cómo hacerlo paso a paso usando Python y Visual Studio Code (VSCode), aunque también existen opciones alternativas para distintos entornos y lenguajes.

¿Qué opciones existen para configurar un entorno de trabajo MCP?

Puedes trabajar con VSCode, Eclipse, IntelliJ, Visual Studio o incluso soluciones en la nube como Cloud de Anthropic. Todas estas opciones son válidas, y la decisión dependerá de lo que más te acomode. Es importante destacar que aunque el ejemplo se realiza en * Python*, existen SDKs para otros lenguajes: TypeScript, C sharp, Java y Node. Esto brinda gran flexibilidad para adaptar MCP a tus necesidades.

¿Cómo instalar y preparar MCP usando Python?

El primer paso práctico es preparar un repositorio vacío. Dentro de él:

Crear un nuevo directorio usando mkdir clase_tres.

Acceder a este directorio y limpiar la terminal.

Instalar el paquete de pip para MCP ejecutando:

pip install mcp-cli
Esto proporciona una herramienta de línea de comandos que ayuda a gestionar todos tus proyectos MCP.
Una vez instalado, abre el directorio en VSCode usando:


code .
Crea un archivo server.py. Opcionalmente puedes crear un entorno virtual de Python, pero para comenzar, trabajar directamente te permite avanzar rápido.

¿Cuál es el flujo básico para correr un servidor MCP?

En server.py debe incluirse un import de la librería MCP y la creación de un servidor, por ejemplo:


import fastmcp
mcpfastdemo = fastmcp.Server()
Puedes agregar recursos y herramientas personalizadas. Luego, desde la terminal, ejecuta:


mcp dev server.py
Verás que el servidor responde mostrando un token de sesión y la entrada al MCP Inspector. Si al intentar conectar en el inspector surge algún problema de conectividad, detén con control+C, limpia la terminal y ejecuta:


mcp run server.py
Esto suele solucionar cualquier inconveniente de conexión.

¿Cómo usar el MCP Inspector para revisar recursos y herramientas?

El Inspector permite listar recursos y herramientas del servidor. Puedes ver:

Recursos definidos, como greeting con el parámetro name.
Herramientas, como sumar dos números, accesibles desde el inspector.
El Inspector facilita rastrear todos los componentes, visualizar su estructura y chequear desde la interfaz lo que un cliente podría consumir. Utiliza la función de listar recursos y herramientas para validar la implementación.

¿Qué ejercicios prácticos ayudan a reforzar el aprendizaje?

Para mejorar la experiencia:

Agrega métodos como restar, multiplicar y dividir a tu servidor.
Crea recursos que permitan respuestas más personalizadas, por ejemplo: «Tu nombre completo es...», usando parámetros.
Experimenta con combinaciones de herramientas y recursos para ampliar las funcionalidades de tu servidor MCP.
¿Tienes ideas o dudas respecto a la configuración o uso de MCP? Comparte abajo tu experiencia y enriquece la conversación con nuevas propuestas y experimentos.
