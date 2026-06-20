# Astro Features Guide

This guide explains what Astro can do and what features are available to you as your website grows. Written in plain English — no prior experience needed.

---

## What makes Astro different

Most website frameworks send a lot of JavaScript to the browser and build the page there. Astro does the opposite — it does all the work when you build the site, and sends the browser plain HTML. The result is a website that loads almost instantly, because the browser has almost nothing to do.

Think of it like this:
- **Other frameworks** — send flat-pack furniture and instructions, the browser builds it
- **Astro** — sends the furniture already built

This makes Astro websites fast, lightweight, and great for search engines.

---

## Feature 1 — Pages and Routing

Astro uses **file-based routing**. The name of the file becomes the address of the page. No configuration needed.

```
src/pages/index.astro        →  lawtechnology.net/
src/pages/about.astro        →  lawtechnology.net/about
src/pages/services.astro     →  lawtechnology.net/services
src/pages/contact.astro      →  lawtechnology.net/contact
```

To add a new page to your site, create a new `.astro` file in `src/pages/`. That is all.

### Example — adding a Privacy Policy page

Create `src/pages/privacy.astro`:

```astro
---
import Layout from '../layouts/Layout.astro';
---

<Layout title="Privacy Policy — Law Technology">
  <main style="max-width: 800px; margin: 120px auto; padding: 0 24px;">
    <h1>Privacy Policy</h1>
    <p>Your privacy policy content goes here.</p>
  </main>
</Layout>
```

It is now live at `lawtechnology.net/privacy`. No other changes needed.

---

## Feature 2 — Layouts

A **layout** is a wrapper that goes around your page content. It handles things that are the same on every page — the `<head>` tag, the page title, global styles, fonts.

This site has one layout: `src/layouts/Layout.astro`. Every page uses it.

You can create multiple layouts for different purposes — for example a plain layout for legal pages and a full layout with navigation for normal pages.

### Example — a minimal layout for legal pages

```astro
---
// src/layouts/MinimalLayout.astro
const { title } = Astro.props;
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>{title}</title>
</head>
<body style="background:#030712; color:#f1f5f9; font-family:system-ui;">
  <slot />
</body>
</html>
```

The `<slot />` tag is where the page content goes when the layout is used.

---

## Feature 3 — Components

A **component** is a reusable piece of the page. You write it once and use it anywhere.

This site already uses components — Navbar, Hero, Services, Contact, Footer. But you can create components for anything that appears more than once: a testimonial card, a team member card, a pricing block, a call-to-action banner.

### Example — a reusable info card component

Create `src/components/InfoCard.astro`:

```astro
---
const { title, text, icon } = Astro.props;
---

<div class="card">
  <div class="icon">{icon}</div>
  <h3>{title}</h3>
  <p>{text}</p>
</div>

<style>
  .card {
    background: var(--bg3);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 28px;
  }
  h3 { color: var(--text); margin-bottom: 8px; }
  p  { color: var(--muted); font-size: .9rem; }
</style>
```

Use it anywhere:

```astro
---
import InfoCard from '../components/InfoCard.astro';
---

<InfoCard title="Fast Response" text="We get back to you within 24 hours." icon="⚡" />
<InfoCard title="Local Support" text="Available on-site or remotely." icon="📍" />
```

---

## Feature 4 — Markdown and Blog Posts

Astro has built-in support for **Markdown** — a simple way to write content without any HTML. You write in plain text with simple formatting symbols, and Astro converts it to a proper webpage.

This is perfect for:
- A blog or news section
- Knowledge base articles
- Case studies
- FAQs

### How Markdown works

A Markdown file looks like this:

```markdown
---
title: Setting Up a UniFi Network
date: 2026-06-01
description: A guide to getting your UniFi system up and running.
---

# Setting Up a UniFi Network

UniFi is a range of networking equipment made by Ubiquiti.
Here is how to get started.

## Step 1 — Install the Controller

Download the UniFi Network application from the Ubiquiti website...

## Step 2 — Adopt Your Devices

Once the controller is running, it will detect any UniFi devices on your network...
```

The section between the `---` lines at the top is called **frontmatter** — it stores information about the post like the title and date.

### Setting up a blog

1. Create a folder: `src/pages/blog/`
2. Add Markdown files to it: `src/pages/blog/unifi-setup.md`
3. The post is now available at `lawtechnology.net/blog/unifi-setup`

To list all blog posts on a page, Astro can automatically gather them for you using `Astro.glob()`:

```astro
---
const posts = await Astro.glob('./blog/*.md');
---

{posts.map(post => (
  <a href={post.url}>
    <h3>{post.frontmatter.title}</h3>
    <p>{post.frontmatter.description}</p>
  </a>
))}
```

---

## Feature 5 — Image Optimisation

Astro has a built-in image tool that automatically:
- Resizes images so they are not larger than they need to be
- Converts images to modern formats like WebP (which load faster than JPG or PNG)
- Adds the correct width and height to prevent layout shifts while loading

### How to use it

Instead of a plain `<img>` tag, use Astro's `<Image>` component:

