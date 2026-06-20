# SVG Guide — Creating and Using SVG Images

SVG stands for **Scalable Vector Graphic**. Unlike a photo (JPG or PNG), an SVG is not made of pixels — it is made of instructions that tell the browser how to draw a shape. This means it looks sharp at any size, on any screen, and the file size is tiny.

This guide explains how to create and use SVGs on the Law Technology site.

---

## How an SVG file is structured

An SVG is just a text file. Open any `.svg` file and you will see something like this:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100">
  <!-- shapes go here -->
</svg>
```

The important parts of the opening tag:

| Attribute | What it does |
|---|---|
| `width` and `height` | How large the SVG is displayed on screen |
| `viewBox="0 0 100 100"` | The internal drawing grid — `0 0` is the top-left corner, `100 100` is the bottom-right |
| `xmlns` | Required — tells the browser this is an SVG. Always leave this as-is |

Think of `viewBox` as the canvas. Everything you draw goes on that canvas. If your viewBox is `0 0 100 100`, the canvas is 100 units wide and 100 units tall, regardless of how big it appears on screen.

---

## The coordinate system

The grid starts at `(0, 0)` in the **top-left corner**:

```
(0,0) ───────────────► x increases →
  │
  │
  │
  ▼
y increases ↓
```

So:
- `x="0" y="0"` is the top-left
- `x="50" y="0"` is the top-middle (on a 100-unit canvas)
- `x="50" y="50"` is the dead centre
- `x="100" y="100"` is the bottom-right

---

## Basic shapes

### Rectangle

```html
<rect x="10" y="10" width="80" height="60" />
```

- `x` and `y` — position of the top-left corner of the rectangle
- `width` and `height` — how big it is
- Add `rx="8"` to get rounded corners: `<rect x="10" y="10" width="80" height="60" rx="8" />`

### Circle

```html
<circle cx="50" cy="50" r="40" />
```

- `cx` and `cy` — the centre point of the circle
- `r` — the radius (half the width)

### Line

```html
<line x1="10" y1="10" x2="90" y2="90" />
```

- `x1, y1` — the starting point
- `x2, y2` — the ending point

### Ellipse (stretched circle)

```html
<ellipse cx="50" cy="50" rx="40" ry="20" />
```

- `rx` — radius on the horizontal axis
- `ry` — radius on the vertical axis

### Text

```html
<text x="50" y="55" text-anchor="middle">Hello</text>
```

- `x` and `y` — position of the text
- `text-anchor="middle"` — centres the text on the x position

---

## The path element

`<path>` is the most powerful shape — it can draw anything. The `d` attribute contains a sequence of drawing commands:

| Command | Meaning | Example |
|---|---|---|
| `M x y` | Move to a point (lift the pen) | `M 10 10` |
| `L x y` | Draw a line to a point | `L 90 10` |
| `H x` | Draw a horizontal line to x | `H 90` |
| `V y` | Draw a vertical line to y | `V 90` |
| `Z` | Close the shape (line back to start) | `Z` |
| `C x1 y1 x2 y2 x y` | Draw a curve | Used for smooth bends |
| `A rx ry rot large sweep x y` | Draw an arc | Used for circles and curves |

Lowercase versions (`m`, `l`, `h`, `v`) mean relative — move by that amount from where you currently are, rather than to an exact position.

**Example — drawing a triangle:**

```html
<path d="M 50 10 L 90 90 L 10 90 Z" />
```

This means:
1. `M 50 10` — move to the top-centre
2. `L 90 90` — draw a line to the bottom-right
3. `L 10 90` — draw a line to the bottom-left
4. `Z` — close back to the start

---

## Styling — fill and stroke

By default shapes are black and solid. You control appearance with `fill` and `stroke`:

| Attribute | What it does |
|---|---|
| `fill` | The colour inside the shape |
| `fill="none"` | No fill — transparent inside |
| `stroke` | The colour of the outline |
| `stroke-width` | How thick the outline is |
| `stroke-linecap` | How line ends look — `round`, `square`, `butt` |
| `stroke-linejoin` | How corners look — `round`, `miter`, `bevel` |
| `opacity` | Transparency — `0` is invisible, `1` is fully visible |

**Example — a blue circle with a white outline:**

```html
<circle cx="50" cy="50" r="40" fill="#3b82f6" stroke="white" stroke-width="3" />
```

**Example — an outline-only rectangle (no fill):**

```html
<rect x="10" y="10" width="80" height="60" rx="8" fill="none" stroke="#60a5fa" stroke-width="2" />
```

**Using `currentColor`** — if you set `stroke="currentColor"`, the shape automatically uses whatever text colour surrounds it. This is how the icons on this site work — they adapt to light or dark backgrounds without needing a hardcoded colour.

---

## A complete simple example

A shield icon, drawn from scratch:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
     fill="none" stroke="currentColor" stroke-width="1.8"
     stroke-linecap="round" stroke-linejoin="round">

  <!-- Outer shield shape -->
  <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>

  <!-- Tick inside the shield -->
  <polyline points="9 12 11 14 15 10"/>

</svg>
```

Breaking it down:
- The `<svg>` tag sets up a 24×24 canvas with no fill and a stroked outline
- The `<path>` draws the shield shape using move and line commands
- The `<polyline>` draws the tick mark through three connected points

---

## Tools to help you build SVGs

You do not always need to write SVG by hand. These tools let you draw visually:

| Tool | What it is | Link |
|---|---|---|
| **SVG Path Editor** | Type or paste path data and see it drawn live. Good for understanding what a path does. | [yqnn.github.io/svg-path-editor](https://yqnn.github.io/svg-path-editor/) |
| **Boxy SVG** | A full visual SVG editor in the browser. Draw shapes and export the SVG code. | [boxy-svg.com](https://boxy-svg.com/) |
| **Lucide Icons** | Hundreds of ready-made icons in the same style used on this site. Copy the SVG and paste it straight in. | [lucide.dev/icons](https://lucide.dev/icons/) |
| **SVGViewer** | Paste SVG code and see it rendered instantly. Good for testing. | [svgviewer.dev](https://svgviewer.dev/) |

---

## How to use an SVG on this site

### Option 1 — As an image file

Save your SVG as a `.svg` file, place it in the `public/` folder, and reference it like any other image:

```html
<img src="/my-icon.svg" alt="Description" style="width: 48px; height: auto;" />
```

### Option 2 — Inline in the HTML

Paste the SVG code directly into the component. This lets you control the colour with CSS:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" viewBox="0 0 24 24"
     fill="none" stroke="currentColor" stroke-width="1.8">
  <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>
</svg>
```

### Option 3 — As a service card icon

Copy just the inner path elements (not the `<svg>` wrapper) and paste into the `icon` field in `Services.astro`:

```js
{
  title: 'My Service',
  desc: "Description of the service.",
  icon: `<path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/>`,
},
```

---

## Quick reference — common shapes

```html
<!-- Rounded rectangle -->
<rect x="2" y="2" width="20" height="20" rx="4"/>

<!-- Circle -->
<circle cx="12" cy="12" r="10"/>

<!-- Horizontal line -->
<line x1="2" y1="12" x2="22" y2="12"/>

<!-- Simple arrow → -->
<path d="M5 12h14M13 6l6 6-6 6"/>

<!-- House shape -->
<path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>

<!-- Database / storage cylinder -->
<ellipse cx="12" cy="5" rx="9" ry="3"/>
<path d="M21 5v6c0 1.66-4 3-9 3s-9-1.34-9-3V5"/>
<path d="M3 11v6c0 1.66 4 3 9 3s9-1.34 9-3v-6"/>
```
