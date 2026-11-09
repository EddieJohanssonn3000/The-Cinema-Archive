# The Cinema Archive

A second-hand film store built on Shopify. The Cinema Archive sells physical films (DVD and Blu-ray) and puts film discovery and film history first: every copy is a single, inspected item, and every film is connected to its director.

The project is a customised version of Shopify's **Horizon** theme, built as a five-week final project before an internship (LIA). It demonstrates Shopify theme development with Liquid, reusable sections and theme blocks, product metafields and a metaobject for directors.

> **Status:** school project, published on a password-protected Shopify development store. Checkout uses Shopify's test payment gateway; there are no real customers or orders.

---

## Contents

- [Features](#features)
- [Tech stack](#tech-stack)
- [Data model](#data-model)
- [Project structure](#project-structure)
- [Getting started](#getting-started)
- [Store setup (Shopify Admin)](#store-setup-shopify-admin)
- [Working with the Theme Editor](#working-with-the-theme-editor)
- [Git workflow](#git-workflow)
- [Testing](#testing)
- [Known limitations](#known-limitations)
- [Use of AI](#use-of-ai)
- [Credits and license](#credits-and-license)

---

## Features

**Homepage**
- Archive introduction (hero), a dark marquee band and a quote section with a photo.
- *Featured films*: four films from a hand-picked collection, *The Edit*, with a staggered layout and a "View all N films" button.
- *Director focus*: highlights one director from the Directors metaobject.
- Newsletter sign-up using Shopify's native customer form.

**Films (collection page)**
- Collection hero with an automatic product count.
- Toolbar with *Format* and *Condition* dropdowns and sorting in one form. The filters combine (for example Blu-ray + Very good), choices with no films are disabled, and a "Clear filters" link resets both while keeping the sorting. After each choice the page reloads at the toolbar, not at the top.
- Film cards in fixed columns (2 on mobile, 3 on tablets, 5 on desktop, 6 on wide screens), showing director, release year, format and condition from metafields.

**Search**
- Search modal in the archive style. Typing a director's name ("wes", "tarantino", "chloe") shows the director above the matching films; the arrow keys and Enter work for directors as well as films.
- Search results page with the same toolbar and film cards as the Films page.

**Product page**
- Large title, description, price and an "Only one copy" badge when exactly one copy is in stock.
- Film details: director (linked to the director page), format, release year, runtime and condition.
- An "archive note" quote box per film, and "More from [director] in the archive".
- Restyled buy button and sticky add-to-cart bar.

**Cart**
- Slide-in cart that opens over the page with a dark backdrop on every screen size, and a cart page, both in the archive style.
- No quantity selector, since every film is a single copy; the remove button stays.
- "You may also like" on the cart page leaves out films that are already in the cart and sold-out films.
- Shopify's native checkout.

**Directors**
- Directors overview with portrait cards and the number of films per director.
- Director page with portrait, facts (born, country, films in the archive), lead text, biography and filmography.

**Contact page**
- Styled Shopify contact form and a "sell or donate your films" section.

**Details**
- Call-to-action buttons share one behaviour: the arrow cell turns yellow on hover and keyboard focus, and the button presses down slightly when clicked (switched off for visitors who prefer reduced motion).
- Header with a text logo and a grid menu on desktop; touch screens get Shopify's drawer menu in the archive colours.
- Favicon in the archive style, and arrows that stay text instead of turning into emoji on iPhone.

**Responsive** layouts for mobile, tablet and desktop, tested at 390–440, 1024, 1280, 1440, 1920 and 2560 px, on a real iPhone and on the school projector (1920 × 1080).

## Tech stack

- Shopify development store and **Horizon 4.2.0** theme
- Shopify Liquid, HTML, CSS and a little JavaScript (Horizon's own components)
- Product metafields and a Directors metaobject
- Shopify Search & Discovery app (storefront filter for format)
- Shopify CLI for local development
- Git and GitHub

Shopify handles products, inventory, cart, checkout and hosting. There is no custom backend or database.

## Data model

### Product metafields (namespace `custom`)

| Key | Type | Used for |
|---|---|---|
| `format` | Single line text | "DVD" or "Blu-ray". Also the storefront filter. |
| `condition` | Single line text | e.g. "Very good" |
| `release_year` | Integer | e.g. 2003 |
| `runtime` | Integer (minutes) | shown as "1 h 42 min" |
| `director` | Metaobject reference (Directors) | links a film to its director |
| `archive_note` | Multi-line text | quote in the product page's archive note box |
| `archive_note_author` | Single line text | e.g. "Archive employee" |

### Directors metaobject (type `directors`)

| Field | Type |
|---|---|
| `name` | Single line text |
| `portrait` | Image |
| `biography` | Rich text |
| `lead` | Multi-line text (optional) |
| `born` | Integer (optional) |
| `country` | Single line text (optional) |

The metaobject has web pages enabled, so each director gets a page at `/pages/directors/<handle>` (template `templates/metaobject/directors.json`).

**Filmography:** Liquid cannot look up "all products with this director" directly, so the theme scans the first 50 products of `collections.all` and keeps those whose director matches. This is fine for a small, curated catalogue.

Empty or missing values are hidden everywhere, so a film without a runtime or a director without a portrait still renders cleanly.

## Project structure

The theme follows the standard Shopify theme layout. Files added for this project:

```
sections/
  archive-introduction.liquid   homepage hero
  archive-marquee.liquid        dark quote band
  featured-films.liquid         homepage film selection
  archive-quote.liquid          photo + quote section
  director-focus.liquid         one highlighted director
  archive-newsletter.liquid     newsletter band (Horizon email-signup block)
  collection-hero.liquid        Films page hero
  collection-toolbar.liquid     format buttons + sorting
  archive-note.liquid           short editorial quote between two rules
  archive-back-link.liquid      "← Back to archive" on the product page
  product-archive-note.liquid   per-film quote box
  product-director-films.liquid "More from [director]"
  main-directors.liquid         directors overview
  main-director.liquid          director filmography
  director-hero.liquid          director page hero
  director-portrait.liquid      director lead text and biography
  contact-archive.liquid        contact page

blocks/
  film-title.liquid             product title and description
  film-price.liquid             price and "Only one copy" badge
  film-details.liquid           wrapper for snippets/film-details.liquid
  film-buy-note.liquid          note under the buy button, buy button styles

snippets/
  film-card.liquid              reusable film card
  film-card-meta.liquid         card text on the Films page (Horizon cards)
  film-details.liquid           director/format/year/runtime/condition list
  quote-box.liquid              coloured quote box
  archive-note.liquid           quote between two rules

templates/
  page.directors.json           directors overview page
  metaobject/directors.json     director page
```

Small, documented changes were also made to Horizon files, for example `sections/header.liquid` (header layout and mobile menu), `blocks/_header-logo.liquid` (text logo), `blocks/_header-menu.liquid` (active state for links with a query string) and `locales/en.default.json` (button text "Add this copy to cart").

## Getting started

### Prerequisites

- A Shopify store (a development store from the Shopify Partner Dashboard works)
- [Shopify CLI](https://shopify.dev/docs/storefronts/themes/tools/cli)
- Git and Node.js (required by Shopify CLI)

### Run the theme locally

```sh
git clone https://github.com/EddieJohanssonn3000/The-Cinema-Archive.git
cd The-Cinema-Archive
shopify theme dev --store <your-store>.myshopify.com --theme-editor-sync
```

Open `http://127.0.0.1:9292`. If the store is password protected, enter the store password once. `--theme-editor-sync` writes changes made in the Theme Editor back to your local JSON files.

The theme only looks right once the store has the metafields, the Directors metaobject and some content (see below).

### Upload the theme to a store

Push to a new, unpublished theme first, check it, then publish it:

```sh
shopify theme push --unpublished --theme "Cinema Archive"
```

## Store setup (Shopify Admin)

These parts live in the store, not in this repository:

1. **Product metafield definitions** (Settings → Metafields and metaobjects → Products) with the keys and types listed in [Data model](#data-model).
2. **Directors metaobject definition** with the fields above, with web pages (online store) enabled.
3. **Search & Discovery app** → Filters → add a filter for the product metafield *Format*.
4. **Pages:** a page *Directors* using the template `page.directors`, and a *Contact* page.
5. **Main menu:** Home, Films (`/collections/all`), Directors and Contact.
6. **Products:** one product per physical copy, inventory tracked with quantity 1, metafields filled in.
7. **Sender email** (Settings → Notifications) for messages from the contact form.

## Working with the Theme Editor

Section settings and the order of sections are stored in JSON files in `templates/` and `sections/*-group.json`.

- Check `git status` after saving in the Theme Editor. If a template you changed is not listed, pull it from the store before committing.
- Stop `shopify theme dev` before switching Git branches or merging. Otherwise the CLI can upload files mid-switch and overwrite settings in the store.
- If the CLI asks whether to keep the local or remote version, compare first; the local version (from Git) is usually the right one.

## Git workflow

- `main`: stable, tested code
- `dev`: finished features collected before release
- `feature/<name>`: one branch per feature, merged into `dev` when done

## Testing

Checked for each page:

- Screen widths 390–440, 1024, 1280, 1440 and 1920 px (Chrome DevTools, device type *Desktop* for desktop sizes)
- Empty and missing data (no director, no runtime, no portrait, no archive note)
- Cart and checkout, sold-out products, the contact form and the newsletter form
- `shopify theme check` (no errors)

## Known limitations

- Filmographies and "More from…" scan the first 50 products only.
- On touch devices with wide screens (e.g. iPad) Horizon switches the header to its drawer menu, which is not fully styled yet.
- The Directors menu link uses `?view=directors` until the theme is published and the page can be assigned the `page.directors` template.
- The footer still uses Horizon's default design.
- Sold-out copies stay visible in listings.

## Use of AI

AI tools were used as a pair programmer and tutor during the project.

**Tools**
- **Claude Code** (Anthropic), in VS Code, for most of the development work
- **GitHub Copilot**, for some smaller fixes, for example the header dividers

**What AI was used for**
- Recommending, writing and refactoring Liquid, CSS and section schemas based on my descriptions and the Figma design
- Explaining Liquid, Shopify concepts (metafields, metaobjects, theme blocks, storefront filters) and the reasoning behind each change, so I could understand and maintain the code
- Reading Horizon's existing code before changes, so custom code reuses Horizon's patterns instead of rewriting them
- Reviewing diffs before commits, and finding risks and bugs (for example a whitespace bug in the runtime and an HTML-escaping bug in a heading)
- Troubleshooting Shopify CLI and Theme Editor sync problems

**What I did myself**
- The concept, the visual design in Figma and all design decisions
- Setting up the store in Shopify Admin: metafield and metaobject definitions, products, directors, pages, menus and the Search & Discovery filter
- Choosing between the options AI proposed, and deciding what to build and what to leave out
- Git: committing, branching, merging and reviewing what goes into each commit

**How I worked with AI**
- One small step at a time: the AI explained the plan and the files involved before changing anything, and asked for my approval before changes.
- Every change was checked in the browser and with `shopify theme check` before it was committed.
- Facts in the store content (years, runtimes, biographies) are my responsibility to verify, since AI can get them wrong.

## Credits and license

- Based on [Horizon](https://github.com/Shopify/horizon) by Shopify. Horizon's license is in [LICENSE.md](LICENSE.md). It restricts how themes derived from Horizon may be distributed, so this repository is not intended for public distribution or reuse as a theme product.
- Photo in the homepage quote section: Jeremy Yap / Unsplash.
- Film posters and director portraits belong to their respective rights holders and are used for educational purposes only.


- Built by Eddie Johansson.
