<div align="center">
  <!-- Aquí puedes poner un logo si tienes uno -->
  <h1>🚌 Colectivos San Juan - Tracker Web</h1>
  
  <p>Plataforma web para usuarios del transporte público (RedTulum) en la provincia de San Juan, Argentina. Permite consultar llegadas, ubicar vehículos en tiempo real y visualizar recorridos desde entornos de escritorio.</p>

  <p>
    <a href="https://colectivossanjuan.pages.dev"><img src="https://img.shields.io/badge/Estado-En_Producci%C3%B3n-success?style=for-the-badge" alt="Estado: En Producción" /></a>
    <img src="https://img.shields.io/badge/Plataforma-Web%20Desktop-blue?style=for-the-badge" alt="Plataforma: Web Desktop" />
  </p>
  
  <p><strong>🌐 Acceso a la plataforma:</strong> <a href="https://colectivossanjuan.pages.dev">colectivossanjuan.pages.dev</a></p>
</div>

---

## 💻 Visualización de la Plataforma

<!-- REEMPLAZA ESTOS LINKS CON TUS IMÁGENES O GIFS REALES (Preferiblemente capturas de pantalla de PC completas 1920x1080) -->
| Vista Principal del Mapa | Consulta de Paradas | Exploración de la Flota Activa |
| :---: | :---: | :---: |
| <img src="https://github.com/LeoAl1590/Colectivos-San-Juan/blob/main/assets/captura1.png" width="400"> | <img src="https://github.com/LeoAl1590/Colectivos-San-Juan/blob/main/assets/captura2.png" width="400"> | <img src="https://github.com/LeoAl1590/Colectivos-San-Juan/blob/main/assets/captura3.png" width="400"> |

## ✨ Resumen de Funcionalidades para el Pasajero

*   **Ubicación Exacta en Vivo:** Motor de tracking (`SmoothVehicleLayer`) que muestra la posición actualizada de las unidades en ruta.
*   **Tiempos de Llegada Claros:** Cálculo de ETA (Estimated Time of Arrival) por parada, frecuencias de líneas y demoras reportadas.
*   **Modo "Gran Hermano":** Opción de alejar el mapa y cargar la totalidad de los vehículos activos de la provincia simultáneamente.
*   **Alertas Comunitarias:** Sistema integrado donde los propios usuarios pueden confirmar desvíos o problemas operativos.
*   **Adaptabilidad Visual:** Soporte nativo para Modo Claro, Oscuro y AMOLED, optimizado para uso en monitores amplios sin fatiga visual.

---

## 🛠️ Stack Tecnológico y Arquitectura

La plataforma está diseñada como una SPA (Single Page Application) enfocada en el procesamiento geoespacial del lado del cliente y la recepción de datos de alta frecuencia.

### 1. Tecnologías Base
*   **Core / UI:** React 19 + Vite 7 + Tailwind CSS.
*   **Motor Gráfico (Mapas):** MapLibre GL (`maplibre-gl`), utilizando renderizado vectorial acelerado por hardware para mantener los 60 FPS independientemente de la carga de marcadores.
*   **Cálculos Geoespaciales:** `@turf/distance` y `@mapbox/polyline` para procesar trazados, descodificar rutas y calcular distancias en el cliente.
*   **Infraestructura:** Alojado en Cloudflare Pages (`.pages.dev`).

### 2. Flujo de Datos en Tiempo Real (Telemetría)
La actualización de los vehículos no se realiza por *polling* HTTP tradicional, sino a través de conexiones persistentes:
1.  Se establece una conexión **WebSocket (`ws`)** directa a los servidores de telemetría al seleccionar una parada, una línea, o al activar el modo "Gran Hermano".
2.  Las tramas de datos binarias/JSON entrantes son capturadas por el custom hook `useVehicleTracking`.
3.  Para evitar que los re-renderizados de React congelen la interfaz ante la llegada masiva de paquetes, los datos se escriben en referencias mutables (`useRef`).

### 3. Motor de Animación (`SmoothVehicleLayer`)
Dado que los GPS vehiculares emiten paquetes con varios segundos de diferencia (y a veces con saltos bruscos), la plataforma implementa una capa personalizada de MapLibre:
*   **Desacoplamiento:** El movimiento de los marcadores DOM o vectores de íconos no depende de los re-renders de los paneles de UI.
*   **Interpolación:** Cuando se recibe una nueva coordenada, el motor calcula la distancia desde el punto anterior y anima la transición de forma fluida durante el "gap" de tiempo estimado hasta el siguiente paquete.
*   **Cálculo de Bearing (Rotación):** Los íconos de los colectivos rotan automáticamente siguiendo la trayectoria de la ruta usando las matemáticas espaciales de la capa intermedia.

### 4. Sistema de Alertas (`useCommunityAlerts`)
Las alertas de servicio combinan dos fuentes de datos:
*   **Oficiales:** Reportes inyectados en la estructura de referencias del transporte público.
*   **Comunitarias:** Eventos generados por los usuarios, gestionados de forma local (confirmación de incidencias) y sincronizados con el backend para actualizar la reputación del evento.

---

## ✉️ Soporte

Para reportes de errores técnicos en la plataforma web o sugerencias, por favor utiliza la pestaña de **Issues** en este repositorio o contáctanos desde la sección "Acerca de" dentro de la misma página web.
