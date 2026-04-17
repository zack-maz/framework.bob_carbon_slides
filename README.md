# Slide Builder Documentation Framework

> A comprehensive documentation framework for building web-based slide decks with Bob using React, Vite, Carbon Design System, and Playwright.

## About This Framework

This repository provides a complete, reusable documentation structure for React-based slide deck projects. It's designed to be cloned, customized, and maintained as a living documentation set that evolves with your project.

### What Makes This a Framework

- **Structured Documentation System**: Complete architecture with ADRs, runbooks, guides, diagrams, and data documentation
- **Generalized & Reusable**: Designed to support multiple projects rather than a single implementation
- **Best Practices Built-in**: Includes maintenance procedures, review schedules, and cross-referencing methodology
- **Framework Organization**: Clear taxonomy with standardized document types and consistent structure

---

## Non-Negotiables

This framework is **prescriptive, not descriptive**. The following architectural decisions are mandatory and must be followed in all implementations:

### Required Technologies
- **@carbon/react** - All UI components must use Carbon Design System components and tokens
- **React Router** - Navigation must use the route pattern `/slide/:slideIndex`
- **JSON Configuration** - Slide definitions must be maintained in `slides.json`
- **Component Architecture** - Each slide must be implemented as a separate component under `src/components/slides/`

### Rejected Patterns
- ❌ Plain custom CSS-only prototypes (unless explicitly requested for a specific use case)
- ❌ Alternative routing patterns that bypass `/slide/:slideIndex`
- ❌ Hardcoded slide arrays or non-JSON configuration
- ❌ Monolithic slide components that combine multiple slides

### Why These Are Non-Negotiable

These constraints are documented in:
- [`adr/0001-use-react-router.md`](adr/0001-use-react-router.md) - Route-based navigation
- [`adr/0002-carbon-design-system.md`](adr/0002-carbon-design-system.md) - Carbon Design System usage
- [`adr/0004-json-slide-configuration.md`](adr/0004-json-slide-configuration.md) - JSON-driven configuration
- [`adr/0005-component-based-architecture.md`](adr/0005-component-based-architecture.md) - Component structure
- [`guides/code-standards.md`](guides/code-standards.md) - Implementation standards

**If you're building quickly or prototyping**, you must still follow these patterns. Speed is not a valid reason to bypass the architecture. The framework is designed to be fast to work with *while* maintaining these standards.

---

## Getting Started with This Framework

### Clone and Customize

```bash
# Clone this repository
git clone https://github.com/zack-maz/framework.bob_carbon_slides.git
cd framework.bob_carbon_slides

# Customize for your project
# 1. Update project-specific details in README.md
# 2. Modify guides and runbooks to match your workflow
# 3. Update ADRs as you make architectural decisions
# 4. Keep documentation in sync with your implementation
```

---

**Status:** Active  
**Audience:** Developers, maintainers, and presentation builders  
**Documentation Scope:** Project overview, workflow, architecture references, and maintenance guidance

---

## Overview

This repository contains documentation for a slide-building workflow centered on:

- **React** for component-driven slide development
- **Vite** for fast local development and production builds
- **Carbon Design System** for accessible, enterprise-ready UI patterns
- **Playwright** for verification and browser-based testing workflows
- **Bob** for guided implementation, iteration, and documentation support
- **MCP Servers** (Carbon MCP and Playwright MCP) for enhanced development workflows

The documentation has been generalized so it can support many kinds of slide decks rather than a single branded presentation.

---

## Quick Start

```bash
npm install
npm run dev
npm run build
npm run preview
```

Default local development URL: `http://localhost:3000`

---

## What This Documentation Covers

- Creating and organizing slide components
- Managing slide metadata in [`slides.json`](src/data/slides.json)
- Navigating slides with React Router
- Applying Carbon styling and design tokens
- Validating behavior with browser workflows and Playwright
- Maintaining the project with runbooks, ADRs, and supporting diagrams

---

## Core Concepts

### Component-Based Slides

Each slide is implemented as an individual React component, typically under `src/components/slides/`. This keeps presentation content modular and easy to update.

### JSON-Driven Configuration

Slide order, titles, section membership, and duration metadata are stored in [`slides.json`](src/data/slides.json). The app uses this file to determine which component to render for each route.

### Route-Based Navigation

Slides are navigated using React Router. Each slide has a route such as `/slide/1`, enabling:

- direct links to a specific slide
- browser back/forward navigation
- easier testing and review

### Carbon-First UI

The project uses Carbon for layout, typography, spacing, theming, and common interface elements. This provides a consistent system for building polished slide decks without inventing a custom design language from scratch.

### Browser Validation

Playwright and browser-based workflows are used to verify:

- slide rendering
- navigation behavior
- keyboard interactions
- regressions after content or component updates

---

## Typical Project Structure

```text
slides/
├── src/
│   ├── components/
│   │   ├── slides/          # Slide components
│   │   ├── layout/          # Shared layout and navigation
│   │   └── charts/          # Visualization components
│   ├── data/
│   │   ├── slides.json      # Slide metadata and ordering
│   │   └── charts.json      # Optional chart data
│   ├── assets/
│   │   └── styles/          # Global styles and SCSS
│   ├── hooks/               # Reusable hooks
│   ├── App.jsx              # Router and slide registration
│   └── main.jsx             # Application entry point
├── public/
├── package.json
├── vite.config.js
└── index.html
```

