# Manual de Diseño del Proyecto: La Fábrica de Agentes
Este documento resume y especifica las directrices de diseño, tokens visuales, componentes, patrones estéticos y el sistema adaptativo responsivo de la landing page de **La Fábrica de Agentes** de ENAE Business School.

---

## 1. Fundamentos de Marca y Filosofía Visual

La estética de **"La Fábrica de Agentes"** combina una atmósfera de tecnología premium (Dark Mode de alto rendimiento y contrastes sutiles) con la seriedad académica y corporativa de **ENAE Business School** (usando su tono carmesí característico como color de acento principal).

La experiencia visual se estructura a través de un **ritmo de alternancia estricto de secciones oscuras y claras**, lo que segmenta el contenido de forma natural y mantiene al lector con un alto nivel de atención e interés.

---

## 2. Paleta de Colores y Tokens

### Variables del Sistema (CSS `:root`)

Las siguientes variables globales representan los cimientos del diseño del sitio, permitiendo que la mayoría de componentes respondan automáticamente al flujo temático:

```css
:root {
    --bg-color: #000000;
    --text-color: #ffffff;
    --accent-color: #a81730;                 /* Carmesí Corporativo ENAE */
    --accent-glow: rgba(168, 23, 48, 0.4);   /* Glow de acento */
    --secondary-accent: #ff8e8e;             /* Coral / Rosa suave */
    --card-bg: rgba(20, 20, 20, 0.6);
    --input-bg: #111111;
    --border-color: rgba(255, 255, 255, 0.1);
    
    --color-text-primary: #ffffff;
    --color-text-secondary: #888888;
    --color-background-secondary: rgba(255, 255, 255, 0.03);
    --color-border-tertiary: rgba(255, 255, 255, 0.1);
    --border-radius-md: 0.75rem;

    /* Tipografías */
    --font-heading: 'Nohemi', sans-serif;
    --font-body: 'Nohemi', sans-serif;
    --font-mono: 'SpaceMono', monospace;
    
    /* Transiciones fluidas de microinteracciones */
    --transition: all 0.5s cubic-bezier(0.16, 1, 0.3, 1);

    /* Desvanecimiento en Scroll (Edge Blur Overlays) */
    --mask-bg-top: #000000;
    --mask-bg-bottom: #000000;
}
```

---

## 3. Estructura de Secciones (Alternancia de Temas)

El flujo visual de la landing page intercala fondos oscuros y claros para potenciar la legibilidad y dinamismo:

| Sección | Identificador | Fondo | Estilo de Tema |
| :--- | :--- | :--- | :--- |
| **Hero** | `<header>` | Imagen de Fondo con superposición | Dark Gradient Overlay |
| **1. Impacto / Riesgo** | `#riesgo` | Negro Puro / Degradados sutiles | Oscuro Nivel 0 (`#000`) |
| **2. Comparativa Eficiencia** | `#comparativa` | Crema Cálido (`#f2efe8`) | Clase `.section-light` |
| **3. Programa Académico** | `#programa` | Negro Puro | Oscuro Nivel 0 |
| **4. Ecosistema de Herramientas** | `#herramientas` | Crema Cálido (`#f2efe8`) | Clase `.section-light` |
| **5. ¿Es esto para ti?** | `#audiencia` | Negro Profundo (`#050505`) | Oscuro Nivel 1 |
| **6. Calendario y Fechas** | `#agenda` | Crema Cálido (`#f2efe8`) | Clase `.section-light` |
| **7. Reconocimiento Académico** | `#enae-prestigio` | Negro Puro (`#000000`) | Oscuro Nivel 0 (Bottom-Right glow) |
| **Footer** | `<footer>` | Negro Puro | Oscuro Nivel 0 |

---

## 4. El Sistema de Inversión de Tema Claro (`.section-light`)

Para evitar tener que duplicar marcado o clases utilitarias, se desarrolló una estructura de cascada CSS basada en la clase contenedora `.section-light`. Esta invierte inteligentemente la apariencia de los componentes comunes dentro de ella:

