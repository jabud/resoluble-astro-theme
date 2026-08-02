# Resoluble — template de Astro

Plantilla minimalista para un **sitio personal** (notas, reseñas y proyectos),
construida con [Astro](https://astro.build) y pensada para publicarse **gratis**
en GitHub Pages. Sin base de datos, sin backend y **sin dependencias de pago**.

Estética de terminal: header con breadcrumb `~/ruta`, menú lateral colapsable,
tema claro/oscuro y contenido en Markdown. Toda la configuración de marca vive en
un solo archivo: [`src/site.config.ts`](src/site.config.ts).

> Hecho en español, pensado para que más gente pueda lanzar su sitio fácil.

---

## Empezar

Crea tu propio sitio a partir de esta plantilla:

```bash
npm create astro@latest -- --template tu-usuario/resoluble-astro-theme
```

O usa el botón **“Use this template”** en GitHub.

Luego, en tu carpeta:

```bash
npm install      # una sola vez
npm run dev      # servidor de desarrollo en http://localhost:4321
```

## Personalizar

1. **Edita [`src/site.config.ts`](src/site.config.ts)** — nombre, autor,
   descripción, URL, idioma/locale, redes sociales y menú. Es lo único que
   necesitas tocar para que el sitio sea tuyo.
2. **Escribe tu “contexto”** (página _about_) en
   [`src/pages/contexto.astro`](src/pages/contexto.astro).
3. **Reemplaza el contenido de ejemplo** en `src/content/` (ver abajo).
4. **Cambia el favicon** en `public/favicon.svg`.

## Escribir contenido

Todo el contenido son archivos Markdown en `src/content/`. Para crear algo nuevo,
copia un archivo existente y edita su bloque de _frontmatter_.

| Tipo | Carpeta | Frontmatter |
|---|---|---|
| Nota | `src/content/notas/` | `title`, `description`, `pubDate`, `tags`, `draft?` |
| Reseña | `src/content/resenas/` | igual que nota + `author?`, `rating?` (0–5) |
| Proyecto | `src/content/projects/` | `title`, `description`, `date`, `repo?`, `link?`, `tags`, `featured?` |

- El nombre del archivo es la URL (`mi-idea.md` → `/notas/mi-idea/`).
- `draft: true` deja una nota/reseña sin publicar.
- En un proyecto, el cuerpo del `.md` es su página de detalle; `repo` y `link`
  generan los botones de _código_ y _sitio_.

Los esquemas exactos están en
[`src/content.config.ts`](src/content.config.ts).

## Publicar el sitio (gratis)

Este template genera un sitio **estático** (la carpeta `dist/`), así que puedes
publicarlo gratis en cualquiera de estos servicios. Los tres se conectan a tu
repo de GitHub y **se actualizan solos** en cada `git push`. Antes de publicar,
ajusta `url` en `src/site.config.ts` a tu dominio final.

### Opción 1 — Cloudflare Pages

1. Entra a [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers &
   Pages** → **Create** → pestaña **Pages** → **Connect to Git**.
2. Elige tu repo. En **Build settings** (a veces bajo *Advanced settings*):
   - Framework preset: **Astro**
   - Build command: `npm run build`
   - Output directory: `dist`
3. **Save and Deploy**. Te da una URL tipo `tu-sitio.pages.dev`.

### Opción 2 — Netlify

1. Entra a [app.netlify.com](https://app.netlify.com) → **Add new site** →
   **Import an existing project** → **GitHub**.
2. Elige tu repo (detecta Astro automáticamente: `npm run build` → `dist`).
3. **Deploy**. Te da una URL tipo `tu-sitio.netlify.app`.

### Opción 3 — GitHub Pages

GitHub Pages necesita un pequeño *workflow* de GitHub Actions. Crea el archivo
`.github/workflows/deploy.yml` con esto:

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
  cancel-in-progress: false
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: withastro/action@v3
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

Luego, en tu repo: **Settings → Pages → Build and deployment → Source: “GitHub
Actions”**. Con eso, cada `push` a `main` publica el sitio.

> **Importante (sobre `base`):** si tu repo **no** se llama exactamente
> `tu-usuario.github.io` — es decir, tu sitio vive en una subcarpeta como
> `tu-usuario.github.io/mi-repo/` — los enlaces internos se romperían. En ese
> caso agrega `base: '/mi-repo'` en `astro.config.mjs`. Si es tu sitio de raíz
> (`tu-usuario.github.io`), no necesitas `base`. Cloudflare y Netlify no tienen
> este detalle porque sirven en la raíz.

## Stack

| Pieza | Qué es |
|---|---|
| [Astro 5](https://astro.build) | Generador de sitios estáticos (HTML plano, sin JS de framework en runtime). |
| Content Collections | Markdown tipado (`notas`, `resenas`, `projects`). |
| [@astrojs/sitemap](https://docs.astro.build/en/guides/integrations-guide/sitemap/) · [@astrojs/rss](https://docs.astro.build/en/guides/rss/) | Sitemap y feed RSS de las notas. |
| CSS plano | Un solo `src/styles/global.css` con variables de tema. Sin Tailwind. |
| Hosting estático | Se publica gratis en Cloudflare Pages, Netlify o GitHub Pages (ver arriba). |

Requiere **Node.js 20+**.

## Licencia

[MIT](LICENSE) — úsalo, modifícalo y compártelo libremente.
