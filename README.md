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
html[data-darkmode="light"]
html[data-darkmode="dark"]
body
#markdown-body.markdown-body
pre, code, blockquote, table, img
```

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
`html[data-darkmode="dark"]`, or make the stylesheet dark by default. Test
headings, links, long code blocks, tables, images, and mathematics in both the
Seer light and dark application themes before sharing a theme.

## Built-in Styles

`md_github.css` and `md_night.css` are complete GitHub-derived document
stylesheets. `md_theme.css` contains the common Seer theme tokens, while the
small `md_*.css` files add the bundled named-theme overrides. They are useful
references when creating a new stylesheet.
