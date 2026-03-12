# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based static course website for "Algoritmos y Estructuras de Datos" (Algorithms and Data Structures) at Universidad Austral. The site is hosted on GitHub Pages using the `gh-pages` branch and contains educational presentations and practice assignments.

## Development Commands

Since this is a Jekyll site, use these commands to build and serve locally:

```bash
# Serve the site locally (typically runs on http://localhost:4000)
jekyll serve

# Serve with baseurl configuration
jekyll serve --baseurl /ayed

# Build the site (output to _site/)
jekyll build

# Build with trace for debugging
jekyll build --trace
```

## Architecture

### Three Layout Types

The site uses three distinct Jekyll layouts in `_layouts/`:

1. **`default.html`**: Main layout for general pages (e.g., home page)
2. **`practice.html`**: Layout for practice assignments (TPs), includes practice-specific header/footer
3. **`remark.html`**: Layout for presentations using Remark.js slideshow framework

### Content Organization

**Presentations** (`presentation/` directory):
- Each topic has its own folder (e.g., `introduction/`, `bst/`, `hashtables/`)
- Two-file pattern per presentation:
  - `.html` file: Jekyll frontmatter wrapper with `layout: remark` that includes the `.md` file
  - `.md` file: Actual presentation content in Remark.js format
- Remark.js syntax:
  - `---` separates slides
  - `???` starts presenter notes
  - `class: center, middle, inverse` for slide styling
  - Images stored in same folder as presentation

**Practice Assignments** (`practice/` directory):
- Individual assignments: `tp1.md` through `tp8.md`
- Global assignment: `tp-global.md` (cumulative exercises)
- Each uses `layout: practice` with permalink like `/practice/1`
- Exercises reference Java implementations with full package paths (e.g., `algorithms.stack.DoublyLinkedStack`)

### Site Configuration

- `_config.yml`: Jekyll configuration with site title, tagline, and baseurl (`/ayed`)
- Base URL is important for asset paths and links
- Site uses custom CSS in `css/` directory (cayman.css, normalize.css)
- Remark.js library located in `js/remark.js`

## Working with Content

### Adding a New Presentation

1. Create a new folder under `presentation/` with the topic name
2. Create two files:
   - `presentation.html` (or topic-specific name):
     ```yaml
     ---
     title: Topic Title
     layout: remark
     permalink: /topicname
     ---
     ... include_relative presentation.md ...
     ```
   - `presentation.md` with Remark.js formatted slides
3. Add images to the same folder
4. Update `README.md` to link to the new presentation

### Adding a New Practice Assignment

1. Create `practice/tpN.md` with frontmatter:
   ```yaml
   ---
   title: Práctica N
   layout: practice
   permalink: /practice/N
   ---
   ```
2. Add exercises following the existing format
3. Update `README.md` to link to the new TP

### Enabling/Disabling Content

Content is controlled by HTML comments in `README.md` (which is included in `index.md`). To enable commented-out content, remove the `<!-- -->` wrapper.

## Important Files

- `index.md`: Homepage that includes `README.md` content
- `README.md`: Main course content (presentations list, TPs, rules, instructor info)
- `_config.yml`: Site-wide configuration
- `.gitignore`: Excludes `_site/`, `.jekyll-cache/`, `.DS_Store`, `.idea/`

## Git Workflow

- Main branch: `main`
- Deployment branch: `gh-pages` (auto-deployed to GitHub Pages)
- When working on gh-pages, changes are immediately reflected in production
