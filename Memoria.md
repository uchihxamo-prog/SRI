Servicios de Streaming - Servicios en Red Índice Descarga directa vs StreamingTopología de redCapa de transporte: TCP vs UDPQoS: Jitter, BufferProtocolos streamingIcecast1. 
Descarga directa vs Streaming 
Descarga Directa: El usuario demanda un fichero (ej. 100MB, 10 min). Se descarga completamente, se almacena en el buffer y comienza la reproducción. Si el usuario para a los 2 minutos, el servidor ya ha entregado los 100MB completos. 
Streaming: Datos enviados en flujo constante. No hay almacenamiento local permanente. Solo se consume el ancho de banda utilizado (siguiendo el ejemplo anterior, solo los 2 minutos visualizados). 
2. Topología de red Unicast Mecánica: Conexión 1 a 1 (estándar de internet). Para 100 oyentes, el servidor abre 100 sockets TCP y envía la información 100 veces.
Cálculo de Ancho de Banda: $BW(tot) = BW(stream) \times N(usuarios)$.
Desventaja: Poco escalable. 
Multicast Mecánica: El servidor envía la información a una dirección específica (224.0.0.0 - 239.255.255.255). Los routers replican el paquete solo si tienen suscriptores. 
Desventaja: Muchos routers bloquean estos paquetes. Solo es viable en redes internas. 3. Capa de transporte: TCP vs UDP TCP: Si se pierde un paquete, se reenvía (ACK/NACK). 
Ventaja: Calidad garantizada y atraviesa firewalls/proxies fácilmente. 
Desventaja: Alta latencia por las retransmisiones. 
UDP: Sacrifica calidad a cambio de una latencia mínima. 
4. QoS: Jitter, Buffer Jitter (Fluctuación): Variación en el tiempo de llegada de los paquetes. Si es superior al buffer, se producen cortes (Buffer Underrun). 
Buffer (Amortiguador): Memoria temporal para absorber el Jitter. A mayor buffer, más estabilidad pero mayor latencia. 
Burst-on-Connect: Característica de Icecast que envía los datos iniciales a máxima velocidad para llenar el buffer del cliente casi instantáneamente.
5. Protocolos de Streaming Modelos de Capa de Aplicación
HTTP Legacy (ICY): Usado por Icecast2. Flujo continuo de bytes vía TCP. 
HTTP Adaptativo (HLS/DASH): El servidor trocea el fichero en chunks (2-10s). Permite calidad adaptativa mediante un Manifest.
Real-Time:
RTMP: Sobre TCP. Usado para ingesta (OBS a servidores).
RTSP: Usado en CCTV. UDP para datos y TCP para control.
WebRTC: Videoconferencia P2P vía UDP en navegador. 
Cuadro Resumen ProtocoloBaseLatenciaUso PrincipalIcecast (ICY)TCP/HTTP10s - 30sRadio OnlineHLS 
/ DASHTCP/HTTP15s - 45sNetflix, YouTube
RTMP TCP2s - 5sIngesta (OBS)WebRTCUDP/TCP< 0.5sZoom, Meet, Discord
RTSP UDP+TCP< 1sCámaras IP6. Icecast 2 Servidor de streaming de código abierto que actúa como una "antena de radio virtual". 
Formatos: OGG / MP3. Puntos de montaje: Permite diferentes emisiones (ej. /radio-asir). 
Instalación básica: apt install icecast2. 
Códecs y Cálculos Códecs: Algoritmos de compresión/descompresión (Audio: MP3, AAC; Vídeo: H.264, H.265). 
Cálculo Peso WAV: $P = \text{Frecuencia} \times \text{Profundidad} \times \text{Canales} \times \text{Tiempo(s)}$. Vídeo: Introduce el concepto de Contenedor (MP4, MKV) que incluye vídeo, audio y metadatos.
Ejemplo de Práctica con FFmpeg Bash# Cambio de contenedor (Remuxing)
ffmpeg -i original.mp4 -c:v copy -c:a copy salida.mkv

# Comprimir a H.264 con bitrate de 2Mbps
ffmpeg -i original.mp4 -c:v libx264 -b:v 2M -c:a copy h264_2mbps.mp4


**bits (b):** Se usan para medir el **Ancho de Banda** y el **Bitrate** (velocidad).
    * *Ejemplo:* 100 Mbps (Megabits por segundo).
