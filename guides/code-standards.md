# Code Standards Guide

**Purpose**: Coding standards and implementation best practices for a generalized slide deck project built with React, Vite, Carbon, and JSON-driven slide metadata.  
**Last Updated**: 2026-04-14

---

## Overview

This guide defines coding standards to keep the codebase consistent, maintainable, and easy to evolve.

---

## ⚠️ Architectural Requirements (Non-Negotiable)

**These are not suggestions—they are requirements.** The following architectural patterns are mandatory for all implementations:

### Required Architecture
1. **@carbon/react** - All UI components must use Carbon Design System
   - Use Carbon components for layout, controls, and UI primitives
   - Use Carbon design tokens for spacing, colors, and typography
   - Do not create custom CSS-only UI components that duplicate Carbon functionality

2. **React Router with `/slide/:slideIndex`** - Navigation must follow this pattern
   - Each slide must be accessible via `/slide/1`, `/slide/2`, etc.
   - Do not use alternative routing schemes or hardcoded navigation
   - Use the shared `onNavigate()` prop for navigation logic

3. **JSON Configuration (`slides.json`)** - Slide metadata must be externalized
   - All slide definitions must be in `slides.json`
   - Do not hardcode slide arrays or configuration in components
   - The `component` field must exactly match the registered component key

4. **Component-Based Architecture** - One component per slide
   - Each slide must be a separate component in `src/components/slides/`
   - Do not create monolithic components that combine multiple slides
   - Keep slide components focused and modular

### Rejected Patterns
- ❌ Plain custom CSS-only prototypes (unless explicitly requested)
- ❌ Alternative routing patterns that bypass `/slide/:slideIndex`
- ❌ Hardcoded slide configuration outside JSON
- ❌ Monolithic slide components
- ❌ Custom UI primitives that duplicate Carbon components

### Enforcement
These requirements are documented in:
- [`adr/0001-use-react-router.md`](../adr/0001-use-react-router.md)
- [`adr/0002-carbon-design-system.md`](../adr/0002-carbon-design-system.md)
- [`adr/0004-json-slide-configuration.md`](../adr/0004-json-slide-configuration.md)
- [`adr/0005-component-based-architecture.md`](../adr/0005-component-based-architecture.md)

**Code reviews will reject implementations that violate these patterns.** Speed or prototyping are not valid reasons to bypass the architecture.

---

## Project Assumptions

The project assumptions behind these standards are:

- slides are implemented as React components
- slide order and metadata are configuration-driven
- Carbon is the primary design system
- Vite is the build tool
- route-based navigation is used for slide access

---

## General Principles

1. **Clarity over cleverness**
2. **Consistency over personal preference**
3. **Small, focused changes**
4. **Document non-obvious decisions**
5. **Validate behavior after every meaningful change**

---

## JavaScript and React Standards

### Component structure

Recommended structure:

```jsx
import { useState } from 'react';
import { Button } from '@carbon/react';
import './MyComponent.scss';

/**
 * MyComponent - Brief description of the component.
 * @param {Object} props - Component props.
 * @param {Function} props.onNavigate - Shared navigation function.
 * @param {Array} props.sections - Section metadata.
 * @returns {JSX.Element}
 */
function MyComponent({ onNavigate, sections }) {
  const [isOpen, setIsOpen] = useState(false);

  const handleClick = () => {
    setIsOpen((current) => !current);
  };

  return (
    <div className="my-component">
      <Button onClick={handleClick}>Toggle</Button>
    </div>
  );
}

export default MyComponent;
```

### Naming conventions

| Type | Convention | Example |
|------|------------|---------|
| Components | PascalCase | `IntroSlide`, `ChartComponent` |
| Functions | camelCase | `goToSlide`, `handleClick` |
| Variables | camelCase | `currentIndex`, `totalSlides` |
| Constants | UPPER_SNAKE_CASE | `FIRST_SLIDE_INDEX` |
| Component files | PascalCase | `IntroSlide.jsx` |
| Utility files | kebab-case or existing project convention | `use-keyboard-navigation.js` |
| CSS classes | kebab-case or BEM | `title-slide`, `slide-card__title` |
| JSON files | kebab-case | `slides.json`, `chart-data.json` |

