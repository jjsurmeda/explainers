# Explainers

Animated, source-checked slide decks on AI engineering topics, by [Jhay Surmeda](https://www.linkedin.com/in/jjsurmeda) · Loop Engineering.

**Live:** https://jjsurmeda.github.io/explainers/

| Explainer | Link | Published |
|---|---|---|
| Laya vs Jev | [/laya-vs-jev/](https://jjsurmeda.github.io/explainers/laya-vs-jev/) | 2026-09-26 |

## Structure

```
index.html              landing page listing every explainer
shared/presenter/       presenter character poses, reused by every deck
<topic>/index.html      the deck (HTML, CSS and JS in one file)
<topic>/preview.png     1200×630 link-preview image
<topic>/content.md      reviewed source copy for the deck
<topic>/assets/         images only that deck uses (optional)
```

## Adding an explainer

1. Create a folder with a short, lowercase, hyphenated name. It becomes the URL.
2. Add `index.html`, `preview.png` and `content.md`. Reference the presenter as `../shared/presenter/<pose>.webp`.
3. Set `og:url` and `og:image` to absolute `https://jjsurmeda.github.io/explainers/<topic>/…` URLs.
4. Add a card to the root `index.html` and a row to the table above.

Keep videos out of the repo. Post them natively or attach them to a GitHub Release.

## Deck controls

Arrow keys or swipe to move. Autoplay and speed are in the top bar. Deep-link to a slide with `#s1` … `#s12`.
