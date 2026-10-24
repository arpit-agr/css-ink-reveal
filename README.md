# Landscape — ink mask reveal

A single-page demo that reveals a painting with an animated ink-brush mask, using
only CSS. The page shows Henri-Edmond Cross's *Landscape* (ca. 1904) in a
responsive five-column grid layout inspired by Karl Gerstner.

## How the reveal works

The image sits inside `.masked-img`. An `::after` pseudo-element covers it with
the page background color and uses a sprite sheet of ink-splash frames as its
`mask-image`:

```css
.masked-img:after {
	background-color: var(--bg-color);
	mask-image: url(/assets/ink-mask-2.png);
	mask-size: 4600% 100%;
	animation: inkMask 2s steps(39) forwards;
}

@keyframes inkMask {
	0%   { mask-position: 0% 50%; }
	100% { mask-position: 100% 50%; }
}
```

`steps()` jumps the mask across the sprite one frame at a time, so the overlay
"dissolves" like ink spreading and the painting shows through. The animation is
wrapped in a `prefers-reduced-motion: no-preference` query, so users who ask
for reduced motion see the image straight away.

## Getting started

Requires Node.js 18 or newer.

```bash
npm install
```

```bash
npm run dev
```

| Script            | What it does                             |
| ----------------- | ---------------------------------------- |
| `npm run dev`     | Start the Vite dev server                |
| `npm run build`   | Build the static site into `dist/`       |
| `npm run preview` | Serve the production build locally       |

## Project structure

```
index.html              Page markup
postcss.config.cjs      Enables postcss-custom-media
public/assets/          Painting, ink mask sprites, background art
src/css/
  style.css             Entry point; imports the files below and declares layer order
  reset.css             CSS reset            (@layer reset)
  global.css            Tokens, custom media queries, base styles (@layer global)
  demo.css              Layout and the mask animation (@layer demo)
  utilities.css         .wrapper, .region, .flow, .stack (@layer utilities)
```

The CSS uses cascade layers, native nesting, subgrid and container query units
(`cqi`, for the fluid heading). Breakpoints are written as custom media queries
(`@media (--md-n-above)`) and compiled by
[postcss-custom-media](https://github.com/csstools/postcss-plugins/tree/main/plugins/postcss-custom-media).

## Credits

- Artwork and description: [The Met](https://www.metmuseum.org/art/collection/search/334129)
- Image mask technique: [Ryan Yu](https://codepen.io/iamryanyu/pen/GXpyLQ)
- Layout inspiration: Karl Gerstner