```astro
---
import { Image } from 'astro:assets';
import myPhoto from '../assets/my-photo.jpg';
---

<Image src={myPhoto} alt="Description" width={800} height={400} />
```

Images used this way go in `src/assets/` rather than `public/`. Astro processes them at build time and serves optimised versions automatically.

> **Note:** Images in `public/` are served as-is with no processing. Images in `src/assets/` are optimised. For logos and heading images you control, `public/` is fine. For photos, `src/assets/` is better.

---

## Feature 6 — SEO (Search Engine Optimisation)

SEO is how search engines like Google find and rank your site. Astro gives you full control over the HTML `<head>` section where SEO tags live.

### The basics — already set up

The `Layout.astro` file already includes a `<title>` and `<meta name="description">` tag. These are the two most important SEO elements.

### Adding more SEO tags

Open `src/layouts/Layout.astro` and add tags inside the `<head>`:

```html
<!-- Tells Google what the page is about -->
<meta name="description" content="Law Technology provides expert IT support in your area." />

<!-- Controls how the page looks when shared on social media -->
<meta property="og:title" content="Law Technology" />
<meta property="og:description" content="Expert technology solutions for home and business." />
<meta property="og:image" content="/logo-banner.png" />

<!-- Stops search engines indexing a page (useful for thank-you pages) -->
<meta name="robots" content="noindex" />
```

### Sitemap

A sitemap is a file that tells Google every page on your site. Astro can generate one automatically with a plugin:

```powershell
npx astro add sitemap
```

Once added, a `sitemap-index.xml` file is generated at build time and submitted to Google via Google Search Console.

---

## Feature 7 — Integrations and Plugins

Astro has a large library of official integrations that add extra capabilities. They are installed with a single command.

| Integration | What it adds | Install command |
|---|---|---|
| `@astrojs/sitemap` | Auto-generates a sitemap for Google | `npx astro add sitemap` |
| `@astrojs/tailwind` | Adds Tailwind CSS utility classes | `npx astro add tailwind` |
| `@astrojs/react` | Lets you use React components inside Astro | `npx astro add react` |
| `@astrojs/partytown` | Moves analytics scripts off the main thread for speed | `npx astro add partytown` |
| `astro-icon` | Easy access to thousands of icon sets | `npm install astro-icon` |

### Example — adding Google Analytics

1. Install Partytown so Analytics does not slow down the site:
```powershell
npx astro add partytown
```

2. Add your Google Analytics script to `Layout.astro`:
```html
<script type="text/partytown" src="https://www.googletagmanager.com/gtag/js?id=G-XXXXXXXXXX"></script>
```

---

## Feature 8 — Content Collections

Content Collections are a more structured way to manage large amounts of content — blog posts, team members, FAQs, case studies. They add automatic validation so you cannot accidentally miss a required field like a title or date.

### Setting up a collection

Create `src/content/config.ts`:

```ts
import { defineCollection, z } from 'astro:content';

const blog = defineCollection({
  schema: z.object({
    title:       z.string(),
    date:        z.date(),
    description: z.string(),
    image:       z.string().optional(),
  }),
});

export const collections = { blog };
```

Now any Markdown file in `src/content/blog/` must have a `title`, `date`, and `description` — Astro will warn you at build time if one is missing.

---

## Feature 9 — View Transitions (Animated Page Changes)

Astro supports smooth animated transitions between pages — instead of a full page reload, sections of the page fade or slide into place.

Enable it by adding one import to `Layout.astro`:

```astro
---
import { ViewTransitions } from 'astro:transitions';
---

<head>
  <ViewTransitions />
</head>
```

That is all. Astro automatically animates page changes with a crossfade. You can customise the animation on individual elements if needed.

---

## Feature 10 — Forms and Server Actions

For a purely static site, forms need a third-party service like Formspree. But if you switch to **server mode**, Astro can handle form submissions directly — no third-party needed.

### Switching to server mode

In `astro.config.mjs`:

```js
export default defineConfig({
  output: 'server',        // change from 'static' to 'server'
  adapter: node(),         // tells Astro to run on Node.js
});
```

You would also need to update the Dockerfile to run the Node server rather than nginx. This is a bigger change but unlocks server-side features like:

- Handling form submissions without Formspree
- Connecting to a database
- User login / authentication
- Generating pages with live data

---

## Summary — What to use and when

| You want to... | Feature to use |
|---|---|
| Add a new page | Create a file in `src/pages/` |
| Reuse the same block on multiple pages | Create a component in `src/components/` |
| Write a blog post or article | Markdown file in `src/pages/blog/` |
| Optimise photos automatically | Use `<Image>` from `astro:assets`, put files in `src/assets/` |
| Help Google find your site | Add `@astrojs/sitemap` integration |
| Animate page transitions | Add `<ViewTransitions />` to Layout.astro |
| Add Tailwind CSS | Run `npx astro add tailwind` |
| Handle forms without Formspree | Switch output to `server` mode |
| Manage lots of structured content | Use Content Collections |
