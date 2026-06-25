# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This repository is a **Quarto RevealJS presentation extension** that replicates the CAIS (Center for Advanced Internet Studies) corporate design. The reference materials (PowerPoint master and manual) are in [`ppt and manual instruction/`](ppt%20and%20manual%20instruction/).

## Commands

```bash
# Render the template presentation
quarto render template.qmd

# Live preview with auto-reload
quarto preview template.qmd
```

## Extension Structure

```
_extensions/cais/
  _extension.yml      # format defaults (1600×900, no controls, Roboto font)
  theme.scss          # all slide layout CSS
  title-slide.html    # custom pandoc partial for the two-column title slide
img/
  Transparent.png     # CAIS logo (used in title slide + as logo: in YAML)
  background.png      # CAIS-blue icon-pattern background (title slide, closing slide)
  MKW-Logo-Transparent.png  # NRW Ministry logo (partner logos row)
template.qmd          # demo presentation showing all 14 layouts
```

## Critical: Image Path in SCSS

The compiled CSS lands at `template_files/libs/revealjs/dist/theme/*.css` — **5 directory levels deep**. Any `url()` reference in `theme.scss` must use `../../../../../img/` to resolve back to the project root, e.g.:

```scss
background-image: url('../../../../../img/background.png');
```

Always add a `background-color` fallback alongside any `background-image` in case the path fails.

## Title Slide Architecture

The title slide uses a **custom HTML partial** (`title-slide.html`) instead of Quarto's default "fancy" template, because the CAIS layout requires an explicit two-column flex structure:

- `.cais-title-left` (35%): white, logo + org info
- `.cais-title-right` (65%): CAIS-blue icon pattern background, title + subtitle

The template partial uses simple pandoc variables: `$title$`, `$subtitle$`, `$for(author)$$author$$endfor$`, `$for(institute)$$institute$$endfor$`, `$date$`. Do **not** use the fancy `$for(by-author)$` syntax — it would require a different YAML structure.

## Slide Layout Classes

Apply these classes to the level-2 heading (`## Heading {.class-name}`):

| Class | Layout | Key divs/structure |
|---|---|---|
| `.agenda-slide` | Left grey strip + numbered list | `ol` with auto-numbered items |
| `.section-slide` | Light grey, speech bubble | `.section-number` + `.section-bubble` |
| `.section-slide-dark` | Dark/photo bg, speech bubble | same + `data-background-image=` |
| `.text-image` | Text left, bleed image right | Quarto `.columns` (55%/45%) |
| `.image-text` | Bleed image left, text right | Quarto `.columns` (45%/55%) |
| `.text-slide-bg` | Headline + grey content box | `.lead` + `.content-box` |
| `.columns-slide` | 3-column icon grid | `.columns-area` > `.col-item` |
| `.overview-slide` | Blue header + 3-image grid | `.overview-grid` > `.overview-item` |
| `.quote-slide` | Light bg, speech-bubble quote | `.quote-bubble` > `blockquote` + `.quote-author` |
| `.quote-slide-dark` | Photo bg, speech-bubble quote | same + `data-background-image=` |
| `.closing-slide` | Same as title, left contact info | `.closing-left` |
| `.blank-slide` | No special styling | — |

Infographic stats use `.infographic` > `.infographic-cols` > `.infographic-item` with `[value]{.stat}`.

## Avoid Ghost Slides

Do **not** place any content (including HTML comments) between the YAML front matter and the first `##` heading — Quarto wraps that content into an empty anonymous slide. Put all comments after their heading, not before.

## CAIS Brand Guidelines

### Colors

| Name | Hex | Usage |
|---|---|---|
| CAIS Blau (primary) | `#009dd2` | Headings, accents, right-column backgrounds |
| CAIS Hellgrau | `#ebeae4` | Agenda strip, section slide bg, content boxes |
| CAIS Dunkelgrau | `#858585` | Body text, secondary elements |
| CAIS Rot | `#de7b73` | Icons, accent badges |
| Dark navy | `#0a1628` | Photo-backed slides |

Do not introduce new colors outside this palette.

### Typography

Roboto (Bold/Regular/Light), imported from Google Fonts. Font sizes and weights are baked into the SCSS — do not override them in individual slides.

### Assets

| File | Purpose |
|---|---|
| [`img/Transparent.png`](img/Transparent.png) | CAIS logo (transparent background) |
| [`img/background.png`](img/background.png) | CAIS-blue icon-pattern tile |
| [`img/MKW-Logo-Transparent.png`](img/MKW-Logo-Transparent.png) | NRW Ministry partner logo |
| [`ppt and manual instruction/CAIS-PowerPoint-Master.pptx`](<ppt%20and%20manual%20instruction/CAIS-PowerPoint-Master.pptx>) | Authoritative source for geometry and colours |
