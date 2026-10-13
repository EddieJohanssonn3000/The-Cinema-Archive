# The Cinema Archive — Project Instructions

## Project Overview

The Cinema Archive is a second-hand film e-commerce store built as a five-week final project before my internship (LIA).

The store sells physical films, primarily DVD and Blu-ray, and focuses on film discovery, curation, and film history.

The project is intended to demonstrate practical Shopify theme development and my understanding of Liquid, Shopify Metafields, Metaobjects, and reusable sections.

## Tech Stack

- Shopify Development Store
- Shopify Horizon theme
- Shopify Liquid
- HTML, CSS, and JavaScript
- Shopify Metafields and Metaobjects
- Shopify CLI for local development and theme deployment
- Git and GitHub for version control

Shopify handles products, inventory, cart, checkout, and hosting. Do not introduce a custom backend or database.

## Core Requirements

1. Customize the Horizon theme to create a distinctive storefront.
2. Build custom, reusable Shopify Liquid sections.
3. Use product Metafields for:
   - Condition
   - Format
   - Release year
   - Runtime
4. Use a Director Metaobject to store director information.
5. Connect films to directors and display their filmography on director pages.
6. Build responsive homepage, collection, product, and director pages.
7. Preserve Shopify's native cart and checkout functionality.
8. Keep the project within a realistic five-week scope.

AI recommendations and external APIs are optional and must not take priority over the core requirements.

## Development Principles

- Inspect existing Horizon code before modifying it.
- Reuse existing theme patterns and components whenever appropriate.
- Prefer small, focused changes over large rewrites.
- Use Shopify-native features instead of introducing unnecessary dependencies.
- Keep custom code maintainable, readable, and responsive.
- Follow Shopify theme conventions and accessibility best practices.
- Do not remove existing functionality without explaining why.
- Do not modify unrelated files.

## Learning and Explanation

I am learning Shopify and Liquid, so explain your reasoning rather than only providing code.

When proposing a change:
1. Explain the problem and the proposed solution.
2. Identify which files need to change and why.
3. Explain important Liquid concepts and syntax.
4. Implement the smallest reasonable solution.
5. Explain how I can test the result in Shopify.

Do not rewrite entire files when a smaller change is sufficient.

Ask before making significant architectural changes or introducing new dependencies.

## Safety and Git

- Never expose, hardcode, or commit API keys, access tokens, passwords, or other secrets.
- Never modify Git remotes or rewrite Git history without explicit permission.
- Inspect `git status` before potentially destructive operations.
- Do not overwrite project files without checking their existing contents.
- Explain the purpose of Git commands before recommending them.

## Definition of Done

A feature is complete when:
- It works in the Shopify development store.
- It behaves correctly on desktop and mobile.
- It uses Shopify data correctly.
- It has been tested for relevant empty or missing data.
- The implementation and important decisions have been explained to me.