```css
/* Inversión General de Textos */
.section-light h2,
.section-light h3,
.section-light h4,
.section-light span:not(.text-gradient):not(.num):not(.accordion-icon) {
    color: #0e0e0e; /* Negro de alta legibilidad, no puro */
}

.section-light p {
    color: #555555; /* Gris equilibrado para lectura continua */
}

/* Conservar acentos corporativos */
.section-light [style*="color: var(--accent-color)"],
.section-light [style*="color:var(--accent-color)"] {
    color: var(--accent-color) !important;
}

/* Bento Grid en Modo Claro */
.section-light .bento-card {
    background: #ffffff !important;
    border: 1px solid rgba(0, 0, 0, 0.06) !important;
    box-shadow: 0 4px 24px rgba(0, 0, 0, 0.06);
}

.section-light .bento-card::before {
    /* Glow de cursor adaptado a luz */
    background: radial-gradient(800px circle at var(--mouse-x, 0px) var(--mouse-y, 0px), rgba(168, 23, 48, 0.07), transparent 40%) !important;
}

.section-light .bento-card:hover {
    border-color: rgba(168, 23, 48, 0.3) !important;
    box-shadow: 0 16px 48px rgba(168, 23, 48, 0.1) !important;
}

/* Glass Cards en Modo Claro */
.section-light .glass {
    background: #ffffff !important;
    border-color: rgba(168, 23, 48, 0.2) !important;
    box-shadow: 0 20px 60px rgba(0, 0, 0, 0.1) !important;
}

/* Textos con Degradados en Modo Claro */
.section-light .text-gradient {
    background: linear-gradient(to bottom, #1a1a1a 0%, #555 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
}
```

---

## 5. Componentes Clave de UI y Adaptabilidad Responsiva

Para mantener la estética premium en móviles, la landing page evita layouts inline fijos y utiliza clases CSS responsivas:

### A. Barra de Navegación y Menú Hamburguesa
- **Trigger Interactivo (`.nav-toggle`):** Compuesto por tres barras horizontales (`.hamburger-bar`) que giran y cambian de forma fluida. Se activa de forma táctil en móviles.
- **Contenedor Móvil (`.nav-links.active`):** En anchos menores de `768px`, la barra de navegación se oculta para convertirse en un panel lateral interactivo con efecto de desenfoque de fondo (`backdrop-filter: blur(20px)`), activado suavemente mediante JS.

### B. Bento Grid Adaptable
- **Estructura base:** Rejilla de 12 columnas que aloja tarjetas de esquinas redondeadas (`border-radius: 1.5rem`) con efecto de iluminación de cursor radial (`::before` con coordenadas CSS mouse).
- **Apilamiento Móvil:** Mediante la regla `.bento-card { grid-column: span 12 !important; }` a nivel móvil, todas las tarjetas Bento que tengan distribuciones de columnas inline en escritorio (`span 6`, `span 7`, etc.) colapsan de manera segura y elegante a una columna, sin desbordarse lateralmente.

### C. Contenedor Hero Responsivo (`.hero-container`)
- **Desktop:** Grilla asimétrica balanceada con `grid-template-columns: 1.2fr 0.8fr` y una amplia separación de `6rem` que da aire y legibilidad al título y al formulario.
- **Móvil/Tablet (<1024px):** La clase `.hero-container` transiciona a un diseño vertical apilado de una sola columna (`grid-template-columns: 1fr`) centrando todo el contenido tipográfico de forma fluida.
- **Botoneras (`.hero-buttons`):** Agrupa los botones principales del Hero. Bajo `768px` cambia a un orden vertical (`flex-direction: column`) adaptando las llamadas a la acción al 100% del ancho del dispositivo para optimizar las pulsaciones táctiles.

### D. Acordeón de Programa (Efecto Timeline)
- **Estructura:** Títulos grandes (`2.2rem`) y números destacados que al desplegarse abren el contenido con una transición fluida.
- **Móvil:** La clase contenedora `.accordion-inner` conmuta de grilla de dos columnas a una sola columna (`grid-template-columns: 1fr !important`), reduciendo el tamaño del gatillo tipográfico a `1.4rem` para evitar líneas cortadas.

