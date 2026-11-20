# Hoja de vida — César Torres Guerrero

Sitio web personal que funciona como hoja de vida en un **único archivo HTML**.
Sin build, sin dependencias, sin framework: se abre el archivo y ya está.

- **Archivo principal:** [`cesar-torres-cv.html`](cesar-torres-cv.html)
- **Publicación:** <https://csar27.github.io/cv-cesar-torres/>
- **Contacto:** c-sar-torres@hotmail.com · Barranquilla, Colombia

---

## Por qué un solo archivo

Todo vive dentro de `cesar-torres-cv.html`: el HTML, el CSS en un bloque
`<style>` y el JavaScript en un bloque `<script>`. Eso tiene tres ventajas
prácticas:

1. **Se abre con doble clic**, sin servidor ni `npm install`.
2. **Se despliega en cualquier hosting estático** sin configuración.
3. **Se archiva y se respalda** como un solo documento.

El único recurso externo son las fuentes de Google Fonts (`Bricolage Grotesque`,
`Instrument Serif`, `IBM Plex Sans`, `JetBrains Mono`).

---

## Verlo en local

Opción más simple:

```bash
# macOS
open cesar-torres-cv.html

# Windows
start cesar-torres-cv.html

# Linux
xdg-open cesar-torres-cv.html
```

Si prefieres servirlo por HTTP (por ejemplo para probar el modo oscuro del
sistema o las herramientas de SEO):

```bash
npx serve .
# o
python3 -m http.server 8000
```

Luego abre <http://localhost:8000/cesar-torres-cv.html>.

---

## Qué incluye

| Área | Detalle |
| --- | --- |
| **Diseño** | Estilo editorial "informe anual": cuadrícula estricta, reglas finas, secciones numeradas 01–07, etiquetas mono en mayúsculas, esquinas rectas, numerales serif (Instrument Serif). Tema claro/oscuro, responsive |
| **Idiomas** | Conmutador ES ⇄ EN en la barra de navegación; detección por navegador, persistencia en `localStorage` y parámetro `?lang=` compartible |
| **Experiencia** | Filas con columna de fechas + contenido, separadas por reglas finas (sin línea de tiempo) |
| **Portafolio** | Filas numeradas con hover de color + flecha; enlaces a código y demo |
| **IA aplicada** | Sección sobre uso de IA, agentes, skills y servidores MCP |
| **Habilidades** | Barras de progreso difuminadas (gradiente con borde suave) + palabra: Experto / Avanzado / Intermedio |
| **Educación e idiomas** | Formación y niveles MCER (Español nativo, Inglés A2) |
| **Contacto** | Correo con botón de copiado, GitHub, LinkedIn y teléfono |
| **Resumen en código** | Bloque tipo C# que se "escribe" al cargar la página (con respeto a `prefers-reduced-motion`) |

### Accesibilidad

- Skip link ("Saltar al contenido principal") como primer elemento enfocable.
- HTML semántico: `<nav>`, `<main>`, `<section>`, `<footer>`, `<dl>`, `<ul>`.
- Etiquetas ARIA: `aria-label` en enlaces externos, `aria-pressed` en el
  conmutador de tema, `aria-live="polite"` al copiar el correo.
- Alternativas textuales a los iconos SVG (`aria-hidden` en los decorativos).
- `prefers-color-scheme` y `prefers-reduced-motion` respetados.
- Foco visible en todos los elementos interactivos.
- **Funciona sin JavaScript**: todo el contenido, incluido el panel de datos y
  las habilidades, está en el HTML; el español es el idioma por defecto.

### Impresión y PDF

Hay una hoja de estilos `@media print` que genera una versión limpia en papel:
fondo blanco, sin navegación ni animaciones, sin cortes de página a mitad de
una tarjeta, y las URLs impresas junto a cada enlace.

Para generar el PDF desde el navegador: `Ctrl/Cmd + P` → *Guardar como PDF*.

---

## Internacionalización (ES / EN)

El sitio es bilingüe sin cambiar de archivo ni de ruta.

### Cómo funciona

- El HTML contiene el texto en **español** directamente (es el idioma por
  defecto y el que se ve sin JavaScript).
- Los elementos traducibles llevan el atributo `data-i18n="clave"` (para
  `textContent`) o `data-i18n-aria="clave"` (para `aria-label`).
- El JavaScript define dos diccionarios, `ES` y `EN`, con las mismas claves.
- Al cambiar de idioma se recorre el DOM, se sustituye el texto y se
  actualizan `<title>`, meta description, og:locale, og:description y el
  JSON-LD.

### Detección y persistencia

Prioridad al cargar la página:

1. Parámetro de URL `?lang=en` o `?lang=es`.
2. `localStorage['cv-lang']`.
3. Idioma del navegador (`navigator.language`).
4. Español por defecto.

Al pulsar el conmutador se guarda en `localStorage` y se escribe `?lang=` en
la URL con `history.replaceState`, así el enlace compartido ya lleva el idioma.

Compartir en inglés: <https://csar27.github.io/cv-cesar-torres/?lang=en>

