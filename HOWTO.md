# How To Guide — Law Technology Website

This guide explains how the website works and how to make changes to it, written in plain English. No coding experience assumed.

---

## How the site is put together

Think of the website like a set of Lego bricks. Each section of the page — the header, the hero, the services, the contact form, the footer — is its own separate file called a **component**. They all live in the `src/components/` folder.

When you build the site, all those separate pieces get assembled into one complete webpage automatically.

Here is what each file is responsible for:

| File | What it controls |
|---|---|
| `Navbar.astro` | The top bar with the logo and navigation links |
| `Hero.astro` | The big full-screen section at the top with the heading image and buttons |
| `About.astro` | The "About Us" section with the stat cards |
| `Services.astro` | The grid of service cards |
| `Contact.astro` | The contact form and details |
| `Footer.astro` | The bottom bar with the logo and copyright |
| `Layout.astro` | The invisible wrapper — sets the page title, favicon, and colours used everywhere |
| `index.astro` | The glue — tells the site which components to show and in what order |

So if something on the hero section needs changing, you go to `Hero.astro`. If a service card needs updating, you go to `Services.astro`. You never need to touch any other file for that change.

---

## What is inside each component file

Every `.astro` file has up to three parts:

```
---
This is the data section. It only runs when the site is being built,
not in the browser. You put things like lists of services or contact
details here.
---

This is the HTML section. This is what actually appears on the page.
You write normal web content here — headings, paragraphs, images, buttons.

<style>
  This is the styling section. This controls how things look —
  colours, sizes, spacing, fonts. Changes here only affect THIS file,
  so you can't accidentally break something else.
</style>
```

You do not need to fill all three sections. Some components only use the HTML section.

---

## Images

### How images work

All images on the site live in one folder called `public/`. When you reference an image in the code, you just write `/filename.png` and the site knows to look in the `public/` folder.

### How to add a new image

1. Copy your image file into the `public/` folder
2. In the component file where you want the image to appear, write:

```html
<img src="/your-filename.png" alt="Description of the image" />
```

The `alt` text is a short description of what the image shows. It is used by screen readers and shows if the image fails to load. Always include it.

### How to control the size of an image

Add a `style` directly to the image tag:

```html
<!-- Set a fixed width — height adjusts automatically -->
<img src="/your-filename.png" alt="Description" style="width: 300px; height: auto;" />

<!-- Make it fill its container -->
<img src="/your-filename.png" alt="Description" style="width: 100%; height: auto;" />
```

### How to replace an existing image

Drop a new file with the **exact same filename** into `public/`. The site will automatically use the new file. No code changes needed.

For example, to update the logo, replace `public/logo-banner.png` with your new logo — just make sure the new file is also called `logo-banner.png`.

> **Important:** Keep filenames lowercase with hyphens instead of spaces. Use `team-photo.png` not `Team Photo.png`. Spaces in filenames can cause the image to not load on the live server.

---

## Icons

### What icons are on this site

The icons on the service cards are tiny drawings made using a web standard called SVG (Scalable Vector Graphics). SVG lets you describe a shape using coordinates on a small 24×24 grid — like giving someone directions to draw a picture on graph paper.

They are drawn using lines and paths rather than being photo or image files, which means they are sharp at any size and change colour automatically to match the surrounding text.

### How to find an icon