### E. Métrica Mini-Cards Adaptativa (`.metric-bento-card`)
- Las tarjetas contenedoras de métricas de tres columnas se adaptan dinámicamente mediante variables CSS (`var(--metric-cols)`). En pantallas menores de `600px`, la grilla cambia a `1fr` obligando a las tres métricas (ahorro, errores, escalabilidad) a apilarse con gran legibilidad tipográfica.

### F. Formulario de Registro con Sello FUNDAE
- **Fondo:** Tarjeta translúcida con difuminado de fondo (`backdrop-filter: blur(20px)`).
- **Badge FUNDAE:** Diseñado con un gradiente ámbar/dorado exclusivo (`rgba(212,166,0,0.1)`) y un borde fino a juego. El logo se estiliza dinámicamente mediante filtros CSS (`brightness(2) sepia(1) hue-rotate(5deg) saturate(2)`) para mantener un look dorado corporativo.
- **Mensaje de Urgencia:** Texto en dorado brillante bajo el badge: `⚠ PLAZAS LIMITADAS POR CONVOCATORIA`.

### G. Sección de Reconocimiento y Prestigio Académico (`#enae-prestigio`)
- **Estética de Alta Gama:** Estructura de tarjetas Bento con fondo ultra-oscuro translúcido y bordes imperceptibles (`rgba(255,255,255,0.04)`) que transmiten el prestigio institucional de ENAE.
- **Logos con Comportamiento Inteligente:** Diseñados para integrarse sutilmente en el fondo mediante un filtro de escala de grises y opacidad reducida (`filter: grayscale(1) opacity(0.85); transition: var(--transition);`). Al pasar el cursor, recuperan suavemente su color y opacidad total (`filter: none; opacity: 1`).
- **Estructura Responsiva:** En escritorio se despliegan en grilla horizontal de tres columnas (`span-4` cada tarjeta), colapsando automáticamente a una sola columna ordenada de forma vertical en pantallas de móviles y tabletas para preservar la legibilidad del texto de acreditación.

---

## 6. Calendario de Agenda Interactivo (Julio 2026)

La sección `#agenda` contiene un calendario mensual completamente interactivo que sustituye a las listas de fechas estáticas convencionales.

### Maquetación Responsiva (`.agenda-layout-grid`):
- **Desktop:** Grilla de dos columnas simétricas (`1fr 1fr`) con `4rem` de separación que permite ver el calendario al lado izquierdo y la tarjeta de detalle a la derecha.
- **Móvil/Tablet:** Se apila automáticamente en una columna (`1fr`) con espacio intermedio de `3rem` para una lectura vertical ágil de arriba (calendario) hacia abajo (panel de descripción del día).

### Diseño del Calendario:
- **Estructura:** Grilla de 7 columnas que muestra los días de la semana y la distribución de los días del mes de julio de 2026 de forma limpia.
- **Resaltado de Días Clave (Sesiones 15, 16, 22 y 23):**
  - Fondo de acento carmesí (`var(--accent-color)`).
  - Texto en blanco, subrayado discreto, y un efecto de elevación al pasar el cursor (`transform: scale(1.15)`).
- **Panel de Detalles:** Tarjeta lateral que se inicializa en estado vacío solicitando al usuario clicar un día. Al seleccionar un día del calendario, la información se despliega con una animación fluida de entrada (`opacity` y `translateY` controlado por `@keyframes calDetailIn`).
- **Interactividad Accesible:** Funciona mediante eventos de click estándar y es controlable con el teclado (`Enter` / `Espacio`) a través de un mapeo en JS autoejecutable.

---

## 7. Sistema de Desvanecimiento y Filtro Progresivo en Scroll (Viewport Edge Blur Overlays)

Para lograr un acabado de interfaz premium similar al de los sistemas operativos más refinados (como Apple macOS o Microsoft Windows Fluent Design), se implementó un sistema dinámico de desvanecimiento progresivo y desenfoque en los bordes del viewport.

