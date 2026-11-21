# IxDF Mendoza · Comunidad

Sitio de una sola página para la comunidad **IxDF Mendoza**, construido a partir del template de Figma *“IxDF Mendoza — Del Diseño a la Web con Claude”* (combinación **Taller + Neobrutalista**).

Es un único `index.html` con el CSS y el JavaScript adentro: sin frameworks, sin librerías y sin instalación. Las fuentes se cargan desde Google Fonts.

## Cómo verlo

Abrí `index.html` directamente en el navegador, o servilo con cualquier servidor estático:

```bash
npx serve .
# o
python -m http.server
```

También se puede publicar tal cual en GitHub Pages (Settings → Pages → rama `main`, carpeta raíz).

## Qué tiene la página

| Sección | Contenido |
|---|---|
| **Nav superior** | Logo, 4 links y botón *Call to action*. En tablet y celular se convierte en menú hamburguesa. |
| **Hero** | Chip, título grande, bajada, botón principal + link secundario y una columna de 3 datos. |
| **Encuentros** | Encabezado de sección y 3 cards con imagen. “Ver detalle” abre el modal de cada encuentro. |
| **Próximos temas** | Gráfico de torta “explotada” en SVG con su leyenda, y un banner de encuesta. |
| **Footer** | Marca y links (Instagram, LinkedIn, Sitio). |
| **Modal** | Dos variantes: con **carrusel de fotos** o con **banner**. Incluye la actividad y una grilla de “Links y materiales”. |

## Editar el contenido

Todo el contenido está en el objeto `SITIO`, al principio del `<script>`. No hace falta tocar el HTML.

```js
const SITIO = {
  logo: "Logo",
  nav: [ { texto: "Sección 1", href: "#inicio" }, … ],
  hero: { chip, titulo, bajada, principal, secundario, datos: [ … ] },
  secciones: { encuentros: { … }, temas: { … } },
  encuentros: [ … ],   // cards + modales
  temas: [ … ],        // gráfico + leyenda
  encuesta: { … },
  footer: { … },
};
```

### Encuentros (cards y modales)

```js
{
  id: "encuentro-1",
  tag: "Tag", titulo: "Título",
  descripcion: "Texto de la card",
  portada: { src: "assets/portada.jpg", alt: "Descripción de la imagen" },
  modal: "fotos",            // "fotos" → carrusel · "banner" → imagen grande
  detalle: "Texto de la actividad",
  fotos: [ { src: "assets/foto-1.jpg", alt: "…", ancho: 378 }, … ],  // solo para "fotos"
  banner: { src: "assets/banner.jpg", alt: "…" },                    // solo para "banner"
  links: [
    { texto: "Link a un recurso", href: "https://…" },
    { texto: "Grabación", pendiente: true },   // se ve en gris y no se puede tocar
  ],
}
```

- Si una imagen no tiene `src`, se dibuja un marco vacío del mismo tamaño (con su texto alternativo).
- Las portadas se muestran con `object-fit: cover` a 250 px de alto.
- Las fotos del modal van en un carrusel horizontal, todas a 260 px de alto, con su ancho natural y sin recortar.

### Temas (gráfico)

```js
temas: [
  { tema: "Accesibilidad", ideas: 22, color: 1 },   // color: 1, 2 o 3 → --color-chart-N
  …
]
```

La torta y la leyenda se generan desde estos datos. El porcentaje se calcula solo.

## Design system

Todas las variables de Figma están pasadas a variables CSS con el mismo nombre (`color/bg` → `--color-bg`, `radius/lg` → `--radius-lg`, `border/width` → `--border-width`, `shadow/offset` → `--shadow-offset`, etc.).

Las dos colecciones de Figma son atributos de `<html>`:

| Colección | Atributo | Modos |
|---|---|---|
| **Paleta** (colores) | `data-paleta` | `Taller` · `Bosque` · `Noche` · `Mono` |
| **Estilo** (fuentes, radios, bordes, sombras) | `data-estilo` | `Neobrutalista` · `Minimal` · `Techy` · `Notion` |

Por defecto: `data-paleta="Taller" data-estilo="Neobrutalista"`. Para probar otra combinación sin editar el archivo, usá la URL:

```
index.html?paleta=Noche&estilo=Techy
```

El fondo depende del estilo: cuadrícula inclinada −12° en **Neobrutalista**, degradé en **Techy** y liso en **Minimal** y **Notion**.

### Estilos de texto

| Clase | Tamaño / peso | Interlineado | Tracking |
|---|---|---|---|
| `.display` | 56 / Bold | 1.1 | −2% |
| `.titulo` | 32 / Bold | 1.2 | −1% |
| `.subtitulo` | 24 / Medium | 1.3 | −0.5% |
| `.cuerpo` | 18 / Regular | 1.5 | 0 |
| `.etiqueta` | 14 / Medium | 1.4 | +4% |

### Componentes

Cada componente de Figma es una función reutilizable en el script: `Boton` (primario, secundario, fantasma × grande, chico), `Chip`, `NavSuperior`, `EncabezadoDeSeccion`, `CardConImagen`, `ItemDeLeyenda`, `FilaDeLink`, `Footer`, `Modal` y `TortaExplotada`.

## Responsive

- **Desktop (≥ 1024 px):** las tres secciones tienen el mismo alto, como en Figma.
- **Tablet:** hero apilado, cards en 2 columnas y menú hamburguesa.
- **Celular (< 720 px):** todo en una columna y la leyenda debajo del gráfico.

## Accesibilidad

- HTML semántico (`header`, `nav`, `main`, `section`, `footer`, `dialog`) y link para saltar al contenido.
- Texto alternativo en todas las imágenes; el gráfico tiene una descripción con sus datos.
- Foco visible en todos los elementos interactivos.
- El modal usa `<dialog>`: se cierra con la cruz, con Escape o tocando afuera, y devuelve el foco al botón que lo abrió.
- Respeta `prefers-reduced-motion`.
- Contraste AA en las paletas Taller, Bosque y Noche. En **Mono**, el texto secundario (`#787774` sobre blanco) da 4.48:1, apenas debajo de AA para textos chicos.
