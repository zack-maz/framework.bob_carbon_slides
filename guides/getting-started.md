# Getting Started Guide

**Purpose**: Quick start guide for developers joining a slide deck project built with React, Vite, Carbon, and Bob.  
**Audience**: New developers, contributors  
**Last Updated**: 2026-04-14

---

## Welcome

This guide helps you get a generalized slide-building project running locally and explains the core concepts you need before making changes.

The workflow documented here assumes:

- slide content is implemented as React components
- slide order and metadata are stored in JSON
- Vite is used for local development and production builds
- Carbon provides UI components, tokens, and theming
- Bob can be used to accelerate implementation and iteration
- browser validation can be done manually or with Playwright
- MCP servers (Carbon MCP and Playwright MCP) enhance development workflows

---

## Prerequisites

Before you begin, make sure you have:

- **Node.js** 18 or higher
- **npm** 9 or higher
- **Git**
- **VS Code** or another editor of your choice
- Working familiarity with React, JavaScript, and SCSS

---

## Quick Start

```bash
git clone <repository-url>
cd slides
npm install
npm run dev
```

After startup, open the local app at `http://localhost:3000` if it does not open automatically.

Expected result:

- the app loads successfully
- the first slide renders
- navigation works
- changes hot reload during development

---

## Project Structure

```text
slides/
├── src/
│   ├── App.jsx                 # Main app shell and route wiring
│   ├── main.jsx                # Entry point
│   ├── components/
│   │   ├── slides/            # Slide components
│   │   ├── charts/            # Optional chart components
│   │   └── layout/            # Shared layout components
│   ├── data/                  # JSON configuration and content data
│   ├── hooks/                 # Reusable React hooks
│   └── assets/                # Styles and static assets
├── package.json
└── vite.config.js
```

---

## Core Concepts

### Slides are React components

Each slide is usually implemented as a component in `src/components/slides/`.

```jsx
function MySlide({ onNavigate, sections }) {
  return (
    <div className="my-slide">
      <h1>My Slide Title</h1>
      <p>Content here</p>
    </div>
  );
}

export default MySlide;
```

### Slides are configured in JSON

Slide order and metadata are usually stored in [`slides.json`](../src/data/slides.json).

```json
{
  "slides": [
    {
      "id": "my-slide",
      "component": "MySlide",
      "title": "My Slide",
      "section": "overview",
      "duration": 60
    }
  ]
}
```

### Components must be registered

Slide components must be imported and registered in the application shell.

```javascript
import MySlide from './components/slides/MySlide';

const slideComponents = {
  MySlide,
};
```

The `component` value in [`slides.json`](../src/data/slides.json:1) must match the registered component key exactly.

---

## Common Tasks

### Add a new slide

1. Create a component file in `src/components/slides/`
2. Implement the component
3. Register it in [`App.jsx`](../src/App.jsx)
4. Add metadata in [`slides.json`](../src/data/slides.json)
5. Test navigation and rendering

Example:

```jsx
function NewSlide({ onNavigate, sections }) {
  return <div className="new-slide">Content</div>;
}

export default NewSlide;
```

```json
{
  "id": "new-slide",
  "component": "NewSlide",
  "title": "New Slide",
  "section": "overview",
  "duration": 60
}
```

### Update data

Edit JSON files under `src/data/` and verify the affected slide or visualization renders correctly after reload.

### Style a slide

Create a companion SCSS file when needed:

```scss
.my-slide {
  padding: $spacing-07;

  h1 {
    @include type-style('productive-heading-05');
  }
}
```

Import the SCSS file into the related component.

---

## Development Workflow

### 1. Start the dev server

```bash
npm run dev
```

Expected behavior:

- server starts on port 3000 unless configured otherwise
- browser opens automatically if configured
- hot reload is enabled

### 2. Make changes

- update components in `src/`
- update metadata in `src/data/`
- check the browser and console continuously

### 3. Validate changes

Confirm that:

- targeted slides render correctly
- keyboard navigation still works
- direct slide URLs work
- styles are applied as expected
- no new console errors appear

### 4. Commit your work

```bash
git add .
git commit -m "feat(slides): add new slide"
git push
```

---

## Using Path Aliases

Prefer path aliases over long relative paths.

```javascript
// Avoid
import Slide from '../../../components/slides/IntroSlide';

// Prefer
import Slide from '@components/slides/IntroSlide';
import data from '@data/slides.json';
import { useKeyboardNavigation } from '@hooks/useKeyboardNavigation';
```

Typical aliases include:

- `@/` → `src/`
- `@components/` → `src/components/`
- `@data/` → `src/data/`
- `@assets/` → `src/assets/`
- `@hooks/` → `src/hooks/`

---

## Carbon Design System Basics

### Use Carbon components where appropriate

```jsx
import { Button, Grid, Column } from '@carbon/react';

function MySlide() {
  return (
    <Grid>
      <Column lg={8}>
        <Button>Click me</Button>
      </Column>
    </Grid>
  );
}
```

### Use Carbon tokens and mixins

```scss
.my-component {
  padding: $spacing-05;
  background: $background;
  @include type-style('body-01');
}
```

### Theme guidance

Many slide deck projects use a Carbon dark theme such as `g90`, but the exact theme can vary based on the presentation environment and project goals.

---

## Working with Bob

Bob is useful for:

- generating or refactoring slide components
- updating metadata and documentation
- reviewing consistency across files
- helping validate workflows before or after changes

A typical Bob-assisted workflow is:

1. define the slide or documentation change
2. update component and metadata files
3. review behavior in the browser
4. capture follow-up improvements
5. update docs if the pattern or workflow changed

---

## Validation with Browser Tooling and Playwright

Use browser validation to confirm:

- slide routes load correctly
- keyboard navigation works
- modal overlays behave correctly
- layout remains intact at different viewport sizes
- charts and visual components render without errors

Playwright is useful for repeatable checks once the slide deck reaches a stable structure. The Playwright MCP server was used during development to streamline browser automation tasks.

---

## Troubleshooting

### Port 3000 is already in use

On Windows:

```bash
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

On macOS or Linux:

```bash
lsof -ti:3000 | xargs kill -9
```

### Module not found

Try reinstalling dependencies:

```bash
rm -rf node_modules package-lock.json
npm install
```

### Styles are not loading

Verify required styling dependencies are installed:

```bash
npm list sass
npm install sass --save-dev
```

### Hot reload is not working

Restart the dev server:

```bash
npm run dev
```

Also check for syntax errors or invalid imports.

---

## Next Steps

After setup:

1. Read [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
2. Review the ADRs in [adr/](../adr/)
3. Review [code-standards.md](code-standards.md)
4. Review [contributing.md](contributing.md)
5. Review [runbooks/development-setup.md](../runbooks/development-setup.md)

---

## Useful Commands

```bash
npm run dev
npm run build
npm run preview
npm install
npm update
npm audit
git status
git add .
git commit -m "msg"
git push
```

---

## Getting Help

- [main/INDEX.md](../main/INDEX.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- [runbooks/troubleshooting.md](../runbooks/troubleshooting.md)
- [guides/code-standards.md](code-standards.md)

---

## Resources

- [React Documentation](https://react.dev/)
- [Carbon Design System](https://carbondesignsystem.com/)
- [Vite Documentation](https://vitejs.dev/)
- [React Router Documentation](https://reactrouter.com/)
- [Playwright Documentation](https://playwright.dev/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)

---

**Last Updated**: 2026-04-14