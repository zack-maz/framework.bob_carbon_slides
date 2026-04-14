# ADR-0005: Component-Based Slide Architecture

**Status**: Accepted  
**Date**: 2026-04-14  
**Deciders**: Development Team  
**Technical Story**: Slide implementation architecture

---

## Context

### Background

This slide deck application consists of multiple distinct slides, each with unique content, layouts, and interactive elements. The application needs an architecture that promotes code reusability, maintainability, and consistency while allowing for slide-specific customization.

### Problem Statement

We need an architecture that:
- Promotes code reusability across similar slides
- Maintains consistency in styling and behavior
- Allows for slide-specific customization
- Supports easy addition of new slides
- Enables independent development and testing of slides
- Provides clear separation of concerns
- Facilitates maintenance and updates

### Constraints

- Must work with React 18+
- Should integrate with Carbon Design System
- Must support React Router navigation
- Should allow for SCSS styling
- Must be maintainable by multiple developers
- Should support hot module replacement during development

---

## Decision

We will implement a **component-based architecture** where each slide is an independent React component, organized by type (slides, charts, layout), with shared layout components providing consistent structure and navigation.

### Rationale

1. **Separation of Concerns**: Each slide is self-contained with its own logic and styles
2. **Reusability**: Common patterns are extracted into shared components
3. **Maintainability**: Changes to one slide do not affect unrelated slides
4. **Testability**: Each component can be tested independently
5. **React Best Practices**: The approach aligns with React's component model
6. **Developer Experience**: Clear structure makes onboarding easier
7. **Scalability**: New slides can be added without changing the overall architecture
8. **Hot Reload**: Changes to individual slides reload quickly during development

---

## Consequences

### Positive Consequences

- **Modularity**: Each slide is independent and self-contained
- **Reusability**: Common components such as charts and layouts can be shared across slides
- **Maintainability**: It is easy to locate and modify a specific slide
- **Testability**: Components can be tested in isolation
- **Parallel Development**: Multiple developers can work on different slides
- **Clear Structure**: Organized directory structure makes project navigation easier
- **Type Safety**: Props can be typed for a better developer experience if TypeScript is added
- **Performance**: Code splitting is possible at the component level

### Negative Consequences

- **Boilerplate**: Each slide requires a component file and may also require a SCSS file
- **Prop Passing**: Some props may need to move through multiple levels of the tree
- **File Count**: More files must be managed as the presentation grows
- **Import Management**: Each new slide must be imported and registered

### Neutral Consequences

- **Bundle Size**: A component-based approach does not significantly affect bundle size by itself
- **Learning Curve**: Uses standard React patterns familiar to most React developers

---

## Alternatives Considered

### Alternative 1: Monolithic Slide Renderer

**Description**: A single component renders all slides based on configuration

**Pros**:
- Fewer files to manage
- Centralized slide logic
- Easier to share state

**Cons**:
- Massive component file
- Hard to maintain
- Difficult to test individual slides
- Poor separation of concerns
- Merge conflicts more likely
- Harder to understand over time

**Reason for Rejection**: Violates the single responsibility principle and would create an unmaintainable monolith.

### Alternative 2: Template-Based System

**Description**: Define slides as data templates and render them through a generic component

**Pros**:
- Very DRY
- Consistent structure
- Easy to add simple slides

**Cons**:
- Limited flexibility for complex slides
- Requires a template model or language
- Harder to customize individual slides
- Less type safety
- Does not fully leverage React's strengths

**Reason for Rejection**: Too restrictive for slides with unique layouts and interactions.

### Alternative 3: Class-Based Components

**Description**: Use React class components instead of functional components

**Pros**:
- Traditional React approach
- Lifecycle methods are explicit
- Familiar to some developers

**Cons**:
- More verbose
- Harder to share logic
- No hooks support
- Functional components are the modern React standard
- More boilerplate

**Reason for Rejection**: Functional components with hooks provide better reuse patterns and align with current React guidance.

### Alternative 4: Micro-Frontends

