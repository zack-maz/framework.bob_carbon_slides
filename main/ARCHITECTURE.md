# Architecture Documentation

This document describes a generalized web-based slide deck application built with React, Vite, Carbon Design System, and route-driven navigation. The project was developed using MCP servers (Carbon MCP and Playwright MCP) to enhance development workflows.

---

## ⚠️ Prescriptive Architecture

**This framework is prescriptive, not descriptive.** The architecture documented here is mandatory for all implementations. These are not suggestions or examples—they are requirements.

### Architectural Mandates

1. **Carbon Design System (@carbon/react)** - Required for all UI
   - All components must use Carbon React components
   - All styling must use Carbon design tokens
   - Custom CSS-only implementations are rejected unless explicitly justified

2. **React Router with `/slide/:slideIndex` pattern** - Required for navigation
   - Each slide must be accessible via this route pattern
   - Alternative routing schemes are not permitted
   - Navigation must use centralized handlers, not direct route manipulation

3. **JSON-Driven Configuration (`slides.json`)** - Required for slide metadata
   - All slide definitions must be externalized to JSON
   - Hardcoded slide arrays or inline configuration are not permitted
   - Component registration must match JSON metadata exactly

4. **Component-Based Architecture** - Required structure
   - Each slide must be a separate component in `src/components/slides/`
   - Monolithic components combining multiple slides are not permitted
   - Component modularity and separation of concerns are mandatory

### Why This Matters

These constraints ensure:
- **Consistency** across all implementations
- **Maintainability** through clear separation of concerns
- **Accessibility** via Carbon's enterprise-ready patterns
- **Scalability** through modular component architecture

### Enforcement

Implementations that deviate from these patterns will be rejected in code review. Speed, prototyping, or "quick demos" are not valid reasons to bypass the architecture.

