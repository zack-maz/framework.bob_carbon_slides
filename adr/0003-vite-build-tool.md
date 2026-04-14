# ADR-0003: Use Vite as Build Tool

**Status**: Accepted  
**Date**: 2026-04-14  
**Deciders**: Development Team  
**Technical Story**: Build tooling and development environment

---

## Context

### Background

This project is a modern React application for building web-based slide decks. It requires a build tool for development, bundling, and optimization. The build tool must provide a fast development experience, efficient production builds, and support for modern JavaScript features, SCSS preprocessing, and path aliases.

### Problem Statement

We need a build tool that:
- Provides instant development server startup
- Offers fast Hot Module Replacement (HMR)
- Supports React with JSX and TSX
- Handles SCSS preprocessing
- Enables path aliases for clean imports
- Produces optimized production builds
- Has minimal configuration overhead
- Supports modern ES modules

### Constraints

- Must support React 18+
- Should work well with Carbon Design System SCSS
- Must support path aliases such as `@/` and `@components/`
- Should provide source maps for debugging
- Must be actively maintained
- Should have good documentation

---

## Decision

We will use **Vite 5.x** as the build tool and development server.

### Rationale

1. **Lightning Fast**: Native ES modules in development provide near-instant server startup and sub-second HMR
2. **Modern by Default**: Built for modern browsers and uses esbuild for pre-bundling
3. **React Support**: Official React plugin with strong JSX and TSX support
4. **SCSS Integration**: Built-in SCSS support with configurable preprocessor options
5. **Path Aliases**: Straightforward configuration for import aliases
6. **Optimized Builds**: Uses Rollup for production with automatic code splitting
7. **Minimal Config**: Works out of the box with sensible defaults
8. **Developer Experience**: Fast feedback loops improve productivity
9. **Active Ecosystem**: Strong community adoption and ongoing development

---

## Consequences

### Positive Consequences

- **Development Speed**: Instant server start compared to slower legacy bundlers
- **Fast HMR**: Changes reflect quickly during development
- **Simple Configuration**: Small, readable configuration file
- **Modern Output**: Optimized for modern browsers by default
- **Better Developer Experience**: Faster iteration improves productivity
- **Source Maps**: Built-in debugging support
- **Tree Shaking**: Automatic dead code elimination
- **Code Splitting**: Automatic chunk optimization

### Negative Consequences

- **Browser Support**: Optimized for modern browsers (ES2015+)
- **Plugin Ecosystem**: Smaller ecosystem than older tools such as webpack
- **Build Differences**: Development and production use different underlying bundlers
- **Learning Curve**: Team members used to webpack may need some adjustment

### Neutral Consequences

- **Configuration Style**: Uses ES modules for configuration
- **Plugin Model**: Different from older bundler ecosystems
- **Community Size**: Still smaller than webpack, though growing rapidly

---

## Alternatives Considered

### Alternative 1: Create React App

**Description**: Official React toolchain with webpack under the hood

**Pros**:
- Historically common setup
- Zero configuration
- Well-tested and stable
- Large community

**Cons**:
- Slower development server startup
- Slower HMR
- Harder to customize without ejecting
- Heavier dependency footprint
- No longer actively recommended for new React projects

**Reason for Rejection**: The slower development experience and lower flexibility make it a weaker fit for this project.

### Alternative 2: Webpack

**Description**: Mature bundler with extensive ecosystem support

**Pros**:
- Battle-tested
- Huge plugin ecosystem
- Extensive configuration options
- Broad community support

**Cons**:
- Slower development experience
- More complex configuration
- More boilerplate
- Steeper learning curve

**Reason for Rejection**: The configuration complexity and slower development workflow are not justified for this project's needs.

### Alternative 3: Parcel

**Description**: Zero-configuration bundler

**Pros**:
- Minimal configuration
- Fast builds
- Automatic transforms
- Simple to use

**Cons**:
- Less control over configuration
- Smaller ecosystem than webpack or Vite
- Less predictable behavior in advanced cases
- Harder to customize for specific project needs

**Reason for Rejection**: Vite offers a better balance of simplicity, speed, and configurability.

### Alternative 4: esbuild Only

**Description**: Use esbuild directly as the main build tool

**Pros**:
- Extremely fast build times
- Minimal configuration
- Lightweight

**Cons**:
- No built-in development server comparable to Vite
- No built-in HMR workflow
- Smaller plugin ecosystem
- Less suitable as a complete developer platform

**Reason for Rejection**: esbuild is excellent as a building block, but Vite provides the broader development experience needed by this project.

---

## Implementation Notes

### Key Configuration ([`vite.config.js`](adr/0003-vite-build-tool.md))

```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@data': path.resolve(__dirname, './src/data'),
      '@assets': path.resolve(__dirname, './src/assets'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
      '@utils': path.resolve(__dirname, './src/utils'),
    }
  },
  
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
  },
  
  server: {
    port: 3000,
    open: true
  },
  
  build: {
    outDir: 'dist',
    sourcemap: true
  }
});
```

### Scripts ([`package.json`](adr/0003-vite-build-tool.md))

```json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  }
}
```

### Migration Path

N/A - Initial implementation

### Validation

- ✅ Development server starts quickly
- ✅ HMR updates are fast
- ✅ SCSS compiles correctly with Carbon mixins
- ✅ Path aliases resolve properly
- ✅ Production build optimizes correctly
- ✅ Source maps work for debugging
- ✅ React Fast Refresh works

---

## References

- [Vite Documentation](https://vitejs.dev/)
- [Vite React Plugin](https://github.com/vitejs/vite-plugin-react)
- [Why Vite](https://vitejs.dev/guide/why.html)
- [Vite vs Others](https://vitejs.dev/guide/comparisons.html)
- [React Documentation - Start a New React Project](https://react.dev/learn/start-a-new-react-project)
- Related ADRs: ADR-0002 (Carbon Design System), ADR-0006 (Path Aliases)

---

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2026-04-14 | Development Team | Initial version |

---

## Notes

- Vite's fast server startup and HMR improve day-to-day development productivity
- The SCSS [`additionalData`](adr/0003-vite-build-tool.md:137) configuration eliminates repeated Carbon mixin imports in every component style file
- Path aliases make imports cleaner and easier to refactor
- Source maps are enabled in production builds to support debugging deployed applications
- The [`open: true`](adr/0003-vite-build-tool.md:149) server option automatically opens the browser on [`npm run dev`](adr/0003-vite-build-tool.md:163)