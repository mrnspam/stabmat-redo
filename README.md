# Stabat Mater Jekyll Migration

This repository contains the migration of [stabatmater.info](https://stabatmater.info) from WordPress to a modular, data-driven Jekyll site that can be deployed to GitHub Pages.

## Current migration snapshot

- A `default` layout renders the shared shell (header, hero area, sidebar, footer and language switcher) and wires in the existing includes.
- `index.md` already mirrors the key sections of the WordPress homepage, including a reusable YouTube include for featured videos.
- `assets/css/main.scss` provides the first pass of typography, layout helpers and widget styling to match the current visual identity.
- Navigation, sidebar widgets and footer blocks are still populated with hard-coded HTML. Converting them to data-driven includes is the next big milestone.

## Proposed content architecture

### Collections and structured content

| Collection | Location | Purpose | Example front matter |
| --- | --- | --- | --- |
| Blog posts | `_posts/` | Preserve the chronological blog feed. | `title`, `date`, `categories`, `excerpt`, `author` |
| Composer profiles | `_composers/` | One Markdown file per composer, used for detail pages and to generate indexes. | `title`, `slug`, `country`, `era`, `birth_year`, `death_year`, `duration`, `sources` |
| Translation pages | `_translations/` | Store each language variant of the poem with metadata for language selection. | `title`, `language_code`, `native_name`, `translator`, `source_url`, `published` |
| Static interior pages | `pages/` (built via `defaults`) | WordPress pages such as “About”, “Donate”, “Contact”. Front matter can drive hero images, sidebar widgets and breadcrumbs. | `title`, `permalink`, `hero`, `sidebar_widgets` |

> Configure the additional collections in `_config.yml`, optionally pointing `collections_dir: content` so the Markdown files live under `content/` while still building to clean permalinks.

### Shared data sources (`_data/`)

| File | Drives | Notes |
| --- | --- | --- |
| `navigation/primary.yml` and `navigation/secondary.yml` | Main menus | Supports nesting for dropdowns, language-aware labels and feature flags for temporary items. |
| `navigation/topbar.yml` | Social links, donate links and language switcher | Replace the static list in `topbar.html`. |
| `sidebar/widgets.yml` | Sidebar ordering | Allows per-page overrides by referencing widget IDs. |
| `footer/widgets.yml` | Footer CTAs | Stores titles, copy, image filenames and external URLs so that footer cards stay consistent across the site. |
| `site/social.yml`, `site/contact.yml` | Global metadata | Shared by templates, JSON-LD, feeds and forms. |
| `translations/languages.yml` | Language switcher | List of language codes, names and slug prefixes for building localized routes. |

### Dynamic listings and relationships

- **Composers:** generate alphabetical, chronological, duration-based and country-based pages directly from `_composers/` metadata. Liquid filters can assemble grouped collections (`group_by` on country, `sort` on `birth_year`, etc.).
- **Recent posts:** populate the sidebar widget by querying the latest `_posts` entries instead of hard-coded placeholders.
- **Translations:** drive the translations overview page and dropdowns from `_translations/` plus `_data/translations/languages.yml`. Include `available: true/false` flags so incomplete translations can be hidden until ready.
- **Hero banners and sidebars:** let Markdown front matter reference entries in `_data/sidebar/widgets.yml` or provide overrides (`sidebar_widgets: [donate, search, composers]`).
- **Navigation and footer:** iterate over `_data/navigation/*.yml` and `_data/footer/widgets.yml` in the includes. This keeps multi-language labels and future additions (e.g. events, shop) centralized.
- **Media galleries & discography:** once composers include an array of recordings, expose them via nested data files (`_data/recordings/<composer>.yml`) to avoid bloated front matter.

### Suggested folder layout

```text
.
├── _config.yml
├── _data/
│   ├── navigation/
│   │   ├── primary.yml
│   │   └── secondary.yml
│   ├── sidebar/widgets.yml
│   ├── footer/widgets.yml
│   ├── site/
│   │   ├── social.yml
│   │   └── contact.yml
│   └── translations/languages.yml
├── content/
│   ├── composers/  → becomes `_composers/`
│   ├── translations/  → becomes `_translations/`
│   └── pages/
├── _includes/
├── _layouts/
├── _posts/
└── assets/
    ├── images/
    ├── downloads/
    └── css/
```

Keeping curated content inside `content/` helps editors focus on Markdown files, while `_data/` centralizes structured lists that power menus, widgets and taxonomy pages.

## Roadmap / next steps

1. **Model new collections** – Define `_composers` and `_translations` in `_config.yml`, add representative Markdown files and ensure permalinks match the current WordPress slugs.
2. **Refactor includes to use `_data`** – Update `topbar.html`, `secondary-nav.html`, `sidebar.html` and `footer.html` to consume the proposed data files. This will automatically reflect new links, posts or CTAs without template edits.
3. **Build listing templates** – Create index pages for composers (alphabetical, chronological, by country, by duration) using Liquid loops over `_composers`. Mirror this approach for translation overviews and archive pages.
4. **Migrate historical content** – Start importing high-value composers and translations, verifying metadata completeness. In parallel, backfill blog posts to `_posts/` with accurate categories and excerpts.
5. **Internationalization hooks** – Use `_data/translations/languages.yml` to render language switchers, set up localized permalinks (`/nl/…`) and plan how shared includes adapt per language.
6. **Enhance search & discovery** – Evaluate Lunr.js or Algolia for site search across composers, translations and posts. Ensure metadata fields (era, instrumentation, difficulty) are exposed for filtering.
7. **QA and parity checks** – Compare output with the existing WordPress site page-by-page, capturing gaps (forms, embeds, downloads) before decommissioning the old platform.

## Manual media files

The following images belong to the layout but are excluded from version control. Add them manually to `assets/images/` after cloning the repository.

| File | Description | Used by |
| --- | --- | --- |
| `logo.png` | Transparent PNG logo for the primary navigation bar. | `_includes/primary-nav.html` |
| `hero-banner.jpg` | Default hero background used on the homepage. | `_includes/hero.html` |
| `hero-placeholder.jpg` | Secondary hero background for pages without a dedicated banner. | `_includes/hero.html` |
| `flag-nl.png` | Dutch flag used for the language switcher. | `_includes/topbar.html` |
| `sidebar-donate.jpg` | Banner artwork for the donation widget. | `_includes/sidebar.html` |
| `footer-youtube.jpg` | CTA image for the YouTube footer widget. | `_includes/footer.html` |
| `footer-instagram.jpg` | CTA image for the Instagram footer widget. | `_includes/footer.html` |
| `footer-book.jpg` | Artwork for the Stabat Mater book CTA. | `_includes/footer.html` |

## Local development

```bash
bundle install
bundle exec jekyll serve
```

The site will be available at <http://127.0.0.1:4000>. Use `bundle exec jekyll build` to produce a static build for deployment.
