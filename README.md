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

## Publicar en GitHub Pages (gratis)

1. Sube tu repo a GitHub.
2. En **Settings → Pages → Build and deployment**, elige **Source: “GitHub
   Actions”**.
3. Ajusta `url` en `src/site.config.ts` a tu dominio de Pages
   (ej. `https://tu-usuario.github.io`).
4. Haz `push` a `main` (o `master`): el workflow
   [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml) compila y
   publica el sitio.

> Si publicas en `https://tu-usuario.github.io/tu-repo/` (no en la raíz),
> agrega también `base: '/tu-repo'` en `astro.config.mjs`.

## Stack

| Pieza | Qué es |
|---|---|
| [Astro 5](https://astro.build) | Generador de sitios estáticos (HTML plano, sin JS de framework en runtime). |
| Content Collections | Markdown tipado (`notas`, `resenas`, `projects`). |
| [@astrojs/sitemap](https://docs.astro.build/en/guides/integrations-guide/sitemap/) · [@astrojs/rss](https://docs.astro.build/en/guides/rss/) | Sitemap y feed RSS de las notas. |
| CSS plano | Un solo `src/styles/global.css` con variables de tema. Sin Tailwind. |
| GitHub Actions + Pages | Build y deploy automáticos. |

Requiere **Node.js 20+**.

## Licencia

[MIT](LICENSE) — úsalo, modifícalo y compártelo libremente.
