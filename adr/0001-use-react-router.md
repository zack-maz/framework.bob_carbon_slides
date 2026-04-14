# ADR-0001: Use React Router for Slide Navigation

**Status**: Accepted  
**Date**: 2026-04-14  
**Deciders**: Development Team  
**Technical Story**: Slide deck navigation system

---

## Context

### Background

This project is a web-based slide deck built with React. The application needs a robust navigation system that allows viewers and presenters to move between slides, supports deep linking to specific slides, and maintains browser history for back/forward navigation.

### Problem Statement

We need a navigation solution that:
- Allows direct navigation to any slide via URL
- Supports browser back/forward buttons
- Enables keyboard shortcuts for slide navigation
- Maintains current deck position across page refreshes
- Provides a clean, bookmarkable URL structure

### Constraints

- Must work seamlessly with React 18+
- Should not require server-side routing
- Must support single-page application (SPA) architecture
- Should be lightweight and performant
- Must integrate with shared keyboard navigation hooks

---

## Decision

We will use **React Router v6** for managing slide navigation with a URL-based routing pattern: `/slide/:slideIndex`

### Rationale

1. **Industry Standard**: React Router is the de facto routing library for React applications with extensive community support
2. **URL-Based Navigation**: Provides clean, bookmarkable URLs for each slide (for example, `/slide/5`)
3. **Browser Integration**: Native support for browser history API and back/forward buttons
4. **Declarative Routing**: Fits naturally with React's component model
5. **Hooks API**: Modern hooks-based API ([`useNavigate()`](adr/0001-use-react-router.md:45), [`useParams()`](adr/0001-use-react-router.md:45)) integrates cleanly with functional components
6. **No Server Required**: Client-side routing works well for static hosting

---

## Consequences

### Positive Consequences

- **Deep Linking**: Users can bookmark and share specific slides
- **Browser History**: Back/forward buttons work as expected
- **State Persistence**: Current slide persists across page refreshes
- **Developer Experience**: Well-documented library with extensive examples
- **Type Safety**: Strong TypeScript support if added later
- **Testing**: Mature testing utilities are available

### Negative Consequences

- **Bundle Size**: Adds approximately 10 KB to the bundle (minified and gzipped)
- **Learning Curve**: Team members unfamiliar with React Router need to learn it
- **Abstraction Layer**: Adds another dependency to manage and update

### Neutral Consequences

- **URL Structure**: Commits the project to the `/slide/:slideIndex` pattern
- **Version Lock**: Using v6 means following its API changes and migration paths

---

## Alternatives Considered

### Alternative 1: Custom Hash-Based Routing

**Description**: Implement custom routing using URL hash fragments (for example, `#slide-5`)

**Pros**:
- No external dependencies
- Lightweight solution
- Simple to implement
- Works with any hosting

**Cons**:
- Need to implement history management manually
- Less clean URLs
- More code to maintain
- No built-in navigation guards
- Harder to test

**Reason for Rejection**: Reinvents functionality already provided by a mature, well-tested solution. The maintenance burden and lack of built-in features outweigh the bundle size savings.

### Alternative 2: State-Based Navigation (No Routing)

**Description**: Keep all slides in memory and use React state to track the current slide

**Pros**:
- No routing library needed
- Simplest implementation
- Smallest bundle size

**Cons**:
- No deep linking capability
- No browser history support
- State lost on page refresh
- Cannot bookmark specific slides
- Poor user experience

**Reason for Rejection**: Fails to meet core requirements for bookmarkable URLs and browser history integration.

### Alternative 3: Reach Router

**Description**: Use Reach Router, a smaller alternative to React Router

**Pros**:
- Smaller bundle size
- Simpler API
- Good accessibility features

**Cons**:
- Project merged into React Router v6
- No longer actively maintained
- Smaller community
- Migration path leads to React Router anyway

**Reason for Rejection**: The project is deprecated and merged into React Router v6. Using React Router directly is the recommended path.

---

## Implementation Notes

### Key Implementation Details

1. **Route Structure**:
   ```jsx
   <Routes>
     <Route path="/slide/:slideIndex" element={<PresentationView />} />
     <Route path="/" element={<Navigate to="/slide/1" replace />} />
   </Routes>
   ```

2. **Navigation Function**:
   ```jsx
   const goToSlide = (index) => {
     if (index >= 0 && index < totalSlides) {
       navigate(`/slide/${index + 1}`);
     }
   };
   ```

3. **Index Conversion**: URLs use 1-based indexing (user-friendly) while internal arrays use 0-based indexing

### Migration Path

N/A - Initial routing implementation

### Validation

- ✅ All slides accessible via URL
- ✅ Browser back/forward buttons work correctly
- ✅ Keyboard navigation integrates properly
- ✅ Page refresh maintains current slide
- ✅ Invalid slide indices redirect to the first slide

---

## References

- [React Router Documentation](https://reactrouter.com/)
- [React Router v6 Migration Guide](https://reactrouter.com/en/main/upgrading/v5)
- Related ADRs: ADR-0005 (Component-Based Architecture)

---

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2026-04-14 | Development Team | Initial version |

---

## Notes

- The 1-based URL indexing (`/slide/1`) was chosen for user-friendliness, even though it requires conversion to 0-based array indices internally
- The [`goToSlide()`](adr/0001-use-react-router.md:123) wrapper function is used instead of direct [`navigate()`](adr/0001-use-react-router.md:125) calls to maintain consistency and allow future enhancements
- Keyboard navigation is handled separately via a shared keyboard hook but calls [`goToSlide()`](adr/0001-use-react-router.md:123) for actual navigation