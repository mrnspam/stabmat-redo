# Stabat Mater Jekyll Migration

This repository houses the migration of [stabatmater.info](https://stabatmater.info) from WordPress to a modular, data-driven Jekyll site that can be deployed to GitHub Pages.

## Migration plan

1. **Structuur opzetten** – Maak de standaard Jekyll-structuur met `_layouts`, `_includes`, `_data` en `assets/`. Definieer een `default`-layout die de hoofdomlijsting (html/head/body) bevat.
2. **Navigatie modulariseren** – Verplaats topbar, hoofdmenu en secundaire menu naar `_includes` (bijv. `topbar.html`, `primary-nav.html`, `secondary-nav.html`) en stuur hun inhoud via `_data/navigation.yml` zodat menu-items eenvoudig beheerd kunnen worden.
3. **Hero & banner** – Maak een include voor de banner met configureerbare achtergrondafbeelding en titels, gevoed vanuit front matter of `_data/home.yml`. Zorg dat achtergrondafbeeldingen in `assets/images/` staan en in CSS/SCSS worden gestyled.
4. **Hoofdcontent naar Markdown** – Zet de hoofdsecties (YouTube-embed, headings, paragrafen) om naar Markdown in `index.md`, gebruik Liquid-tags of shortcodes voor embeds.
5. **Sidebar als component** – Gebruik `_includes/sidebar.html` met data-gestuurde widgets (donatiebanner, zoekformulier, recente posts, navigatielinks). Zo kunnen later andere pagina’s dezelfde sidebar hergebruiken.
6. **Footer-widgets** – Modelleer de vierkoloms footer via `_includes/footer-widgets.html` en stuur inhoud via `_data/footer_widgets.yml` voor consistente styling.
7. **Styling scheiden** – Maak `assets/css/main.scss` waarin je basisvariabelen (kleuren, lettertypen) en component-styles definieert. Zet responsive gedrag (zoals verborgen menu’s op mobiel) in SCSS in plaats van inline styles.
8. **Contentmigratie** – Voor latere fases zorg je dat alle WordPress-inhoud (blogposts, componistenpagina’s) naar Markdown-bestanden in passende collecties gaat (`_posts`, `_composers`, etc.) met YAML-data voor metadata.

## Current status

- Een `default`-layout en kern-includes (topbar, navigatie, hero, sidebar, footer) vormen de basis van de homepage.
- De eerste versie van `index.md` bevat de belangrijkste secties van de homepagina, inclusief een YouTube-embed via `_includes/youtube.html`.
- `assets/css/main.scss` bevat de eerste opzet van typografie en component-styling die het uiterlijk van stabatmater.info benadert.
- Verdere iteraties zullen de navigatie en widgets voeden vanuit `_data`, extra collecties toevoegen en inhoud migreren uit WordPress.

## Handmatige media-bestanden

De volgende afbeeldingen horen bij de lay-out maar zijn wegens PR-beperkingen niet opgenomen in de repository. Voeg ze handmatig toe na het clonen. Bewaar ze allemaal in `assets/images/`.

| Bestand | Beschrijving | Plaats in de site |
| --- | --- | --- |
| `logo.png` | Transparant PNG-logo van *Stabat Mater Foundation* voor de primaire navigatiebalk. | Wordt geladen in `_includes/primary-nav.html` als merklogo linksboven. |
| `hero-banner.jpg` | Full-width hero-achtergrond met een klassiek muziekoptreden. | Ingezet door `_includes/hero.html` als standaard hero-achtergrond. |
| `hero-placeholder.jpg` | Alternatieve hero-afbeelding met een subtiel tekstuurpatroon voor pagina’s zonder specifieke banner. | Gebruik voor secundaire pagina’s via dezelfde hero-include. |
| `flag-nl.png` | Klein Nederlands vlaggetje voor taalkeuze. | Getoond in `_includes/topbar.html` naast de taalwissel. |
| `sidebar-donate.jpg` | Visuele banner met orkest voor de donatie-widget. | In `_includes/sidebar.html` als achtergrond voor de donatiecall-to-action. |
| `footer-youtube.jpg` | Thumbnail van een kooroptreden. | Onderste kolom “YouTube” in `_includes/footer.html`. |
| `footer-instagram.jpg` | Close-up foto uit een repetitie, afgestemd op Instagram-CTA. | Onderste kolom “Instagram” in `_includes/footer.html`. |
| `footer-book.jpg` | Boekcover van *Stabat Mater Dolorosa*. | Onderste kolom “The book” in `_includes/footer.html`. |

> **Let op:** zorg dat de bestandsnamen exact overeenkomen zodat de stijlen en includes zonder extra wijzigingen werken.

## Getting started

```bash
bundle install
bundle exec jekyll serve
```

De site is dan beschikbaar op <http://127.0.0.1:4000>. Gebruik `bundle exec jekyll build` om een productie-build te genereren.
