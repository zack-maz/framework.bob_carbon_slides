# ADR-0006: Path Aliases for Import Management

**Status**: Accepted  
**Date**: 2026-04-14  
**Deciders**: Development Team  
**Technical Story**: Import path management and code organization

---

## Context

### Background

This slide deck application has a nested directory structure with components, data files, assets, hooks, and utilities organized in separate folders. Without path aliases, imports use relative paths that can become complex and error-prone, especially when refactoring or moving files.

### Problem Statement

We need an import system that:
- Avoids complex relative paths such as `../../../components/slides/`
- Remains stable when files are moved or refactored
- Provides a clear indication of import source
- Improves code readability
- Reduces import errors
- Supports IDE autocomplete and navigation
- Works consistently across the codebase

### Constraints

- Must work with the Vite build system
- Should support IDE features such as autocomplete and go-to-definition
- Must not break existing imports during migration
- Should be intuitive for developers
- Must work in both development and production builds

---

## Decision

We will configure **path aliases** in Vite to provide clean, absolute-style imports using the `@` prefix pattern:

- `@/` → `src/`
- `@components/` → `src/components/`
- `@data/` → `src/data/`
- `@assets/` → `src/assets/`
- `@hooks/` → `src/hooks/`
- `@utils/` → `src/utils/`

### Rationale

1. **Readability**: Clear, absolute-style imports are easier to understand
2. **Refactoring Safety**: Imports do not break when files move
3. **Consistency**: The same import path style can be used regardless of file location
4. **IDE Support**: Modern IDEs recognize and autocomplete aliases
5. **Industry Convention**: The `@` prefix is widely used in React projects
6. **Maintainability**: Import paths are easier to update globally
7. **Developer Experience**: Less cognitive load when writing and reviewing imports

---

## Consequences

### Positive Consequences

- **Cleaner Imports**: `import Slide from '@components/slides/TitleSlide'` is clearer than deep relative imports
- **Refactoring Safety**: Moving files is less likely to break imports
- **Consistency**: Same import style across the codebase
- **IDE Features**: Autocomplete and go-to-definition work better
- **Reduced Errors**: Lower chance of incorrect relative paths
- **Easier Navigation**: Import origin is easier to identify
- **Better Diffs**: Import changes are clearer in version control

### Negative Consequences

- **Learning Curve**: New contributors need to learn the alias mappings
- **Configuration Required**: Both Vite and IDE settings must be configured
- **Debugging Context**: Some tooling may show resolved paths instead of aliases
- **Migration Effort**: Existing relative imports may need updating

### Neutral Consequences

- **Bundle Size**: No impact on final bundle size
- **Performance**: Negligible impact on build time
- **Compatibility**: Works well with modern frontend tooling

---

## Alternatives Considered

### Alternative 1: Relative Paths Only

**Description**: Use only relative paths for all imports

**Pros**:
- No configuration needed
- File relationships are explicit
- Works everywhere by default
- No alias learning curve

**Cons**:
- Deep paths become difficult to read
- Imports break more easily when files move
- More error-prone
- Less consistent in nested structures

**Reason for Rejection**: Relative paths become unwieldy in nested project structures and are fragile during refactoring.

### Alternative 2: Root-Relative Paths

**Description**: Import from the project root without aliases, such as `src/components/...`

**Pros**:
- Clear absolute paths
- Easy to read
- No alias names to remember

**Cons**:
- Not supported by default in many toolchains
- Usually still requires configuration
- Less conventional than aliases in React projects

**Reason for Rejection**: Requires configuration anyway and is less clean than an alias-based approach.

### Alternative 3: Barrel Exports

**Description**: Use [`index.js`](adr/0006-path-aliases.md:96) files to re-export modules from directories

**Pros**:
- Clean import statements
- Can help define a public API surface
- Encapsulates internal structure

**Cons**:
- Requires many additional files
- Can obscure actual file locations
- Adds maintenance overhead
- Does not solve the full deep-path problem on its own