### Functional components only

Prefer function components with hooks.

```jsx
// Preferred
function MyComponent({ title }) {
  return <div>{title}</div>;
}
```

Avoid introducing class components unless required by legacy constraints.

### Destructure props

Destructure component props in the function signature when practical.

```jsx
function MyComponent({ onNavigate, sections }) {
  return <div>{sections.length}</div>;
}
```

### JSDoc guidance

Use JSDoc for exported functions, utilities, and any component whose expected props or purpose are not obvious.

```jsx
/**
 * Calculates the next slide index.
 * @param {number} currentIndex - Current slide index.
 * @param {number} totalSlides - Total number of slides.
 * @returns {number}
 */
function getNextSlide(currentIndex, totalSlides) {
  return (currentIndex + 1) % totalSlides;
}
```

### Import organization

Recommended order:

```jsx
// 1. React imports
import { useEffect, useState } from 'react';

// 2. Third-party packages
import { Button, Grid } from '@carbon/react';

// 3. Internal imports via aliases
import slidesData from '@data/slides.json';
import { useKeyboardNavigation } from '@hooks/useKeyboardNavigation';

// 4. Same-folder relative imports
import './MyComponent.scss';
```

### Path aliases

Prefer aliases over long relative imports.

```jsx
// Avoid
import IntroSlide from '../../../components/slides/IntroSlide';

// Prefer
import IntroSlide from '@components/slides/IntroSlide';
```

---

## Slide-Specific Standards

### Keep slide components focused

A slide component should primarily be responsible for:

- rendering the slide
- handling slide-local interactions
- using shared navigation or section metadata when needed

Extract shared patterns when the same structure appears repeatedly across multiple slides.

### Register components correctly

If a slide is configuration-driven:

1. create the component
2. import it into the application shell
3. register it in the slide mapping
4. add a matching record to [`slides.json`](../src/data/slides.json)

The component key and metadata `component` field must match exactly.

### Prefer passed navigation handlers

Use shared handlers such as `onNavigate()` instead of mutating routes directly inside slides, unless the project explicitly documents a different pattern.

---

## SCSS Standards

### File organization

Keep styles close to components when they are component-specific.

```scss
.my-component {
  display: flex;
  padding: $spacing-05;
  background: $background;
  @include type-style('body-01');

  &__header {
    margin-bottom: $spacing-03;
  }

  &--highlighted {
    background: $background-selected;
  }

  @media (max-width: 768px) {
    padding: $spacing-03;
  }
}
```

### Use Carbon tokens

Prefer Carbon tokens and mixins over hard-coded values.

```scss
// Preferred
.my-component {
  padding: $spacing-05;
  background: $background;
  color: $text-primary;
  @include type-style('body-01');
}
```

Avoid introducing fixed values where Carbon tokens already express the intent.

### Keep selectors shallow

Avoid deep nesting when flatter selectors are clearer and easier to maintain.

### Use consistent class naming

BEM or a clear kebab-case approach is preferred. Choose one pattern and keep it consistent within the project.

---

## Carbon Design System Guidance

### Prefer Carbon components

Use Carbon components for layout and controls when they meet the requirement.

```jsx
import { Button, Column, Grid } from '@carbon/react';

function MyComponent() {
  return (
    <Grid>
      <Column lg={8}>
        <Button>Click me</Button>
      </Column>
    </Grid>
  );
}
```

### Use Carbon patterns consistently

- use Carbon Grid for layout when appropriate
- use Carbon spacing tokens
- use Carbon typography mixins
- use Carbon color tokens
- keep theme usage centralized

### Avoid unnecessary custom UI primitives

