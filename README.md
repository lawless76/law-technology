# Law Technology — Website

The official website for **Law Technology**, a technology solutions business offering hardware installation, networking, domain management, and web setup services.

Built with [Astro](https://astro.build) — a static site framework that compiles everything down to fast, plain HTML with no JavaScript overhead unless needed.

---

## Tech Stack

| Layer | Tool |
|---|---|
| Framework | [Astro](https://astro.build) v4 |
| Styling | Scoped CSS (no framework) |
| Icons | Hand-written SVG paths (Lucide style, 24×24) |
| Contact Form | [Formspree](https://formspree.io) (requires setup — see below) |
| Container | Docker + nginx:alpine |
| CI/CD | GitHub Actions → GHCR |
| Local Hosting | Dockge |
| Production | Cloudflare Pages (optional) |

---

## Project Structure

```
law-technology/
├── .github/
│   └── workflows/
│       └── docker.yml          # Builds and pushes Docker image on every push to main
├── public/                     # Static assets — served as-is, no processing
│   ├── favicon.png
│   ├── logo-banner.png
│   ├── hero-heading.png
│   ├── about-heading.png
│   ├── services-heading.png
│   └── contact-heading.png
├── src/
│   ├── layouts/
│   │   └── Layout.astro        # HTML shell — <head>, global CSS, fonts
│   ├── components/
│   │   ├── Navbar.astro        # Fixed top navigation + mobile hamburger menu
│   │   ├── Hero.astro          # Full-screen hero with circuit board background
│   │   ├── About.astro         # About section with stat cards
│   │   ├── Services.astro      # Service cards grid
│   │   ├── Contact.astro       # Contact form (Formspree)
│   │   └── Footer.astro        # Footer with logo and nav links
│   └── pages/
│       └── index.astro         # Root page — imports and assembles all components
├── astro.config.mjs            # Astro configuration (static output)
├── docker-compose.yml          # Dockge/Docker stack definition
├── Dockerfile                  # Multi-stage build: Node (build) → nginx (serve)
├── nginx.conf                  # nginx config — serves static files with gzip
├── package.json
└── tsconfig.json
```

### How the pages are assembled

Astro uses a component model similar to React but compiles to static HTML at build time.

1. `src/pages/index.astro` is the only page. It imports each component in order.
2. Each component (`Navbar.astro`, `Hero.astro`, etc.) contains its own HTML, scoped CSS, and any client-side `<script>` tags.
3. `src/layouts/Layout.astro` wraps everything — it provides the `<html>`, `<head>`, and global CSS variables used across all components.
4. At build time (`npm run build`), Astro compiles everything into a `dist/` folder of plain HTML, CSS, and JS files.

---

## Local Development

### Prerequisites

- [Node.js](https://nodejs.org) v18 or later
- npm (comes with Node)

### Run the dev server

```powershell
cd law-technology
npm install
npm run dev
```

The site will be available at `http://localhost:4321`. Changes to any `.astro` file hot-reload instantly in the browser.

### Build for production

```powershell
npm run build
```

Output goes to `dist/`. You can preview it locally with:

```powershell
npm run preview
```

---

## Editing Content

### Adding or editing a service card

Open `src/components/Services.astro`. Each service is an object in the `services` array at the top of the file:

```js
{
  title: 'Service Name',
  desc: 'A short description of the service.',
  icon: `<path d="..."/>`,   // SVG path data — see Icon Resources below
},
```

Add a new object to the array to create a new card. The grid automatically adjusts.

### Replacing a heading image

All heading images live in `public/`. To replace one, simply drop a new file with the same filename into `public/` and push — no code changes needed.

Current heading images:

| File | Used in |
|---|---|
| `hero-heading.png` | Hero section |
| `about-heading.png` | About section |
| `services-heading.png` | Services section |
| `contact-heading.png` | Contact section |

### Updating contact details

Open `src/components/Contact.astro`. The details array near the top controls the email, location, and response time displayed on the left side of the contact section:

```js
const details = [
  { emoji: '📍', label: 'Location',      value: 'Available locally & remotely' },
  { emoji: '⏱',  label: 'Response Time', value: 'Within 24 hours' },
  { emoji: '📧', label: 'Email',         value: 'contact@lawtech.example.com' },
];
```

### Setting up the contact form

The form submits to [Formspree](https://formspree.io):

1. Create a free account at formspree.io
2. Create a new form — you'll get an ID like `xpwzrjkq`
3. Open `src/components/Contact.astro` and replace `YOUR_FORM_ID` near the top:

```js
const FORMSPREE_ID = 'xpwzrjkq';   // ← your ID here
```

Form submissions will then be delivered to your email.

### Icon resources

All service card icons are 24×24 SVG paths in the Lucide style. To find icons:

- Browse [lucide.dev/icons](https://lucide.dev/icons/) and search for what you need
- Click the icon, select **SVG**, and copy the inner path/shape elements (everything inside the `<svg>` tags)
- Paste the copied paths into the `icon` field of a service object

---

## Adding Components and Content

This section explains how to add new sections, images, icons, and HTML elements to the site.

---

### Images

Images are the simplest thing to add. All images go in the `public/` folder and are referenced with a `/` prefix.

**To add a new image:**

1. Drop the file into `public/` — e.g. `public/my-image.png`
2. Reference it anywhere in a `.astro` file as `/my-image.png`:

```html
<img src="/my-image.png" alt="Description of image" />
```

**To control the size**, add a `style` attribute or a CSS class:

```html
<!-- Fixed width, height scales automatically -->
<img src="/my-image.png" alt="Description" style="width: 300px; height: auto;" />

<!-- Full width of its container -->
<img src="/my-image.png" alt="Description" style="width: 100%; height: auto;" />
```

**To replace an existing image**, drop a new file with the exact same filename into `public/` and push. No code changes needed — the file reference in the component stays the same.

> **Tip:** Keep image filenames lowercase with hyphens — e.g. `team-photo.png` not `Team Photo.png`. Spaces in filenames can cause issues on Linux servers.

---

### Icons

Icons on this site are inline SVGs — small vector drawings defined directly in the HTML. They use a 24×24 coordinate grid and are styled with `stroke` (outline) rather than `fill` (solid), which is why they look like line drawings.

#### Finding icons

The easiest source is **[Lucide](https://lucide.dev/icons/)** — a free icon library that matches the style already used on the site.

1. Go to [lucide.dev/icons](https://lucide.dev/icons/) and search for what you need (e.g. "server", "database", "wifi")
2. Click the icon
3. Click **Copy SVG**
4. You only need the *inner* content — everything between the `<svg ...>` and `</svg>` tags

For example, copying the **server** icon gives you the full SVG:
```html
<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" ...>
  <rect x="2" y="2" width="20" height="8" rx="2" ry="2"/>
  <rect x="2" y="14" width="20" height="8" rx="2" ry="2"/>
  <line x1="6" y1="6" x2="6.01" y2="6"/>
  <line x1="6" y1="18" x2="6.01" y2="18"/>
</svg>
```

You only paste the inner lines (without the `<svg>` wrapper) into the `icon` field in `Services.astro`:
```js
icon: `<rect x="2" y="2" width="20" height="8" rx="2" ry="2"/><rect x="2" y="14" width="20" height="8" rx="2" ry="2"/><line x1="6" y1="6" x2="6.01" y2="6"/><line x1="6" y1="18" x2="6.01" y2="18"/>`,
```

#### Understanding SVG path commands

SVG paths use a mini drawing language. The `d="..."` attribute on a `<path>` element is a sequence of commands:

| Command | What it does | Example |
|---|---|---|
| `M x,y` | Move to a point (pick up the pen) | `M 3,9` — move to x=3, y=9 |
| `L x,y` | Draw a line to a point | `L 21,9` — line to x=21, y=9 |
| `H x` | Horizontal line to x | `H 18` — line across to x=18 |
| `V y` | Vertical line to y | `V 21` — line down to y=21 |
| `C x1,y1 x2,y2 x,y` | Curved line (bezier) | Used for smooth arcs |
| `Z` | Close the path (join back to start) | |

Lowercase versions (`m`, `l`, `h`, `v`) mean *relative* — move by that amount from where you are, rather than to an absolute position.

So `<path d="M3 9h18"/>` means: move to (3, 9), then draw a horizontal line 18 units to the right — ending at (21, 9). That's the toolbar line in the browser icon.

#### Using an icon outside of a service card

To use an icon anywhere else in the site, write it as a full `<svg>` element:

```html
<svg
  width="24"
  height="24"
  fill="none"
  stroke="currentColor"
  stroke-width="1.8"
  stroke-linecap="round"
  stroke-linejoin="round"
  viewBox="0 0 24 24"
>
  <!-- paste inner paths here -->
  <rect x="2" y="2" width="20" height="8" rx="2"/>
</svg>
```

Setting `stroke="currentColor"` means the icon automatically inherits the text colour of whatever element it sits inside, so you rarely need to set a colour manually.

---

### HTML Elements

Astro components are `.astro` files. Inside each file there are three sections:

```
---
// 1. Frontmatter — JavaScript that runs at BUILD TIME only
//    Use this for data, imports, variables
const title = 'My Title';
---

<!-- 2. HTML template — what gets rendered to the page -->
<h1>{title}</h1>
<p>This is a paragraph.</p>

<style>
  /* 3. Scoped CSS — only applies to THIS component */
  h1 { color: white; }
</style>
```

#### Adding a paragraph or text block

Open the relevant component file and add HTML directly in the template section. For example, to add a paragraph to the About section, open `src/components/About.astro` and add inside `.about-body`:

```html
<div class="about-body">
  <p>First paragraph of text.</p>
  <p>Second paragraph of text.</p>
  <p>A new paragraph you just added.</p>
</div>
```

#### Adding a new HTML element with styling

1. Add the element to the HTML template section
2. Give it a class
3. Add the style in the `<style>` block at the bottom of the same file

```html
<!-- In the HTML section -->
<div class="highlight-box">
  <p>This is a highlighted callout.</p>
</div>

<style>
  /* In the style section */
  .highlight-box {
    background: rgba(59, 130, 246, 0.1);
    border: 1px solid rgba(59, 130, 246, 0.3);
    border-radius: 12px;
    padding: 20px 24px;
    color: #94a3b8;
  }
</style>
```

Styles in `.astro` files are **scoped by default** — a class called `.highlight-box` in `About.astro` will not affect anything in `Services.astro`, even if both use the same class name.

#### Adding a new section to the page

1. Create a new file in `src/components/` — e.g. `src/components/Testimonials.astro`
2. Write your HTML, styles, and any scripts inside it
3. Open `src/pages/index.astro` and import and use the component:

```astro
---
import Layout from '../layouts/Layout.astro';
import Navbar from '../components/Navbar.astro';
import Hero from '../components/Hero.astro';
import About from '../components/About.astro';
import Testimonials from '../components/Testimonials.astro';   // ← add import
import Services from '../components/Services.astro';
import Contact from '../components/Contact.astro';
import Footer from '../components/Footer.astro';
---

<Layout>
  <Navbar />
  <main>
    <Hero />
    <About />
    <Testimonials />   <!-- ← add here in the order you want it to appear -->
    <Services />
    <Contact />
  </main>
  <Footer />
</Layout>
```

#### CSS variables available everywhere

These variables are defined in `src/layouts/Layout.astro` and can be used in any component's `<style>` block:

| Variable | Value | Use for |
|---|---|---|
| `var(--bg)` | `#030712` | Main dark background |
| `var(--bg2)` | `#0f172a` | Slightly lighter background (alternating sections) |
| `var(--bg3)` | `#111827` | Card backgrounds |
| `var(--blue)` | `#3b82f6` | Primary blue — buttons, accents |
| `var(--blue-lt)` | `#60a5fa` | Lighter blue — icons, highlights |
| `var(--cyan)` | `#06b6d4` | Cyan accent |
| `var(--text)` | `#f1f5f9` | Main text colour |
| `var(--muted)` | `#94a3b8` | Secondary text |
| `var(--faint)` | `#4b5563` | Subtle labels, captions |
| `var(--border)` | `#1f2937` | Card and section borders |
| `var(--radius)` | `16px` | Standard border radius |

Example usage:
```css
.my-card {
  background: var(--bg3);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  color: var(--muted);
}
```

---

## Deploying — Local Docker Stack (Dockge)

The site runs as a Docker container behind nginx. The standard workflow is:

### 1. Make your changes locally

Edit files in `src/` or `public/`, test with `npm run dev`.

### 2. Push to GitHub

```powershell
git add .
git commit -m "feat/fix: describe what changed"
git push
```

### 3. GitHub Actions builds the image

The workflow in `.github/workflows/docker.yml` triggers automatically on every push to `main`. It:

- Builds the Astro site inside a Node container
- Copies the `dist/` output into an nginx container
- Pushes the final image to GitHub Container Registry (GHCR) as:
  `ghcr.io/lawless76/law-technology:latest`

Monitor progress in the **Actions** tab of the GitHub repo.

### 4. Pull and restart in Dockge

In Dockge (`http://10.0.1.250`):

1. Open the `law-technology` stack
2. Pull the latest image
3. Restart the stack

The site will be available at **`http://10.0.1.250:4000`**.

### docker-compose.yml (reference)

```yaml
services:
  law-technology:
    image: ghcr.io/lawless76/law-technology:latest
    ports:
      - "4000:80"
    restart: unless-stopped
```

### First-time GHCR setup

After the first GitHub Actions run, make the package public so Dockge can pull without credentials:

1. Go to `github.com/lawless76/law-technology` → **Packages**
2. Click **law-technology** → **Package settings**
3. Set visibility to **Public**

---

## Deploying — Cloudflare Pages

Cloudflare Pages can host the site for free with automatic deployments from GitHub.

### Option A — Connect via GitHub (recommended)

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Go to **Workers & Pages** → **Create** → **Pages** → **Connect to Git**
3. Select the `lawless76/law-technology` repository
4. Set the build configuration:

| Setting | Value |
|---|---|
| Framework preset | Astro |
| Build command | `npm run build` |
| Build output directory | `dist` |

5. Click **Save and Deploy**

Every push to `main` will trigger a new deployment automatically.

### Option B — Manual deploy via Wrangler CLI

```powershell
npm run build
npx wrangler pages deploy dist --project-name=law-technology
```

### Custom domain on Cloudflare Pages

1. In the Pages project, go to **Custom domains** → **Set up a custom domain**
2. Enter your domain (e.g. `lawtechnology.net`)
3. Cloudflare will automatically configure the DNS records

---

## Environment Summary

| Environment | URL | How to deploy |
|---|---|---|
| Local dev | `http://localhost:4321` | `npm run dev` |
| Local Docker | `http://10.0.1.250:4000` | Push to GitHub → Dockge pull |
| Production | `https://lawtechnology.net` | Push to GitHub → Cloudflare Pages auto-deploys |