### A. Elementos de Máscara (`.scroll-blur-mask`)
Se inyectan dinámicamente en el DOM dos contenedores fijos al inicio de la carga:
- `.scroll-blur-mask-top`: Ubicada en la parte superior del viewport (altura `140px`). Sits justo por debajo de la barra de navegación (`z-index: 90`).
- `.scroll-blur-mask-bottom`: Ubicada en la parte inferior del viewport (altura `110px`).

### B. Técnicas CSS Avanzadas (Progressive Blur Masking)
Para que el efecto no corte de manera abrupta, se combina un gradiente lineal de color de fondo con una máscara de opacidad CSS progresiva:
```css
.scroll-blur-mask {
    position: fixed;
    left: 0;
    width: 100%;
    z-index: 90;
    pointer-events: none; /* No interfiere con clics o interacciones del usuario */
    will-change: background-color, backdrop-filter;
    transition: background-color 0.3s ease; /* Transición suave al cruzar secciones */
}

.scroll-blur-mask-top {
    top: 0;
    background: linear-gradient(to bottom, var(--mask-bg-top) 30%, transparent 100%);
    backdrop-filter: blur(16px);
    -webkit-backdrop-filter: blur(16px);
    mask-image: linear-gradient(to bottom, black 25%, rgba(0, 0, 0, 0.65) 55%, transparent 100%);
}
```
Esto asegura que el desenfoque del fondo (`backdrop-filter`) se atenúe gradualmente hacia el centro de la pantalla, logrando una transición sumamente pulida y orgánica.

### C. Sincronización Dinámica de Color en JS (`initScrollEffects`)
Dado que la página alterna secciones oscuras y claras, las máscaras deben adaptar su color de fondo en tiempo real para evitar que se vean de un tono incorrecto cuando el usuario hace scroll.
1. **Caché de Dimensiones:** Para evitar el "layout thrashing" (recálculos pesados del navegador que afectan al rendimiento), se calculan y guardan en caché los límites (`offsetTop` y `offsetHeight`) de cada sección en la carga inicial, actualizándose únicamente al redimensionar la pantalla (`resize`).
2. **Resolución de Colores Transparentes:** Si una sección tiene fondo transparente, el script detecta inteligentemente si es una sección clara (clase `.section-light`) y le asigna el color correspondiente (`#f2efe8` para claro, `#000000` para oscuro).
3. **Muestreo de Coordenadas de Scroll:** Durante el scroll, se evalúa qué sección se encuentra en la posición correspondiente al centro de la máscara superior (`scrollY + 40px`) y de la máscara inferior (`scrollY + windowHeight - 40px`).
4. **Actualización CSS:** Se asignan los colores de fondo detectados a las variables `--mask-bg-top` y `--mask-bg-bottom` en el elemento raíz (`:root`), provocando una transición suave gracias a CSS transition.
5. **Optimización con `requestAnimationFrame`:** El scroll se procesa mediante un mecanismo de throttling con `requestAnimationFrame` para asegurar una tasa de refresco constante a 60 FPS o más sin sobrecargar la CPU.

---

## 8. Directrices para Nuevos Desarrollos e Integraciones

Si se añaden nuevas secciones o componentes, es crucial seguir estas reglas para asegurar la coherencia estética de la landing:
1. **Tipografías:** Usar estrictamente las fuentes cargadas en `fonts.css`. No usar fuentes de sistema para elementos destacados.
2. **Espaciados:** Los márgenes de las secciones deben ser generosos, idealmente `padding: 10rem 0` o `padding: 12rem 0` para un look moderno con suficiente aire. En móviles, estos deben escalar automáticamente a `padding: 6rem 0` a través de las media queries existentes.
3. **Imágenes y Logos:** Si se requiere incluir logos o assets, envolverlos en componentes que tengan un tema claro definido o usar filtros CSS para adaptarlos (por ejemplo, pasándolos a escala de grises o invirtiéndolos en fondos oscuros).
4. **Interactividad:** Todas las transiciones de hover deben utilizar la curva de aceleración configurada en `--transition`. Evitar transiciones directas instantáneas sin suavizado.
5. **Estilos Responsivos:** **Nunca utilizar anchos o grillas inline fijas** sin antes ligarlas a una variable CSS dinámica o una clase que soporte conmutación en las consultas de medios móviles de `index.css`.