**Description**: Each slide is a separate micro-frontend application

**Pros**:
- Complete independence
- Different tech stacks possible
- Separate deployments possible

**Cons**:
- Massive overkill
- Complex infrastructure
- Shared state becomes difficult
- Performance overhead
- Deployment complexity

**Reason for Rejection**: Adds unnecessary complexity for a single presentation application.

---

## Implementation Notes

### Directory Structure

```text
src/components/
├── slides/              # Individual slide components
│   ├── IntroSlide.jsx
│   ├── ContentSlide.jsx
│   ├── ContentSlide.scss
│   └── ...
├── charts/              # Reusable chart components
│   ├── ChartComponent.jsx
│   └── ...
└── layout/              # Layout and navigation components
    ├── PresentationLayout.jsx
    ├── NavigationControls.jsx
    ├── TableOfContents.jsx
    └── SlideProgressBar.jsx
```

### Component Pattern

Each slide component follows a pattern like this:

```jsx
/**
 * SlideComponent - Brief description
 * @param {Object} props - Component props
 * @param {Function} props.onNavigate - Navigation function
 * @param {Array} props.sections - Section data
 * @returns {JSX.Element} The rendered slide
 */
function SlideComponent({ onNavigate, sections }) {
  return (
    <div className="slide-component">
      {/* Slide content */}
    </div>
  );
}

export default SlideComponent;
```

### Shared Props

All slide components receive:
- `onNavigate`: Function to navigate to a specific slide
- `sections`: Array of section data from [`slides.json`](adr/0005-component-based-architecture.md:164)

### Component Registration

Components must be registered in [`App.jsx`](adr/0005-component-based-architecture.md:169):

```javascript
import IntroSlide from './components/slides/IntroSlide';
// ... other imports

const slideComponents = {
  IntroSlide,
  // ... other components
};
```

### Styling Approach

- Each slide can have its own SCSS file
- SCSS files automatically receive Carbon mixins
- Carbon tokens are used for consistency
- Styles should remain component-scoped using a consistent naming convention such as BEM

### Migration Path

N/A - Initial implementation

### Validation

- ✅ Each slide renders independently
- ✅ Shared components work across multiple slides
- ✅ Navigation props are passed correctly
- ✅ Styles remain scoped to components
- ✅ Hot reload works for individual components
- ✅ Shared abstractions do not introduce unnecessary complexity

---

## References

- [React Component Documentation](https://react.dev/learn/your-first-component)
- [Thinking in React](https://react.dev/learn/thinking-in-react)
- [Component Composition](https://react.dev/learn/passing-props-to-a-component)
- Related ADRs: ADR-0001 (React Router), ADR-0002 (Carbon Design System), ADR-0004 (JSON Configuration)

---

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2026-04-14 | Development Team | Initial version |

---

## Notes

### Component Organization Principles

1. **Slides**: One component per slide, named after the slide's purpose
2. **Charts**: Reusable data visualization components
3. **Layout**: Shared layout and navigation components
4. **Common**: Truly generic components, if needed

### Best Practices

- Keep slide components focused on presentation logic
- Extract complex logic into custom hooks
- Use Carbon components for UI consistency
- Follow JSDoc commenting standards
- Keep components reasonably sized
- Extract repeated patterns into shared components when the abstraction clearly helps

### Adding a New Slide

1. Create the component file in [`src/components/slides/`](adr/0005-component-based-architecture.md:221)
2. Create a SCSS file if needed
3. Import the component in [`App.jsx`](adr/0005-component-based-architecture.md:223)
4. Add it to the [`slideComponents`](adr/0005-component-based-architecture.md:224) object
5. Add the corresponding entry to [`slides.json`](adr/0005-component-based-architecture.md:225)

### Shared Component Strategy

Extract to a shared component when:
- A pattern is used in three or more slides
- Complex logic benefits from isolation
- A data visualization is reusable
- A common layout pattern is repeated

Do not extract when:
- A pattern is used in only one or two slides
- Slide-specific customization is still dominant
- The abstraction adds more complexity than value