You do not need to draw icons yourself. The site [**lucide.dev/icons**](https://lucide.dev/icons/) has hundreds of free icons in the same style already used on the site.

1. Go to [lucide.dev/icons](https://lucide.dev/icons/)
2. Type what you are looking for in the search box — for example: `server`, `database`, `wifi`, `printer`, `tool`
3. Click the icon you want
4. Click **Copy SVG**

### How to use the icon you copied

When you copy an SVG from Lucide it looks like this:

```html
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"
     fill="none" stroke="currentColor" stroke-width="2">
  <rect x="2" y="2" width="20" height="8" rx="2"/>
  <rect x="2" y="14" width="20" height="8" rx="2"/>
  <line x1="6" y1="6" x2="6.01" y2="6"/>
</svg>
```

You only need the lines **inside** the `<svg>` tags — not the `<svg>` tag itself. So from the example above, you only want:

```
<rect x="2" y="2" width="20" height="8" rx="2"/><rect x="2" y="14" width="20" height="8" rx="2"/><line x1="6" y1="6" x2="6.01" y2="6"/>
```

That gets pasted into the `icon:` field of a service card in `Services.astro`.

### What do all those numbers mean

SVG draws on a 24×24 grid. The top-left corner is (0,0) and the bottom-right is (24,24). The shapes are described using that grid:

- `<rect x="2" y="2" width="20" height="8"/>` — draw a rectangle starting at position (2,2) that is 20 units wide and 8 units tall
- `<line x1="6" y1="6" x2="6.01" y2="6"/>` — draw a line from point (6,6) to point (6.01,6) — this creates a tiny dot
- `<circle cx="12" cy="12" r="10"/>` — draw a circle with its centre at (12,12) with a radius of 10 units
- `<path d="M3 9h18"/>` — move to position (3,9) then draw a horizontal line 18 units to the right

You do not need to memorise these — just copy from Lucide and paste. But knowing this helps you understand what you are looking at when you read the code.

### How to add an icon to a service card

Open `src/components/Services.astro`. At the top you will see the list of services. Add a new entry like this:

```js
{
  title: 'Your Service Name',
  desc: 'A description of what you offer.',
  icon: `PASTE YOUR SVG PATHS HERE`,
},
```

---

## HTML Elements

### What is HTML

HTML is the language used to describe what is on a webpage. It uses **tags** — words wrapped in angle brackets — to mark up content. Most tags come in pairs: an opening tag and a closing tag.

```html
<p>This is a paragraph.</p>

<h2>This is a heading.</h2>

<img src="/photo.png" alt="A photo" />
```

The browser reads these tags and knows how to display them.

### Common elements you will use

| Tag | What it does |
|---|---|
| `<p>` | A paragraph of text |
| `<h1>` `<h2>` `<h3>` | Headings — h1 is the biggest, h3 is smaller |
| `<img>` | An image |
| `<a>` | A clickable link |
| `<div>` | A generic container — used to group things together |
| `<button>` | A clickable button |
| `<br />` | A line break |

### How to add a new paragraph of text

Open the component file for the section you want to edit. Find where you want to add the text and write a `<p>` tag:

```html
<p>This is the new text I want to add to the page.</p>
```

### How to add a heading

```html
<h2>This is a section heading</h2>
<h3>This is a smaller sub-heading</h3>
```

### How to add a clickable button

```html
<button onclick="document.getElementById('contact')?.scrollIntoView({behavior:'smooth'})">
  Click Me
</button>
```

The `onclick` part tells the browser what to do when the button is clicked. The example above scrolls the page down to the contact section.

### How to add a new section to the page

1. Create a new file in `src/components/`. Name it something like `Testimonials.astro`.

2. Write the content inside it. Here is a simple starting template:

```html
<section id="testimonials">
  <div class="container">
    <h2>What our customers say</h2>
    <p>Content goes here.</p>
  </div>
</section>

<style>
  section {
    background: var(--bg2);
    padding: 96px 24px;
  }
  .container {
    max-width: 1100px;
    margin: 0 auto;
  }
</style>
```

3. Open `src/pages/index.astro` and add two lines — one at the top to import the file, one in the middle where you want it to appear:

```astro
---
import Testimonials from '../components/Testimonials.astro';
---

...
<About />
<Testimonials />   <!-- appears between About and Services -->
<Services />
...
```

---

## CSS and Styling

### What is CSS

CSS is what makes the page look the way it does — colours, sizes, spacing, fonts, layout. In this site, CSS is written inside `<style>` blocks at the bottom of each component file.

### How CSS works

You write a **selector** (what to style) and then **rules** (how to style it):

```css
p {
  color: white;
  font-size: 1rem;
}
```

This makes all `<p>` tags white with a standard font size. But in Astro, styles are **scoped** — that means if you write this in `About.astro`, it only affects paragraphs inside `About.astro`. It will not accidentally change paragraphs in `Services.astro`.

### Using a class

If you only want to style specific elements (not all paragraphs), give the element a `class`:

```html
<p class="intro-text">This paragraph will be styled differently.</p>
```

Then in the `<style>` block:

```css
.intro-text {
  color: #60a5fa;
  font-size: 1.2rem;
  font-weight: 600;
}
```

The `.` before the name means "find elements with this class".

### Common CSS properties

| Property | What it does | Example |
|---|---|---|
| `color` | Text colour | `color: white;` |
| `background` | Background colour | `background: #111827;` |
| `font-size` | Text size | `font-size: 1.2rem;` |
| `font-weight` | Bold or light text | `font-weight: 700;` |
| `padding` | Space inside an element | `padding: 24px;` |
| `margin` | Space outside an element | `margin: 0 auto;` |
| `border-radius` | Rounded corners | `border-radius: 12px;` |
| `width` | How wide something is | `width: 300px;` |
| `max-width` | Maximum width | `max-width: 100%;` |
| `display` | How something is laid out | `display: flex;` |
| `text-align` | Align text left/center/right | `text-align: center;` |

### The site's built-in colours

Rather than remembering hex codes like `#3b82f6`, the site has named colour variables you can use anywhere:

| Variable | Colour | Use it for |
|---|---|---|
| `var(--bg)` | Very dark navy | Main page background |
| `var(--bg2)` | Slightly lighter navy | Alternate section backgrounds |
| `var(--bg3)` | Dark grey-blue | Card backgrounds |
| `var(--blue)` | Bright blue | Buttons, highlights |
| `var(--blue-lt)` | Light blue | Icons, secondary highlights |
| `var(--cyan)` | Cyan | Accent colour |
| `var(--text)` | Near white | Main body text |
| `var(--muted)` | Grey | Secondary text, descriptions |
| `var(--faint)` | Dark grey | Labels, captions |
| `var(--border)` | Very dark border | Card and section outlines |
| `var(--radius)` | 16px | Standard rounded corners |

Use them like this:

```css
.my-element {
  background: var(--bg3);
  color: var(--muted);
  border: 1px solid var(--border);
  border-radius: var(--radius);
}
```

### Making something look different on mobile

You can write styles that only apply on small screens using a **media query**:

```css
.my-element {
  font-size: 1.5rem;   /* desktop size */
}

@media (max-width: 700px) {
  .my-element {
    font-size: 1rem;   /* smaller on mobile */
  }
}
```

Anything inside `@media (max-width: 700px)` only applies when the screen is 700px wide or narrower — i.e. a phone.

---

## Quick Reference — Common Tasks

| I want to... | Open this file | Change this |
|---|---|---|
| Update the logo | `public/` | Replace `logo-banner.png` with a new file of the same name |
| Change the hero heading image | `public/` | Replace `hero-heading.png` |
| Add a new service card | `src/components/Services.astro` | Add an entry to the `services` array |
| Change the about section text | `src/components/About.astro` | Edit the `<p>` tags inside `.about-body` |
| Change the stat numbers | `src/components/About.astro` | Edit the `stats` array at the top |
| Update contact details | `src/components/Contact.astro` | Edit the `details` array at the top |
| Set up the contact form email | `src/components/Contact.astro` | Replace `YOUR_FORM_ID` with your Formspree ID |
| Change a colour | The relevant component file | Edit the `<style>` block at the bottom |
| Add a new page section | `src/components/` + `src/pages/index.astro` | Create new component, import it in index.astro |
| Change the page title or favicon | `src/layouts/Layout.astro` | Edit the `<title>` or `<link rel="icon">` tags |
