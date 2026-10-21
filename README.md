<p align="center">
  <img src="static/qwentts.webp" alt="Qwen TTS WebUI" width="250">
</p>

# Qwen3 TTS WebUI: clonación de voz con IA

Proyecto del curso **IA Generativa** (Ingeniería de Sistemas, Universidad Santo Tomás, Tunja).

Interfaz web para **clonar una voz** con el modelo **Qwen3-TTS** de Alibaba: subes un audio de referencia de una persona y el sistema genera nuevas frases con esa misma voz. Está basado en el proyecto de código abierto [qwen3-tts-webui](https://github.com/AfkaraLP/qwen3-tts-webui) de AfkaraLP.

## Funcionalidades

- **Clonación de voz** a partir de un audio de referencia (subido, grabado en el navegador o descargado de YouTube).
- **Transcripción automática** del audio de referencia con Whisper.
- **Varios idiomas** para el texto generado.
- **Gestión de audios**: escuchar, renombrar y descargar referencias y resultados.
- **API REST** con documentación automática (FastAPI).

## Cómo funciona

1. El servidor (FastAPI + Uvicorn) carga el modelo `Qwen/Qwen3-TTS-12Hz-1.7B-Base`.
2. El usuario sube un audio de referencia, que se guarda en `reference_audios/`, y Whisper lo transcribe.
3. Se escribe el texto a generar y se elige el idioma.
4. Qwen3-TTS sintetiza el texto imitando la voz de referencia. El resultado queda en `output/`.

## Tecnologías

- Python 3.10
- [uv](https://docs.astral.sh/uv/) para el entorno y las dependencias
- PyTorch con CUDA 12.8
- `qwen-tts`, `openai-whisper`, `librosa`, `pydub`, `yt-dlp`
- FastAPI y Uvicorn (backend), HTML/CSS/JavaScript (frontend en `static/`)

## Requisitos

- GPU NVIDIA compatible con CUDA, con unos 6 GB de VRAM (recomendado).
- Python 3.10 o superior y uv.
- FFmpeg instalado (para procesar audios y descargas de YouTube).

## Instalación y uso

```bash
git clone https://github.com/Mans0089/qwen3-tts-webui-ia-gen.git
cd qwen3-tts-webui-ia-gen
uv sync
uv run start_server.py
```

La primera ejecución descarga los modelos, así que tarda un poco. Cuando la terminal diga que el modelo cargó, abre:

- Interfaz web: http://localhost:8000
- Documentación de la API: http://localhost:8000/docs

### Opciones del servidor

```bash
uv run start_server.py --port 3000       # otro puerto
uv run start_server.py --device cpu      # forzar CPU (mucho más lento)
uv run start_server.py --device cuda:0   # elegir GPU
```

## Uso de la interfaz

1. **Referencia**: sube un archivo, graba con el micrófono o pega un enlace de YouTube. Lo ideal son 10 a 30 segundos de voz limpia.
2. **Texto**: escribe lo que quieres que diga la voz clonada y elige el idioma.
3. **Generar**: pulsa el botón de clonar. El audio aparece en la sección de audios generados para escucharlo o descargarlo.

## Estructura

```
start_server.py              # arranca el servidor
src/qwen_voice_cloning/
  core.py                    # carga del modelo y clonación
  server.py                  # endpoints de la API
static/                      # interfaz web
reference_audios/            # audios de referencia
output/                      # audios generados
```

## Uso responsable

Clona solo voces propias o de personas que hayan dado su permiso. Revisa también los términos de uso del modelo Qwen3-TTS antes de cualquier uso comercial.

## Créditos

- Proyecto original: [qwen3-tts-webui](https://github.com/AfkaraLP/qwen3-tts-webui) de AfkaraLP (licencia MIT, ver `LICENSE.txt`). Su README original está en `README.original.md`.
- Modelo: [Qwen3-TTS](https://huggingface.co/Qwen) de Alibaba Cloud.
- Adaptación y documentación: Manuel Santiago Martínez Sierra ([@Mans0089](https://github.com/Mans0089)).
