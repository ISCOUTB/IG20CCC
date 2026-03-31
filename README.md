# Generador de Plantillas Instagram (20CCC)

Herramienta frontend interactiva diseñada para crear contenido gráfico estandarizado para redes sociales, con énfasis en el evento académico **20CCC 2026** (Congreso Colombiano de Computación).

Permite a los usuarios generar, editar y previsualizar en tiempo real múltiples "diapositivas" (Portada, Beneficios, Temas y Cierre) con un formato institucional, visualmente armónico y adaptable a las principales proporciones de plataformas móviles.

## Características Principales

*   **Edición en Vivo:** Panel lateral izquierdo con campos editables que actualizan la previsualización del diseño en tiempo real de forma reactiva mediante JavaScript (Data Binding).
*   **Formatos Dinámicos Flexibles:** Soporte completo (basado en `flexbox` y recalibración de variables CSS) para redistribuir espacios, escalas topográficas e incrementos de relleno (`padding/gap`):
    *   **Vertical:** Relación 4:5 (`1080x1350`)
    *   **Story / Reels:** Relación 9:16 (`1080x1920`)
*   **Diseño Institucional Premium Blanco:** Paleta luminosa moderna (blancos absolutos, gris cristal, Verde institucional `#7ab648` acentuado y azul corporativo). Componentes estructurados tipo "tarjetas" e integraciones gráficas como la mención de **Springer** LNCS y panel unificado de logos organizadores.
*   **Agnóstico de Plataforma y Servidores:** Construido al 100% con Vanilla JS, CSS puro y HTML. No requiere dependencias (`Node.js`, `Webpack`, etc.), base de datos o backend. Tan solo abre el documento en cualquier navegador y captura el DOM completo como imagen para publicar.

## Estructura

*   `instagram-ad-template.html`: Documento *Single Page* que contiene a su vez las hojas de estilos modulares integradas y la lógica Javascript de manipulación del DOM debajo.
*   `20ccc-1-300x114.jpeg`: Marca principal del Congreso desplegada en las portadas.
*   `springer.png` : Certificación de publicación en el cierre (Call for Papers).
*   `SCO2.jpg`, `utb.png`, `escuela.jpg`: Logos institucionales de co-organizadores integrados nativamente en el header principal superior empleando `mix-blend-mode` para transiciones perfectas al blanco.

## Componentes

La vista está diseñada para exportar dinámicamente **5 Artes/Diapositivas** estructuradas:
1.  **Portada (`cover`):** Gran gancho principal, patrocinadores y bloque de fechas de Neumorfismo.
2.  **Beneficios (`benefits`):** Sub-sección de valor añadido para publicar y asistir.
3.  **Temas (`topics`):** Retícula iconográfica interactiva de cuadrícula para desglosar líneas temáticas como Educación, Software, etc.
4.  **Eventos (`events`):** Listado detallado para hitos críticos de cierre como *Demos de Software* o el *Workshop II MCC*. Soporta listados paralelos de cierres independientemente adaptables.
5.  **Cierre (`cta`):** Llamado a enviar el documento a revisión, donde impera la "Tarjeta de Dates" (formato apilado vía CSS) y el `Action Button` hacia la URL real de SCO2.

## Cómo Usar

Dado que los cambios se realizan estrictamente por el árbol de visualización del DOM:
1. Abre el documento `html` en un navegador como Chrome, o Firefox.
2. Llena los formularios del entorno gráfico en la pestaña de `Controls`.
3. Selecciona tu formato 4:5 o 9:16 desde los botones interactivos superiores.
4. Usa extensiones de navegador que permitan capturar elementos del DOM (`Capture Node Screenshot`) o la herramienta nativa del inspector web para capturar y descargar el bloque HTML de la tarjeta (`.poster` node) en formato `.png` de altísima resolución.
