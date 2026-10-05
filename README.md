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
`IBM Plex Sans`, `JetBrains Mono`).

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
| **Diseño** | Tema claro y oscuro, responsive, tipografía con fallback del sistema |
| **Experiencia** | Línea de tiempo con entradas reveladas al hacer scroll |
| **Portafolio** | Proyectos destacados con stack tecnológico y enlaces a código y demo |
| **IA aplicada** | Sección sobre uso de IA, agentes, skills y servidores MCP |
| **Habilidades** | Barras de nivel animadas al entrar en pantalla |
| **Educación e idiomas** | Formación y niveles MCER (Español nativo, Inglés A2) |
| **Contacto** | Correo con botón de copiado, GitHub, LinkedIn y teléfono |
| **Resumen en código** | Bloque tipo C# que se "escribe" al cargar la página |

### Accesibilidad

- Skip link ("Saltar al contenido principal") como primer elemento enfocable.
- HTML semántico: `<nav>`, `<main>`, `<section>`, `<footer>`, `<dl>`, `<ul>`.
- Etiquetas ARIA: `aria-label` en enlaces externos, `aria-pressed` en el
  conmutador de tema, `aria-live="polite"` al copiar el correo.
- Alternativas textuales a los iconos SVG (`aria-hidden` en los decorativos).
- `prefers-color-scheme` y `prefers-reduced-motion` respetados.
- Foco visible en todos los elementos interactivos.
- **Funciona sin JavaScript**: el contenido, las barras de habilidades y el
  resumen en código ya están en el HTML.

### Impresión y PDF

Hay una hoja de estilos `@media print` que genera una versión limpia en papel:
fondo blanco, sin navegación ni animaciones, sin cortes de página a mitad de
una tarjeta, y las URLs impresas junto a cada enlace.

Para generar el PDF desde el navegador: `Ctrl/Cmd + P` → *Guardar como PDF*.

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
  --accent:#0b6e8a;  /* color de marca */
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
| Bloque de código animado | el array `L` al final del `<script>` |
| Estadísticas (`7+`, `8`, `CI/CD`) | la etiqueta `<dl class="stats">` |
| Contenido de las skills | `data-v="90"` — ese número es el porcentaje; también está replicado en `style="width:90%"`, hay que cambiar los dos |
| Metadatos de SEO y el JSON-LD | las etiquetas `<meta>` y el `<script type="application/ld+json">` en el `<head>` |

> Las barras de habilidades declaran el ancho dos veces a propósito: en el
> atributo `data-v` (lo usa la animación) y en `style="width:…%"` (para que se
> vean completas si el visitante llega con JavaScript desactivado).

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

- [ ] Actualizar un puesto nuevo al comienzo de la línea de tiempo
- [ ] Agregar proyectos nuevos al portafolio
- [ ] Revisar los porcentajes de las habilidades
- [ ] Cambiar el correo si dejas de usar la dirección de Hotmail

---

## Licencia

Código y diseño de esta página: uso personal. Si reutilizas la estructura,
dale crédito a [Csar27](https://github.com/Csar27).