---

## Technology Stack

| Technology | Purpose |
|------------|---------|
| React | Slide and layout components |
| Vite | Local development and production builds |
| React Router | URL-based slide navigation |
| Carbon Design System | UI components, tokens, and theming |
| Carbon Charts | Optional data visualization |
| Framer Motion | Optional animation support |
| Playwright | Browser automation and verification |
| Bob | Guided implementation and iteration |
| Carbon MCP Server | MCP integration for Carbon Design System workflows |
| Playwright MCP Server | MCP integration for browser automation |

---

## Building Slides

### 1. Create a slide component

Example structure:

```jsx
export default function NewSlide({ onNavigate, sections }) {
  return (
    <div className="slide-container">
      <h1>New Slide</h1>
    </div>
  );
}
```

### 2. Register the component

Add the component import and mapping in [`App.jsx`](src/App.jsx).

### 3. Add metadata

Add a matching entry to [`slides.json`](src/data/slides.json):

```json
{
  "id": "new-slide",
  "component": "NewSlide",
  "title": "New Slide",
  "section": "overview",
  "duration": 90
}
```

**Important:** The `component` value in [`slides.json`](src/data/slides.json:1) must exactly match the key used in the component registry in [`App.jsx`](src/App.jsx:1).

---

## Navigation Model

### Keyboard shortcuts

Typical keyboard support includes:

- `ArrowRight`, `Space`, `PageDown` → next slide
- `ArrowLeft`, `PageUp` → previous slide
- `Home` → first slide
- `End` → last slide
- `Escape` → close open overlays or modals

### Navigation guidance

Slides should use the shared `onNavigate()` prop rather than modifying routes directly. This keeps behavior centralized and consistent with layout controls and keyboard navigation.

---

## Styling Guidance

### Carbon usage

Recommended practices:

- prefer Carbon components for layout and controls
- use Carbon spacing and typography tokens
- keep theming consistent across all slides
- use a shared presentation layout shell for navigation and framing

### SCSS and global styles

Shared styles usually live in [`src/assets/styles/global.css`](src/assets/styles/global.css) and related SCSS files configured through [`vite.config.js`](vite.config.js).

---

## Validation Workflow

Use browser automation and manual review to confirm that:

- every slide renders without errors
- routing works for direct URLs
- keyboard navigation behaves correctly
- layout elements remain consistent
- charts and visual content load correctly

Playwright is especially useful for repeatable checks after structural or styling changes. The Playwright MCP server was used during development to streamline browser automation workflows.

---

## Deployment Model

Because the app is a static React SPA, common deployment options include:

- local preview with `npm run preview`
- static hosting providers
- GitHub Pages
- object storage plus CDN
- standard web servers serving the `dist/` directory

See [`runbooks/deployment.md`](runbooks/deployment.md) for detailed deployment guidance.

---

## Documentation Map

- [`main/INDEX.md`](main/INDEX.md) — central documentation hub
- [`main/ARCHITECTURE.md`](main/ARCHITECTURE.md) — architecture overview
- [`HANDOFF_DOCUMENTATION.md`](HANDOFF_DOCUMENTATION.md) — maintenance and ownership guidance
- [`guides/getting-started.md`](guides/getting-started.md) — onboarding guide
- [`guides/contributing.md`](guides/contributing.md) — contribution workflow
- [`guides/code-standards.md`](guides/code-standards.md) — coding standards
- [`runbooks/development-setup.md`](runbooks/development-setup.md) — setup procedures
- [`runbooks/troubleshooting.md`](runbooks/troubleshooting.md) — issue diagnosis and recovery
- [`runbooks/maintenance.md`](runbooks/maintenance.md) — recurring maintenance tasks

---

## Recommended Workflow with Bob

A practical build loop looks like this:

1. Define the slide goal and content structure
2. Create or update React slide components
3. Add or revise metadata in [`slides.json`](src/data/slides.json)
4. Apply Carbon-based layout and styling
5. Run the app locally with Vite
6. Validate interactions and rendering in the browser
7. Use Playwright or browser tooling for repeatable verification
8. Update documentation when architecture or procedures change

---

## Standard Git Workflow

```bash
# Create a feature branch
git checkout -b feature/your-feature-name

# Make your changes and stage them
git add .

# Commit with a descriptive message
git commit -m "feat: add new slide component documentation"

# Push to remote
git push origin feature/your-feature-name

# Create a pull request on GitHub for review
```

## Commit Message Conventions

Follow conventional commits for clarity:

- `feat:` - New features or documentation sections
- `fix:` - Bug fixes or documentation corrections
- `docs:` - Documentation-only changes
- `refactor:` - Restructuring without changing functionality
- `chore:` - Maintenance tasks

Example:
```bash
git commit -m "feat(guides): add browser validation workflow"
git commit -m "fix(adr): correct React Router decision rationale"
git commit -m "docs(readme): update quick start instructions"
```

---

## License and Ownership

This documentation is intentionally generalized for reusable slide-building workflows. Replace ownership, licensing, and team-specific details with your own project conventions when adopting it elsewhere.

---

## Acknowledgments

Built around:

- [React](https://react.dev/)
- [Vite](https://vitejs.dev/)
- [Carbon Design System](https://carbondesignsystem.com/)
- [Playwright](https://playwright.dev/)
- Bob-assisted implementation workflows
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

---

*Last Updated: 2026-04-17*