### Añadir o modificar traducciones

1. Añade el atributo `data-i18n="mi.clave"` (o `data-i18n-aria`) al elemento.
2. Añade `'mi.clave':'Texto en español'` al diccionario `ES`.
3. Añade `'mi.clave':'English text'` al diccionario `EN`.
4. El script de verificación comprueba que ambos diccionarios tengan las mismas
   claves y que no haya valores vacíos.

Los nombres propios, tecnologías y certificaciones oficiales **no se
traducen** (`.NET`, `Azure`, `SCRUM`, nombres de empresas, etc.).

---

## Cómo personalizarlo

### 1. Colores y tema

Los colores están como variables CSS al inicio del `<style>`:

```css
:root{
  --bg:#f4f6f8;      /* fondo de la página */
  --surface:#fff;    /* tarjetas */
  --ink:#0f1c2a;     /* texto principal */
  --muted:#55636f;   /* texto secundario */
  --line:#d8dee4;    /* bordes */
  --accent:#0d9488;  /* color de marca */
  --sun:#d9930d;     /* acento secundario */
  --code:#0d1b26;    /* fondo del bloque de código */
}
```

El tema oscuro está duplicado en tres sitios. Si cambias un color, actualiza
**las tres reglas** para que no se desincronicen:

```css
:root{ ... }
@media(prefers-color-scheme:dark){:root:not([data-theme="light"]){ ... }}
:root[data-theme="dark"]{ ... }
```

### 2. Contenido

| Para cambiar | Busca |
| --- | --- |
| Nombre, rol y resumen | `.role` y `.lead` dentro de `<header>` |
| Enlaces de contacto | `c-sar-torres@hotmail.com`, `github.com/Csar27`, el `tel:` |
| Bloque de código animado | el array `L` al final del `<script>` y el `<pre id="code">` en el hero |
| Estadísticas (`7+`, `8`, `CI/CD`) | la etiqueta `<dl class="stats">` |
| Experiencia laboral | los `<article class="job">` |
| Portafolio | los `<a class="prow link">` / `<div class="prow item">` |
| Habilidades | los `<li>` con `<b class="dots">` y `data-i18n="lv.*"` |
| Educación e idiomas | las secciones `#educacion` e `#idiomas` |
| Metadatos de SEO y el JSON-LD | las etiquetas `<meta>` y el `<script type="application/ld+json">` en el `<head>` |

Las habilidades usan una barra de progreso difuminada (gradiente con borde suave) + palabra: Experto / Avanzado / Intermedio.

### 3. Imagen de perfil

El archivo no incluye foto. Para añadirla, usa un `<img>` dentro de `<header>`:

```html
<img src="foto.jpg" width="160" height="160" alt="Retrato de César Torres Guerrero">
```

En `README.md` y en el sitio es buena idea añadir una versión con foto y otra
sin foto, porque en algunas regiones el sesgo de foto sigue pesando en el
proceso de selección.

---

## Publicarlo

Como el repositorio ya está en GitHub, la vía más directa es **GitHub Pages**:

1. Ve a **Settings → Pages** en el repositorio.
2. En *Source* elige **Deploy from a branch** → rama `main`, carpeta `/ (root)`.
3. Se publica en <https://csar27.github.io/cv-cesar-torres/>.

Cualquiera de estas alternativas también funciona sin cambiar el archivo:

- **Netlify**: arrastra la carpeta al editor; genera URL y HTTPS automático.
- **Vercel**: `npx vercel --prod`.
- **Cloudflare Pages**: conecta el repositorio y deja el directorio en `/`.

---

## Accesibilidad del enlace

Desde cualquier sitio se puede incrustar en un iframe con `width="100%"` y un
`height` razonable. Recuerda añadir `title` al iframe, que es el único
contenido que algunos lectores de pantalla usan para identificar el marco:

```html
<iframe src="https://csar27.github.io/cv-cesar-torres/"
        style="border:0;width:100%;height:900px"
        title="Hoja de vida de César Torres Guerrero"></iframe>
```

---

## Compatibilidad

Navegadores actuales (Chrome, Edge, Firefox, Safari) en escritorio y móvil.
El sitio no usa librerías externas, así que no hay dependencias que
actualizar ni vulnerabilidades de terceros que rastrear.

---

## Mantenimiento

Este es un CV, no una aplicación. El mantenimiento real consiste en
actualizar el contenido. Revisa periódicamente:

- [ ] Actualizar un puesto nuevo al comienzo de la experiencia
- [ ] Agregar proyectos nuevos al portafolio
- [ ] Revisar los niveles de las habilidades (puntos + palabra)
- [ ] Mantener sincronizados los diccionarios ES y EN al añadir texto
- [ ] Cambiar el correo si dejas de usar la dirección de Hotmail
- [ ] Actualizar metadatos de SEO y JSON-LD si cambia el rol o la ubicación

---

## Licencia

Código y diseño de esta página: uso personal. Si reutilizas la estructura,
dale crédito a [Csar27](https://github.com/Csar27).
path">
D:\projects\cv-cesar-torres\README.md