**Reason for Rejection**: Adds complexity and maintenance overhead without replacing the benefits of path aliases.

### Alternative 4: Monorepo Package Boundaries

**Description**: Split the project into internal packages with separate package manifests

**Pros**:
- Strong module boundaries
- Explicit dependency separation
- Scales for very large codebases

**Cons**:
- Massive overkill for this application
- Complex setup and tooling
- Slower development workflow
- Unnecessary for a single app

**Reason for Rejection**: Too much infrastructure for the size and purpose of this project.

---

## Implementation Notes

### Vite Configuration ([`vite.config.js`](adr/0006-path-aliases.md))

```javascript
import { defineConfig } from 'vite';
import path from 'path';

export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@data': path.resolve(__dirname, './src/data'),
      '@assets': path.resolve(__dirname, './src/assets'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
      '@utils': path.resolve(__dirname, './src/utils'),
    }
  }
});
```

### Usage Examples

```javascript
// Before (relative paths)
import IntroSlide from '../../../components/slides/IntroSlide';
import slidesData from '../../../data/slides.json';
import useKeyboardNavigation from '../../../hooks/useKeyboardNavigation';

// After (path aliases)
import IntroSlide from '@components/slides/IntroSlide';
import slidesData from '@data/slides.json';
import useKeyboardNavigation from '@hooks/useKeyboardNavigation';
```

### IDE Configuration

For VS Code, create or update [`jsconfig.json`](adr/0006-path-aliases.md):

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@components/*": ["src/components/*"],
      "@data/*": ["src/data/*"],
      "@assets/*": ["src/assets/*"],
      "@hooks/*": ["src/hooks/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

### Migration Path

1. Configure aliases in [`vite.config.js`](adr/0006-path-aliases.md:125)
2. Configure IDE support in [`jsconfig.json`](adr/0006-path-aliases.md:154) or [`tsconfig.json`](adr/0006-path-aliases.md:154)
3. Update imports gradually or all at once
4. Test that all imports resolve correctly
5. Verify the production build still works

### Validation

- ✅ All aliases resolve correctly in development
- ✅ Production build works with aliases
- ✅ IDE autocomplete works for aliased paths
- ✅ Go-to-definition works in the IDE
- ✅ No import errors appear in the console
- ✅ Hot reload works with aliased imports

---

## References

- [Vite Path Aliases](https://vitejs.dev/config/shared-options.html#resolve-alias)
- [Node.js Path Module](https://nodejs.org/api/path.html)
- [VS Code Path Mapping](https://code.visualstudio.com/docs/languages/jsconfig)
- Related ADRs: ADR-0003 (Vite Build Tool), ADR-0005 (Component-Based Architecture)

---

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2026-04-14 | Development Team | Initial version |

---

## Notes

### Alias Naming Convention

- `@/` - Root [`src/`](adr/0006-path-aliases.md:191) directory
- `@<folder>/` - Specific folders for more explicit imports

The `@` prefix is used because:
- It is a widely recognized convention in the React ecosystem
- It is unlikely to conflict with npm package names
- It is visually distinct from relative paths
- It is supported by most modern IDEs

### Best Practices

1. **Use specific aliases** when available, such as `@components/` over `@/components/`
2. **Be consistent** and avoid mixing relative and aliased imports without a reason
3. **Keep IDE config in sync** with Vite config
4. **Document aliases** for new team members
5. **Use aliases in new code** so the convention remains consistent

### Common Pitfalls

- Forgetting to update IDE configuration
- Mixing relative and aliased imports inconsistently
- Using aliases in configuration files that expect Node-style resolution
- Forgetting to restart the IDE after config changes

### Future Considerations

If the project grows, consider adding aliases such as:
- `@types/` for TypeScript types
- `@constants/` for application constants
- `@contexts/` for React contexts
- `@services/` for service or API layers