See:
- [`README.md`](../README.md#non-negotiables) for quick reference
- [`guides/code-standards.md`](../guides/code-standards.md) for implementation details
- [`adr/`](../adr/) for architectural decision records

---

## System Overview

The application is a single-page React presentation that renders one slide at a time using React Router and metadata from [`slides.json`](src/data/slides.json).

### Core stack

- React from [`package.json`](package.json:1)
- React Router from [`package.json`](package.json:1)
- Carbon React from [`package.json`](package.json:1)
- Carbon Charts from [`package.json`](package.json:1)
- Framer Motion from [`package.json`](package.json:1)
- Vite from [`package.json`](package.json:1)

## Runtime Structure

```text
App
└─ Theme
   └─ Router
      └─ Routes
         ├─ "/" -> redirect to first slide
         └─ "/slide/:slideIndex" -> PresentationView
                                      ├─ PresentationLayout
                                      └─ Active slide component
```

## Application Shell

### [`App`](src/App.jsx:1)

[`App`](src/App.jsx:1) is the application entry shell. In a typical implementation it:

- applies the Carbon theme
- sets up the router
- redirects `/` to the first slide route
- registers slide components used by the metadata-driven view

### [`PresentationView()`](src/App.jsx:1)

[`PresentationView()`](src/App.jsx:1) acts as the runtime controller for the deck:

- reads the route param
- converts the route value to a slide index
- resolves the active slide record from [`slidesData.slides`](src/data/slides.json:1)
- looks up the renderable component from a slide component registry
- passes navigation and section metadata into the shared layout

### Shared layout

A layout component such as [`PresentationLayout`](src/components/layout/PresentationLayout.jsx:1) typically provides:

- global header or presentation frame
- table of contents or menu access
- fullscreen toggle
- content region for the active slide
- previous and next controls
- progress indicators
- modal or overlay support

## Routing Model

The project uses route-driven navigation so every slide can be addressed directly.

Typical characteristics:

- each slide is accessible through a route such as `/slide/1`
- browser history works naturally
- slides can be bookmarked or shared
- route changes trigger re-rendering of the active slide

### Important implementation note

Projects may use either 1-based or 0-based route values. The documentation in this repository assumes the common presentation-friendly pattern of 1-based URLs, with route values converted internally to array indexes.

## Slide Registration Model

The deck is configuration-driven and depends on synchronization between metadata and component registration.

### Slide metadata

Slides and section groupings live in [`src/data/slides.json`](src/data/slides.json).

Each slide entry commonly includes:

- `id`
- `component`
- `title`
- `section`
- `duration`

### Component mapping

Slide components are imported and registered in the application shell, often in a structure like `slideComponents`.

### Rendering rule

The `component` string in [`slides.json`](src/data/slides.json:1) must exactly match the registered component key used by the application. If not, the app should fall back to an explicit error or “slide not found” state.

## Navigation Architecture

### Wrapper-based navigation

Recommended navigation flow:

```text
User action
-> onNavigate(index)
-> goToSlide(index)
-> navigate(route)
-> route update
-> slide re-render
```

Slide components should use a passed navigation handler such as `onNavigate()` instead of mutating routes directly. This keeps navigation behavior centralized.

### Keyboard navigation

A hook such as [`useKeyboardNavigation`](src/hooks/useKeyboardNavigation.js:1) usually wires global keyboard controls:

- `ArrowRight`, `Space`, `PageDown` -> next slide
- `ArrowLeft`, `PageUp` -> previous slide
- `Home` -> first slide
- `End` -> last slide

A robust implementation should ignore these shortcuts while the user is focused inside editable controls such as inputs or textareas.

### Layout controls

The shared layout is usually responsible for:

- determining whether previous and next navigation is available
- rendering the navigation UI
- coordinating overlays such as a table of contents

## Styling System

### Global CSS

[`main.jsx`](src/main.jsx:1) typically imports a global stylesheet such as [`global.css`](src/assets/styles/global.css:1), which provides:

- Carbon base styles
- Carbon Charts styles
- shared layout rules
- reusable presentation utilities

### SCSS token injection

[`vite.config.js`](vite.config.js:1) can inject Carbon SCSS modules automatically so component-level styles can use spacing, theme, and typography tokens without repeated boilerplate imports.

### Theme

The application theme is usually applied centrally through Carbon’s theme provider. Teams can choose whichever Carbon theme best fits their presentation environment.

## Content and Visualization Architecture

Slide decks often combine text, layout components, and data visualizations.

Common patterns include:

- chart components rendered inside slide components
- shared card and grid structures for repeated content patterns
- reusable layout primitives for slide consistency
- optional animation layers for transitions and emphasis

If a slide uses external JSON content or chart data, the relationship between that data and the rendering component should be documented in [data/data-lineage.md](data/data-lineage.md).

## Build and Tooling

### Vite configuration

[`vite.config.js`](vite.config.js:1) commonly defines:

- the React plugin
- path aliases
- SCSS preprocessing behavior
- development server settings
- build output configuration

### Package scripts

[`package.json`](package.json:1) typically provides:

- a development server script
- a production build script
- a local preview script

## Data and Configuration Sources

Primary configuration and content sources commonly include:

- [`src/data/slides.json`](src/data/slides.json) for slide metadata and sections
- [`src/assets/styles/global.css`](src/assets/styles/global.css) for global styling
- [`vite.config.js`](vite.config.js) for aliases and build behavior
- [`package.json`](package.json) for scripts and dependencies

## Adding or Replacing a Slide

To update the deck safely:

1. add the component under `src/components/slides/`
2. import it into the application shell
3. register it in the slide component mapping
4. add or update the matching record in [`slides.json`](src/data/slides.json)

If either the import, registry entry, or metadata record is missing, the slide will not render correctly.

## Operational Considerations

Key details that documentation should keep current:

- whether routes are 1-based or 0-based
- where slide metadata is stored
- how navigation handlers are expected to work
- which shared layout component wraps slides
- how styling tokens and global styles are loaded
- how charts or content data are sourced
- what local and production build commands are supported

## Related Docs

- [README.md](../README.md)
- [INDEX.md](INDEX.md)
- [HANDOFF_DOCUMENTATION.md](../HANDOFF_DOCUMENTATION.md)
- [guides/getting-started.md](guides/getting-started.md)
- [guides/code-standards.md](guides/code-standards.md)
- [data/data-dictionary.md](data/data-dictionary.md)
- [data/data-lineage.md](data/data-lineage.md)
