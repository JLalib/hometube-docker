# HomeTube Docker

Este repositorio contiene la configuración necesaria para desplegar **HomeTube**, un descargador web self-hosted basado en yt-dlp y Streamlit, capaz de descargar vídeos de más de 1800 plataformas y organizarlos en tu biblioteca.

## 🚀 ¿Qué es HomeTube?

HomeTube permite descargar vídeos sueltos de una gran variedad de sitios (YouTube, TikTok, Twitch, Instagram, etc.) con un flujo directo y sencillo. Incluye funciones avanzadas como:

- **SponsorBlock**: Bloqueo automático de sponsors, intros y outros.
- **Procesado avanzado**: Recorte de clips, incrustación de subtítulos y control de calidad.
- **Soporte de Cookies**: Permite descargar contenido restringido (edad, membresías o geo-bloqueos).
- **Organización**: Los vídeos se guardan automáticamente en carpetas organizadas.

## ✅ Requisitos del sistema

- **Docker Engine** y **Docker Compose** V2.
- Sistema operativo basado en Linux.

## 🛠️ Instalación

Para poner en marcha HomeTube, sigue estos pasos:

1. **Clona este repositorio o descarga el archivo `docker-compose.yml`**.
2. **Crea las carpetas necesarias** en el mismo directorio que el compose:
   ```bash
   mkdir downloads cookies
   ```
3. **Inicia el servicio** en segundo plano:
   ```bash
   docker compose up -d
   ```

## 🌐 Acceso a la interfaz web

Una vez desplegado, puedes acceder a HomeTube a través de tu navegador:

`http://<tu-ip-servidor>:8501`

## ⚙️ Configuración y Variables

En el archivo `docker-compose.yml` puedes ajustar las siguientes variables:

- `TZ`: Tu zona horaria (ej. `Europe/Madrid`).
- `VIDEOS_FOLDER`: Ruta interna donde se guardarán los vídeos.
- `YOUTUBE_COOKIES_FILE_PATH`: Ruta al archivo de cookies para evitar restricciones.

---
Basado en el tutorial de [Genbyte](https://genbyte.blogspot.com/2025/12/instalar-hometube-con-docker-descarga.html).
