# ADR-0002: Adopt Carbon Design System

**Status**: Accepted  
**Date**: 2026-04-14  
**Deciders**: Development Team  
**Technical Story**: UI framework and design system selection

---

## Context

### Background

This slide deck application requires a professional, consistent, and accessible user interface. The project needs a design system that supports presentation-quality layouts, reusable interface patterns, and integrated data visualization while remaining easy to style within a React and Vite workflow.

### Problem Statement

We need a design system that:
- Provides enterprise-grade UI components
- Ensures visual consistency across all slides
- Supports accessibility standards (WCAG 2.1 AA)
- Offers comprehensive theming capabilities
- Includes data visualization components for charts
- Has excellent React integration

### Constraints

- Must support React 18+
- Should provide dark theme support
- Must include chart and data visualization components
- Should be actively maintained
- Must have good documentation
- Should support SCSS for styling customization

---

## Decision

We will adopt **Carbon Design System** (v1.59.0+) as our primary UI framework, including:
- `@carbon/react` for UI components
- `@carbon/charts-react` for data visualizations
- `@carbon/icons-react` for iconography
- Carbon's [`g90`](adr/0002-carbon-design-system.md:42) dark theme as the default presentation theme

### Rationale

1. **Comprehensive Component Library**: Carbon provides a rich set of production-quality components such as buttons, grids, modals, tabs, and forms
2. **Data Visualization**: Carbon Charts integrates well with Carbon UI and supports common presentation use cases
3. **Accessibility**: Carbon is designed with accessibility in mind and supports WCAG 2.1 AA standards
4. **Dark Theme Support**: The [`g90`](adr/0002-carbon-design-system.md:42) theme works well in presentation settings
5. **React Integration**: First-class React support aligns with the application's architecture
6. **Active Maintenance**: Carbon is actively maintained and broadly documented
7. **Design Tokens**: The token system supports consistent spacing, colors, typography, and motion

---

## Consequences

### Positive Consequences

- **Professional Appearance**: The application benefits from a polished, presentation-ready UI
- **Consistency**: Design tokens help keep layouts and spacing consistent across slides
- **Accessibility**: Carbon includes built-in accessibility patterns and keyboard support
- **Productivity**: Pre-built components accelerate development
- **Theming**: Theme switching and customization are straightforward
- **Documentation**: Carbon provides strong documentation and live examples
- **Chart Integration**: Carbon Charts fits naturally into the same visual system

### Negative Consequences

- **Bundle Size**: Carbon adds a meaningful amount of UI and styling code to the bundle
- **Learning Curve**: The team needs familiarity with Carbon's component APIs and styling patterns
- **Opinionated Design**: Carbon encourages certain layout and interaction conventions
- **Update Dependency**: The project must keep up with Carbon releases
- **Build Configuration**: SCSS preprocessing and Vite configuration are required

### Neutral Consequences

- **Design System Adoption**: The application becomes aligned to Carbon conventions
- **Component Coverage**: Some niche use cases may still need custom components
- **Theme Lock-in**: Switching to a different design system later would require rework

---

## Alternatives Considered

### Alternative 1: Material UI

**Description**: Google's Material Design implementation for React

**Pros**:
- Very popular with a large community
- Comprehensive component library
- Good documentation
- Flexible theming system

**Cons**:
- Different design language from Carbon
- Would require more customization to match the desired presentation aesthetic
- No built-in chart library tightly aligned with the component system

**Reason for Rejection**: Carbon offers a better combination of presentation-oriented styling, theming, and ecosystem fit for this project.

### Alternative 2: Ant Design

**Description**: Enterprise-focused design system for React

**Pros**:
- Enterprise-oriented components
- Comprehensive library
- Good data-heavy UI support
- Professional appearance

**Cons**:
- Different design language from Carbon
- Less aligned with the project's desired visual system
- Would still require customization for presentation-specific styling

**Reason for Rejection**: Carbon provides a cleaner fit for the intended design language and chart integration needs.

### Alternative 3: Custom Design System

**Description**: Build a custom component library from scratch

**Pros**:
- Complete control over design
- No external UI framework dependency
- Exactly tailored to project needs
- Potentially smaller bundle

**Cons**:
- Significant development time
- Accessibility would need to be implemented and maintained manually
- Ongoing maintenance burden
- No chart library included
- Higher risk of inconsistency

**Reason for Rejection**: The time investment and maintenance cost are too high for a slide deck project when Carbon already satisfies the main requirements.

### Alternative 4: Tailwind CSS + Headless UI

**Description**: Utility-first CSS framework with unstyled primitives

**Pros**:
- Very flexible
- Smaller runtime overhead
- Modern approach
- Good developer experience

**Cons**:
- No pre-built themed design system
- More custom work needed to build a polished presentation UI
- No integrated chart library
- Less consistency by default

**Reason for Rejection**: Too much custom implementation effort would be required to achieve the same level of polish and consistency.

---

## Implementation Notes

### Key Implementation Details

1. **Theme Configuration**:
   ```jsx
   import { Theme } from '@carbon/react';
   
   function App() {
     return (
       <Theme theme="g90">
         {/* Application content */}
       </Theme>
     );
   }
   ```

2. **SCSS Integration** ([`vite.config.js`](adr/0002-carbon-design-system.md)):
   ```javascript
   css: {
     preprocessorOptions: {
       scss: {
         additionalData: `
           @use '@carbon/react/scss/spacing' as *;
           @use '@carbon/react/scss/theme' as *;
           @use '@carbon/react/scss/type' as *;
         `
       }
     }
   }
   ```

3. **Global Styles**:
   ```css
   @import '@carbon/styles/css/styles.css';
   @import '@carbon/charts-react/styles.css';
   ```

### Migration Path

N/A - Initial implementation

### Validation

- ✅ All components render correctly in the default theme
- ✅ Charts display properly with Carbon styling
- ✅ Accessibility features work, including keyboard navigation and screen reader support
- ✅ Responsive behavior functions correctly
- ✅ SCSS mixins are available in component styles
- ✅ Icons render properly

---

## References

- [Carbon Design System](https://carbondesignsystem.com/)
- [Carbon React Components](https://react.carbondesignsystem.com/)
- [Carbon Charts](https://charts.carbondesignsystem.com/)
- [Carbon Themes](https://carbondesignsystem.com/guidelines/themes/overview)
- Related ADRs: ADR-0003 (Vite Build Tool), ADR-0005 (Component-Based Architecture)

---

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2026-04-14 | Development Team | Initial version |

---

## Notes

- The [`g90`](adr/0002-carbon-design-system.md:42) dark theme was chosen as the default because it works well in presentation environments
- Carbon spacing tokens such as `$spacing-05` and `$spacing-07` should be used consistently throughout the application
- Custom components should follow Carbon patterns wherever practical for consistency
- Auto-injection of SCSS mixins through Vite reduces repetitive imports in component styles