# Seer Markdown CSS

This repository contains the Markdown stylesheets bundled with
[Seer](https://github.com/ccseer/Seer). You can also use any local CSS file as
a custom Markdown theme.

## Add a Custom Theme

1. Create a `.css` file anywhere on your computer.
2. Open Seer **Settings > Type** and select the **Web** viewer.
3. Set **Markdown theme:** to **Custom**.
4. Enter the full CSS file path in the **Custom CSS path:** field.
5. Apply the settings, then reopen the Markdown file.

Seer remembers the path when you temporarily select another built-in theme,
but it only applies the file while **Custom** is selected.

## How Seer Loads CSS

Seer reads the selected file and embeds it in the generated Markdown preview.
The stylesheet therefore needs to be self-contained:

- Define both `html` and `body` backgrounds so the whole preview matches your
  theme.
- Prefer installed system fonts. Relative `url(...)` paths do not resolve from
  the CSS file because the preview HTML is generated in a temporary directory.
- Do not rely on JavaScript, browser extensions, or an external site being
  available.
- Seer's preview helper styles load after your custom stylesheet. Use a more
  specific selector when you need to override them.

The normal Custom template includes the following useful selectors:

```text
:root[data-darkmode="light"]
:root[data-darkmode="dark"]
body
#markdown-body.markdown-body
pre, code, blockquote, table, img
```

Because Seer's helper stylesheet (`md_chrome.css`) loads after custom styles
and also declares `:root[data-darkmode="..."]` (specificity `(0, 2, 0)`), overriding
its `--seer-*` tokens requires `!important` or a higher-specificity selector.

## Preview DOM Hooks

The preview adds these classes and ids around the rendered document. Target
them from a custom stylesheet when you need to restyle Seer's own chrome.

| Hook | Present when |
|---|---|
| `body.seer-reading-width-880` / `-1200` / `-full` | Reading width setting |
| `body.seer-table-zebra` | Table zebra stripes (on by default) |
| `body.seer-code-lines`, `body.seer-code-wrap` | Code line numbers / soft wrap |
| `body.seer-code-frame-mac` / `-win` | Code frame style |
| `html.seer-scrolling` | While a programmatic scroll runs |
| `.seer-code-block` | Wrapper around every fenced code block |
| `.seer-code-line`, `.seer-line-num` | A code line and its number cell |
| `.copy-code-btn` | Copy button injected into code blocks and diagrams |
| `.seer-heading-toggle`, `.seer-collapsed-content`, `.seer-expanding-content` | Collapsible headings and reveal animation |
| `.mermaid-block` | Host for a rendered Mermaid diagram |
| `.markdown-alert` + `.markdown-alert-{note,tip,important,warning,caution}` | `> [!NOTE]` style callouts |
| `#toc-container`, `#toc-title`, `#toc-content` | Table of contents panel |
| `.seer-inline-toc`, `.seer-inline-toc-title` | Inline `[TOC]` block rendered inside `#markdown-body` |
| `#seer-lightbox` (+ `.visible`) | Image lightbox, created on first click |
| `#seer-link-tooltip` (+ `.visible`, `.seer-tooltip-text`) | Multi-line hover tooltip for links and images |
| `.seer-target-active` | Target of an in-page anchor, alongside `:target` |

## Minimal Theme

Use this as a starting point. A custom stylesheet replaces Seer's normal
Markdown document stylesheet, so include the basic document rules you need.

```css
:root {
    --page-bg: #f6f3ed;
    --page-text: #252321;
    --muted-text: #6d675f;
    --accent: #006d77;
    --border: #d8d1c7;
    --code-bg: #eee9e1;
}

html,
body {
    min-height: 100%;
    background: var(--page-bg);
    color: var(--page-text);
}

body {
    margin: 0;
    font: 16px/1.7 "Segoe UI", sans-serif;
}

#markdown-body.markdown-body {
    max-width: 900px;
    margin: 0 auto;
    padding: 32px 48px 56px;
}

.markdown-body a {
    color: var(--accent);
}

.markdown-body h1,
.markdown-body h2 {
    border-bottom: 1px solid var(--border);
    padding-bottom: 0.3em;
}

.markdown-body blockquote {
    margin: 1em 0;
    padding: 0 1em;
    color: var(--muted-text);
    border-left: 0.25em solid var(--border);
}

.markdown-body pre,
.markdown-body code {
    background: var(--code-bg);
    border-radius: 4px;
}

.markdown-body pre {
    overflow: auto;
    padding: 16px;
}

```

For a dark stylesheet, override the same variables inside
`:root[data-darkmode="dark"]`, or make the stylesheet dark by default. Test
headings, links, long code blocks, tables, images, and mathematics in both the
Seer light and dark application themes before sharing a theme.

## Custom Theme Example

See [`custom-example.css`](custom-example.css) for a comprehensive,
production-ready template. It centralizes all customizable design tokens
(typography, vertical spacing and radii, light & dark palettes, tables, code
blocks, blockquotes, scrollbars) at the top, features clean Latin-focused
typography stacks with documented extension points for CJK, Arabic, Cyrillic,
and Indic scripts, and serves as an ideal reference or starting point.

## Built-in Styles

`md_github.css` and `md_night.css` are complete GitHub-derived document
stylesheets. `md_theme.css` contains the common Seer theme tokens plus the
`!important` structural overrides that map them onto `hr`, `blockquote`,
`thead`, `mark`, `::selection`, tables and alert blocks, while the small
`md_*.css` files add the bundled named-theme overrides. They are useful
references when creating a new stylesheet.

`md_alert.css` is the helper sheet for the five GitHub-style alert blocks
(surfaces, borders and icons); both bundled Markdown preview templates link it.

Single-file HTML exports inline only the injected `<style>`. Helper sheets
loaded via `<link>` (`md_chrome.css`, `md_alert.css`) are not carried over, so
rules that live only there do not apply to an exported document.

`md_paperwhite.css` is the one named theme that is not a plain token sheet. The
base stylesheets hard-code their own bold and italic colours, gradient washes,
table surfaces and alert palettes, so this file adds scoped rule overrides on
top of its grayscale tokens, including an achromatic ramp for the five GitHub
alert types.

The bundled named-theme files (nine of them, such as `md_obsidian.css`,
`md_sahara.css`, and `md_paperwhite.css`) are color overlays designed to sit
on top of `md_github.css` or `md_night.css`, while `md_theme.css` acts as the
shared token and override layer. None of these is a standalone document
stylesheet: pointing **Custom CSS path** at any one of them provides theme
tokens and scoped colour overrides, but no base typography or container layout,
because Seer skips its base document stylesheet in Custom mode (while still
loading helper styles in `md_chrome.css`).
