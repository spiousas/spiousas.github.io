# spiousas.github.io

Sitio personal de Ignacio Spiousas, hecho con [Quarto](https://quarto.org) y
publicado en GitHub Pages: <https://spiousas.github.io>

El sitio es bilingüe: **español en la raíz** (`/`) e **inglés en `/en/`**, con un
selector de idioma en la barra de navegación.

## Estructura

```
├── _quarto.yml          # configuración compartida (tema, formato, bibliografía)
├── _quarto-es.yml       # perfil español: navbar, pie de página, páginas a renderizar
├── _quarto-en.yml       # perfil inglés: ídem, renderiza solo en/
├── index.qmd            # inicio (ES)          en/index.qmd       (EN)
├── investigacion.qmd    # investigación        en/research.qmd
├── docencia.qmd         # docencia             en/teaching.qmd
├── blog.qmd             # listado del blog     en/blog.qmd
├── cv.qmd               # CV                   en/cv.qmd
├── posts/               # entradas del blog    en/posts/
│   └── bienvenida/index.qmd
├── assets/
│   ├── theme-base.scss  # tipografía y layout comunes
│   ├── theme-light.scss # paleta clara
│   ├── theme-dark.scss  # paleta oscura
│   └── publications.bib # publicaciones (se listan solas en investigación)
├── images/              # foto de perfil, favicon
├── files/               # PDFs descargables (CV, etc.)
└── .github/workflows/publish.yml
```

## Trabajar localmente

Requiere [Quarto](https://quarto.org/docs/get-started/) (en macOS:
`brew install --cask quarto`).

```bash
# Previsualizar el sitio en español (perfil por defecto), con recarga automática
quarto preview

# Previsualizar el sitio en inglés
quarto preview --profile en

# Construir el sitio completo tal como lo hace GitHub Actions
quarto render --profile es
quarto render --profile en --no-clean
```

El resultado queda en `_site/` (ignorado por git: lo construye GitHub Actions).

## Escribir una entrada del blog

1. Crear una carpeta en `posts/` (una por entrada, así las imágenes viven al lado):

   ```bash
   mkdir -p posts/mi-entrada
   ```

2. Crear `posts/mi-entrada/index.qmd` con este encabezado:

   ```yaml
   ---
   title: "Título de la entrada"
   description: >
     Un resumen de una o dos líneas; aparece en el listado del blog.
   date: 2026-10-20
   categories: [rstats, estadística]
   image: thumbnail.png      # opcional
   ---
   ```

3. Escribir. Para que el código se ejecute, usar bloques con llaves:

   ````markdown
   ```{r}
   library(tidyverse)
   mtcars |> ggplot(aes(wt, mpg)) + geom_point()
   ```
   ````

   Los bloques sin llaves (` ```r `) se muestran pero no se ejecutan.

Las opciones comunes a todas las entradas están en `posts/_metadata.yml`.

### `freeze`: por qué el servidor no necesita R

Las entradas usan `freeze: true`: los resultados del código se guardan en
`_freeze/` y se versionan en git. GitHub Actions **no ejecuta R ni Python**, solo
usa esos resultados congelados. Por eso, cuando escribas o modifiques una entrada
con código, hay que renderizar localmente y commitear `_freeze/`:

```bash
quarto render posts/mi-entrada/index.qmd
git add _freeze posts/mi-entrada
git commit -m "Nueva entrada"
```

## Publicación

Cada push a `main` dispara `.github/workflows/publish.yml`, que renderiza los dos
idiomas y los publica en GitHub Pages. El origen de Pages está configurado como
**GitHub Actions** (no como rama).

## Agregar publicaciones

Pegá las entradas BibTeX en `assets/publications.bib`. La sección
"Publicaciones" de `investigacion.qmd` y `en/research.qmd` las lista
automáticamente (`nocite: @*`). Para cambiar el estilo de cita, agregá
`csl: assets/apa.csl` en `_quarto.yml` con el archivo CSL que prefieras
(<https://www.zotero.org/styles>).