* **Bytes (B):** Se usan para medir el **Almacenamiento** (peso del archivo).
    * *Ejemplo:* 500 GB (Gigabytes de disco).

DE GB A MB HAY QUE X 1024 Y DE MB A Mb x 8

## 1. Video RAW (4K)
Calculamos el bitrate resultante para un flujo de video sin compresión.

### Datos:
* **Resolución:** $3840 \times 2160$ (UHD)
* **Frecuencia de imagen:** 60 fps
* **Profundidad de color:** 30 bits

Bitrate = \Resolución Horizontal \Resolución Vertical \FPS\Profundidad de 

### Datos:
* **Frecuencia de muestreo:** 44.1 kHz (44,100 Hz)
* **Profundidad de bits:** 16 bits
* **Canales:** 2 (Estéreo)
* **Duración:** 5 minutos (300 segundos)

### Fórmula:
$$Tamaño = \frac{\text{Frecuencia (Hz)} \times \text{Bits} \times \text{Canales} \times \text{Segundos}}{8 \times 1,000,000}$$
Bitrate=Frecuencia Bits Canales


porcentaje de uso = bitrate del directo/ancho de banda de subida x 100
Para determinar la cantidad de usuarios (o conexiones simultáneas):

Usuarios = \frac{Capacidad total de la línea/{Bitrate por usuario}}$$

**Fórmula:** (Bitrate * Oyentes) 


Tamaño = Bitrate (Gbps) x Tiempo (s) /8


Tienes un disco de 500 GB. ¿Cuántas horas de vídeo HD a 2 Mbps podrías alojar aproximadamente?
* **Datos:** Disco de 500 GB, Video a 2 Mbps.
* **Fórmula:** Horas = (Capacidad GB * 8,000) / (Bitrate Mbps * 3,600)
* **Resultado:** 4,000,000 / 7,200 = **555.55 horas**

Si emites un streaming de audio a un bitrate constante (CBR) de 128 kbps y tienes 25 oyentes simultáneos en una red Unicast, ¿cuál es el ancho de banda total consumido?
Oyentes = Límite de Subida (kbps)/Bitrate por Oyente

# Guía de Cálculos: Streaming y Audio

Resumen de fórmulas para gestión de ancho de banda y almacenamiento.

---

## Conversiones Básicas
* **Bytes a bits:** x 8
* **bits a Bytes:** / 8
* **Multicast:** x 1 (flujo único)

---

## 1. Porcentaje de Uso de Red
Calcula la carga que genera el streaming sobre tu conexión.

**Fórmula:**
(Bitrate del directo / Ancho de banda de subida) * 100 = % de uso

---

## 2. Tiempo de Grabación en Disco
Para saber cuántas horas de video puedes guardar.

1. **Capacidad a Megabits:** MB * 8 = Mb
2. **Segundos totales:** Mb / Bitrate (Mbps) = Segundos
3. **Pasar a horas:** Segundos / 3600 = Horas

---

## 3. Peso de Audio Digital (Calidad CD)
Para calcular el tamaño de un archivo de audio.

**Paso 1: Bitrate (bps)**
Frecuencia * Profundidad * Canales = bps

**Paso 2: Peso Total**
1. (bps * segundos) = Total bits
2. Total bits / 8 = Bytes
3. Bytes / 1.000.000 = Megabytes (MB)

---

## 4. Capacidad de Oyentes
Determina cuántos usuarios soporta tu servidor.

**Ejemplo:**
* Subida: 100 Mbps (100.000 Kbps)
* Calidad de audio: 256 Kbps

**Cálculo:**
100.000 / 256 = 390 oyentes

---

## 5. Espacio Total (Resumen)
1. Convertir duración a Segundos.
2. Peso Total = Segundos * (Bitrate en Mbps / 8).
3. Convertir resultado a GB (Dividir entre 1024).


Un usuario quiere escuchar un archivo de 100 MB. Si su conexión es de 10 Mbps, ¿cuánto tiempo tardará en completarse la "Descarga Directa" antes de asegurar que no haya cortes?

pasar todo a las mismas unidades y dividir


Una canción de 4 minutos pesa 42,33 MB en formato WAV. Si se comprime a MP3 a 128 kbps, ¿cuál será su nuevo peso aproximado?
Duración: 240s
* Bitrate: 128 kbps
* Cálculo: (240 * 128) / 8 / 1024 = 3,75 MB
