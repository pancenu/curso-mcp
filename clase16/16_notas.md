## Resumen

La evolución de herramientas como ChatGPT hacia funcionalidades multimodales, capaces de procesar imágenes y audio aparte del texto, impulsa la importancia de entender e implementar modos multimodales en tus servidores usando MCP. Este tipo de servidor ahora puede analizar diferentes tipos de archivos gracias a bibliotecas especializadas como NumPy y Pillow, facilitando el procesamiento y análisis de datos complejos como imágenes.

¿Qué significa modo multimodal y por qué es importante?

Modo multimodal implica trabajar con diferentes tipos de datos como texto, imágenes y audio. Hasta hace poco, herramientas como ChatGPT se limitaban al texto, pero la demanda mostró la necesidad de interacción con otros tipos de contenidos. Utilizar modo multimodal permite:

Interacciones más ricas y diversas.
Procesamiento avanzado de datos visuales y auditivos.
Desarrollar aplicaciones más completas y útiles.
¿Cómo implementar NumPy y Pillow en MCP?

Puedes extender las funcionalidades de tus servidores MCP a imágenes, gracias a bibliotecas como NumPy y Pillow:

1. Instala ambas bibliotecas:

``` pip install numpy pillow ```
2. Luego integra las referencias necesarias en tu código de servidor:

```
import numpy as np
from PIL import Image
```
 
3. Usa FastAPI para estructurar tu servidor, creando endpoints adecuados para manejar distintos tipos de archivos, por ejemplo:
```
from fastapi import FastAPI, UploadFile

app = FastAPI()

@app.post("/MCPImageBrightness")
async def analizar_brillo(archivo: UploadFile):
    imagen = Image.open(archivo.file)
    arreglo = np.array(imagen)
    brillo = arreglo.mean()
    return {"brillo": brillo, "mensaje": "la imagen ha sido procesada exitosamente"}
```
¿Cómo ejecutar y probar tu servidor?

Para ejecutar tu servidor, utiliza Uvicorn desde la terminal:

```
uvicorn server:app --host localhost --port 8000
``` 
Confirma tu servidor enviando una imagen mediante una petición CURL de esta forma:

```
curl -X POST "http://localhost:8000/MCPImageBrightness" -F "archivo=@ruta/a/tu/imagen.jpg"
``` 
Recibirás una respuesta indicando el nivel de brillo y confirmando el éxito del proceso.

¿Cómo convertirlo en una herramienta integrada?

Un reto adicional que apuntala tu aprendizaje es transformar tu endpoint POST en una herramienta directamente integrada en el servidor MCP. Es recomendable intentar este proceso por ti mismo antes de consultar las soluciones disponibles. Esto reforzará tus conocimientos prácticos en la integración de funcionalidades multimodales.

---

<!-- Jonathan Rodriguez -->

La clase es un poco caotica, primero la linea `@mcp.post("/mcp/image/brightness")`
realmente es `@app.post("/mcp/image/brightness")`

porque el metodo POST lo defines con la libreria de FastAPI no con MCP, en un frame del video el profesor lo corrigió pero no dijo nada y en el codigo de Github está la versión mala.

El comando de CURL tampoco es correcto, ha cambiado el PATH de la URL y no coincide con el que se define en el codigo.

A mi no me conecta el inspector usando STDIO, a saber si algo cambió para poder conectar y es algo que no sale en el video, no se si solo me pasa a mi.

Y por último, no me queda claro el objetivo de esta clase en concreto, implementamos tools de "add", "get_greeting" pero realmente estamos es usando FastAPI, ejecutando la url para obtener el brillo de la imagen, pero realmente no hay nada de MCP aqui, es solo FastAPI con Pillow y numpy.

Creo que la demostración de que en el inspector solo salen las tools definidas y no el POST es mas que obvio pero viene de la confusion del profesor al intentar registrar la url de FastAPI con @mcp.post (que eso no existe)

---

<!--Por Nery Alberto Cano Ortigoza-->

- ¿Cuándo debo convertir rutas en herramientas MCP?

Debes hacer esta conversión cuando quieras que un modelo de lenguaje (LLM) decida autónomamente cuándo usar esa función. Una ruta web normal (como un endpoint POST) requiere que un humano o un script externo envíe la petición. Sin embargo, si envuelves esa misma lógica matemática de procesamiento de imágenes dentro de una herramienta MCP (tool), le estás dando a la IA un "botón" que puede presionar por su cuenta. Por ejemplo, si un usuario le pide a la IA: "Revisa si esta foto está muy oscura", la IA sabrá que tiene una herramienta de cálculo de brillo disponible, la ejecutará por sí misma, leerá el resultado y le dará una respuesta natural al usuario sin intervención manual.


- ¿Es posible analizar imágenes usando solo NumPy?

Sí, y es extremadamente eficiente. Aunque NumPy es famoso por el cálculo matemático, una imagen digital no es más que una cuadrícula gigante de números donde cada píxel tiene valores de color. Al usar Pillow para abrir la imagen y NumPy para convertirla en un arreglo (array), transformas una foto en una hoja de cálculo matemática. A partir de ahí, calcular el brillo general es tan simple como sacar el promedio matemático de todos esos números. Esta técnica es la base de la visión por computadora; antes de usar redes neuronales pesadas, puedes usar estas matrices para detectar bordes, cambiar contrastes o filtrar imágenes defectuosas en milisegundos, ahorrando costos de procesamiento en la nube.


- ¿Qué pasa si pruebo el servidor con curl?

Al usar curl, estás interactuando con tu servidor MCP exactamente igual que como lo harías con cualquier API REST tradicional, saltándote la interfaz del inspector de MCP. Esto es súper útil para pruebas de integración. Si creaste una ruta POST para subir imágenes, el inspector estándar de MCP podría no tener la interfaz gráfica para adjuntar archivos fácilmente. Usar un comando curl en tu terminal te permite enviar la imagen directamente al puerto de tu servidor (por ejemplo, localhost:8000) y verificar que la lógica de procesamiento (como calcular el brillo) funciona a la perfección antes de conectar este sistema a un agente de inteligencia artificial más complejo.