Do not rebuild generic buttons, grids, or input patterns unless Carbon does not satisfy the requirement.

---

## Code Quality Guidance

### Keep functions small

Prefer small functions with one clear responsibility.

```jsx
function validateSlideIndex(index, totalSlides) {
  return index >= 0 && index < totalSlides;
}
```

### Avoid magic numbers

Use named constants for values with meaning.

```jsx
const FIRST_SLIDE_INDEX = 0;
const DEFAULT_ANIMATION_DURATION_MS = 300;
```

### Use descriptive names

Names should explain purpose, not just implementation detail.

```jsx
const currentSlideIndex = parseInt(slideIndex, 10) - 1;
const isFirstSlide = currentSlideIndex === 0;
```

### Handle errors deliberately

Use clear fallback behavior for parsing, loading, or mapping failures.

```jsx
function loadSlideData(rawValue) {
  try {
    return JSON.parse(rawValue);
  } catch (error) {
    console.error('Failed to parse slide data:', error);
    return null;
  }
}
```

---

## File Organization

### Component files

Common patterns:

```text
MyComponent/
├── MyComponent.jsx
├── MyComponent.scss
└── MyComponent.test.jsx
```

or for simpler components:

```text
MyComponent.jsx
MyComponent.scss
```

### File length guidance

Prefer approximate limits like:

- components: 300 lines or less
- utilities: 200 lines or less

If a file becomes hard to scan, split responsibilities into smaller pieces.

---

## Accessibility Standards

### Use semantic HTML

Prefer semantic structure where possible.

```jsx
<nav>
  <button onClick={handleClick}>Next</button>
</nav>
```

### Label interactive elements

Buttons and controls should have clear visible text or accessible labels.

```jsx
<button aria-label="Navigate to next slide">Next</button>
```

### Preserve keyboard usability

- all interactive elements should be reachable by keyboard
- focus states should remain visible
- keyboard shortcuts should not interfere with text entry fields

---

## Performance Guidance

### Avoid unnecessary re-renders

Memoize expensive calculations or components when the performance gain is real and the code remains understandable.

```jsx
const sortedData = useMemo(() => {
  return [...data].sort((a, b) => a.value - b.value);
}, [data]);
```

### Be intentional with media and charts

- use appropriately sized images
- avoid unnecessary data transformations inside render paths
- extract repeated chart logic into shared helpers or wrappers

---

## Testing Checklist

Before committing, confirm:

- [ ] code follows project standards
- [ ] no new console errors were introduced
- [ ] targeted slides render correctly
- [ ] navigation works through keyboard and UI
- [ ] responsive behavior remains acceptable
- [ ] accessibility has not regressed
- [ ] documentation is updated when required

---

## Code Review Checklist

When reviewing code, check for:

- [ ] alignment with documented patterns
- [ ] readable naming and structure
- [ ] appropriate Carbon usage
- [ ] safe handling of errors and edge cases
- [ ] reasonable performance characteristics
- [ ] accessibility considerations
- [ ] documentation updates when needed

---

## Git Commit Standards

### Commit message format

```text
<type>(<scope>): <subject>
```

### Common types

- `feat`
- `fix`
- `docs`
- `style`
- `refactor`
- `test`
- `chore`

### Good examples

```text
feat(slides): add summary slide
fix(navigation): ignore arrow keys inside text inputs
docs(architecture): clarify slide registration workflow
```

---

## Working with Bob

When using Bob to help with implementation:

- reference the exact files or workflows being changed
- keep requests narrow and concrete
- update docs when Bob-assisted changes affect architecture or procedure
- validate generated changes in the browser and through the build

---

## Resources

- [React Documentation](https://react.dev/)
- [Carbon Design System](https://carbondesignsystem.com/)
- [Vite Documentation](https://vitejs.dev/)
- [React Router Documentation](https://reactrouter.com/)
- [SCSS Guidelines](https://sass-guidelin.es/)

---

**Last Updated**: 2026-04-17