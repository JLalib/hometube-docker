# ▶️ HomeTube Docker: Descarga vídeos de +1800 plataformas con yt-dlp y Streamlit

[![GitHub](https://img.shields.io/badge/GitHub-Repositorio-blue)](https://github.com/JLalib/hometube-docker)
[![HomeTube](https://img.shields.io/badge/Docker-Imagen-blue)](https://hub.docker.com/r/jlalib/hometube)
[![License](https://img.shields.io/badge/Licencia-MIT-green)](https://github.com/JLalib/hometube-docker/blob/main/LICENSE)

## 📋 Descripción general

HomeTube es un descargador web self-hosted basado en yt-dlp y Streamlit, capaz de descargar vídeos de más de 1800 plataformas y organizarlos en tu biblioteca. Incluye funciones avanzadas como SponsorBlock, procesado de clips, incrustación de subtítulos y soporte de cookies para contenido restringido.

Esta solución proporciona una instalación sencilla y mantenible mediante Docker Compose, permitiéndote tener tu propio servicio de descarga de vídeos autoalojado y privado.

## ✨ Características principales

- **Descarga de +1800 plataformas**: YouTube, TikTok, Twitch, Instagram, Facebook, Vimeo, Reddit y muchas más
- **SponsorBlock integrado**: Bloqueo automático de sponsors, intros, outros y promociones
- **Procesado avanzado**: Recorte de clips, incrustación de subtítulos y control de calidad
- **Soporte de cookies**: Descarga contenido restringido por edad, membresías o geo-bloqueos
- **Organización automática**: Los vídeos se guardan en carpetas estructuradas por plataforma y fecha
- **Interfaz web moderna**: Basada en Streamlit, fácil de usar y responsive
- **Formato de salida configurable**: Por defecto MKV, con opciones para otros formatos
- **Reinicio automático**: Política de reinicio `unless-stopped` para alta disponibilidad
- **Imagen personalizada**: Basada en la imagen oficial de Python con dependencias preinstaladas

## 📋 Requisitos del sistema

- Docker Engine (versión 20.10 o superior)
- Docker Compose (v2 recomendado)
- Al menos 512MB de RAM disponibles
- Espacio en disco según tus necesidades de almacenamiento de vídeos
- Puerto 8501 libre para la interfaz web (personalizable)
- Conexión a internet para descargar el contenido

## 🐳 Instalación

### Docker Compose (Recomendado)

Crea un archivo `docker-compose.yml` con el siguiente contenido:

```yaml
services:
  hometube:
    image: ghcr.io/jlalib/hometube:latest
    container_name: hometube
    restart: unless-stopped
    ports:
      - "8501:8501"
    environment:
      - TZ=Europe/Madrid
      - VIDEO_OUTPUT_FORMAT=mkv
      - ENABLE_SPONSORBLOCK=true
    volumes:
      - ./downloads:/app/downloads
      - ./cookies:/app/cookies
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8501/_stcore/health"]
      interval: 30s
      timeout: 10s
      retries: 5
```

Luego, inicia el servicio:

```bash
docker compose up -d
```

## ⚙️ Configuración

### Variables de entorno principales

| Variable | Valor por defecto | Descripción |
|----------|-------------------|-------------|
| `TZ` | `Europe/Madrid` | Zona horaria para marcas de tiempo y programación |
| `VIDEO_OUTPUT_FORMAT` | `mkv` | Formato de salida de los vídeos (mkv, mp4, webm, etc.) |
| `ENABLE_SPONSORBLOCK` | `true` | Activar o desactivar el bloqueo de sponsors vía SponsorBlock |

### Configuración avanzada

- **Personalización de rutas**: Modifica las rutas de los volúmenes (`./downloads` y `./cookies`) si necesitas almacenarlos en ubicaciones específicas.
- **Puertos personalizados**: Cambia el mapeo de puertos (ej: `"8080:8501"`) si el puerto 8501 está en uso.
- **Variables adicionales**: La imagen de Hométube soporta más variables de configuración para ajustar el comportamiento de yt-dlp y Streamlit. Consulta la documentación del proyecto para la lista completa.
- **Actualizaciones**: Para actualizar a la última versión, ejecuta `docker compose pull` seguido de `docker compose up -d`.

## 🚀 Primeros pasos

1. **Crear las carpetas necesarias**
   ```bash
   mkdir -p downloads cookies
   ```

2. **Levantar el contenedor**
   ```bash
   docker compose up -d
   ```

3. **Verificar que está funcionando**
   ```bash
   docker compose logs -f
   ```
   Deberías ver mensajes indicando que:
   - El contenedor hometube se ha iniciado correctamente
   - La aplicación está escuchando en el puerto 8501
   - El healthcheck está pasando

4. **Acceder a la interfaz web**
   - Abre tu navegador y navega a `http://[IP de tu servidor]:8501`
   - En la interfaz, pega la URL del vídeo que deseas descargar
   - Configura las opciones según tus necesidades (formato, sponsors, etc.)
   - Haz clic en "Descargar" y espera a que el proceso finalice
   - El vídeo se guardará en la carpeta `downloads` organizada por plataforma

## 💡 Casos de uso

- **Biblioteca multimedia personal**: Descarga y organiza tus vídeos favoritos de múltiples plataformas
- **Contenido educativo**: Guarda tutoriales, conferencias y material de estudio para ver sin conexión
- **Entretenimiento**: Descarga episodios de series, películas o contenido de creadores independientes
- **Archivado**: Crea copias de seguridad de vídeos importantes antes de que puedan ser eliminados
- **Desarrollo y pruebas**: Usa como herramienta para obtener vídeos de prueba en proyectos de procesamiento de video

## 🔒 HTTPS con Caddy (acceso remoto seguro)

Si deseas acceder a tu instancia de HomeTube desde fuera de tu red local de forma segura, puedes usar Caddy para obtener un certificado gratuito de Let's Encrypt.

### Configuración Caddyfile (ejemplo)
```caddy
hometube.tudominio.com {
    reverse_proxy localhost:8501
    encode gzip
}
```

> **⚠️ Importante**: HomeTube no tiene autenticación incorporada en la capa de red. Si expones el servicio públicamente, considera:
> 1. Añadir autenticación en tu proxy inverso (Caddy/Nginx)
> 2. Limitar el acceso por IP o rango de IPs
> 3. Usar fail2ban para proteger contra ataques de fuerza bruta
> 4. Cambiar los puertos por defecto si lo deseas
> 5. Siempre mantener el software actualizado

### Acceso remoto seguro
1. Configura Caddy con tu dominio.
2. HTTPS se habilita automáticamente con Let's Encrypt.
3. Accede desde cualquier lugar: `https://hometube.tudominio.com`
4. Usa la interfaz web normalmente con tu navegador.

## 🛠️ Gestión y mantenimiento

### Ver registros
```bash
docker compose logs -f
```

### Copia de seguridad de configuración
```bash
# Solo necesitas respaldar el docker-compose.yml y .env si lo usas
cp docker-compose.yml /ruta/de/backup/
# Para respaldar la configuración real, copia los volúmenes o las carpetas de datos
cp -r downloads cookies /ruta/de/backup/hometube-data/
```

### Actualizar a la última versión
```bash
docker compose pull
docker compose up -d
```

### Reiniciar servicios
```bash
docker compose restart
```

### Ver estado de salud
```bash
docker compose ps
```

### Copia de seguridad de las descargas y cookies
```bash
# Crear un archivo comprimido con tus datos
tar czf hometube-backup-$(date +%Y%m%d).tar.gz downloads cookies
```

### Restaurar desde copia de seguridad
```bash
# Detener el contenedor si es necesario
docker compose stop hometube
# Restaurar los datos
tar -xzf hometube-backup-$(date +%Y%m%d).tar.gz
# Reiniciar el contenedor
docker compose start hometube
```

## 📝 Licencia

Este proyecto se basa en [HomeTube](https://github.com/yt-dlp/streamtubes) (concepto) y utiliza [yt-dlp](https://github.com/yt-dlp/yt-dlp) y [Streamlit](https://github.com/streamlit/streamlit) bajo sus respectivas licencias. La configuración y documentación proporcionada aquí está bajo la [MIT License](https://github.com/JLalib/hometube-docker/blob/main/LICENSE).

---

> ✨ **Nota**: Este repositorio contiene la configuración Docker y documentación extraída del tutorial de Genbyte: [Instalar HomeTube con Docker: descarga vídeos de +1800 plataformas](https://genbyte.blogspot.com/2025/12/instalar-hometube-con-docker-descarga.html)