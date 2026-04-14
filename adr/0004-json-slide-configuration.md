# ADR-0004: JSON-Based Slide Configuration

**Status**: Accepted  
**Date**: 2026-04-14  
**Deciders**: Development Team  
**Technical Story**: Slide management and configuration system

---

## Context

### Background

This slide deck application consists of multiple slides that need to be organized, ordered, and configured. The project needs a way to manage slide metadata, section groupings, timing information, and the relationship between slides and their React components.

### Problem Statement

We need a configuration system that:
- Defines slide order and metadata
- Maps slide IDs to React components
- Groups slides into logical sections
- Tracks presentation timing information
- Enables easy reordering without code changes
- Supports non-technical contributors editing slide order
- Provides a single source of truth for deck structure
- Allows for future extensibility such as speaker notes and transitions

### Constraints

- Must be easily editable by non-developers
- Should not require code changes for slide reordering
- Must maintain a clear relationship between data and implementation
- Should support validation
- Must be version-controllable
- Should be human-readable

---

## Decision

We will use a **JSON-based configuration file** named [`slides.json`](adr/0004-json-slide-configuration.md:41) to define slide order, metadata, and section structure, separate from the React component implementations.

### Rationale

1. **Separation of Concerns**: Configuration is separated from implementation
2. **Easy Editing**: Non-developers can modify slide order and metadata
3. **Single Source of Truth**: One file defines the overall presentation structure
4. **Version Control**: JSON diffs are clear and reviewable
5. **Validation**: JSON schema can validate structure
6. **Extensibility**: Easy to add new fields without redesigning the architecture
7. **Tooling Support**: JSON editors often provide validation and autocomplete
8. **Human Readable**: The structure is simple to understand and maintain

---

## Consequences

### Positive Consequences

- **Non-Technical Editing**: Presenters and content contributors can reorder slides without changing component code
- **Clear Structure**: Presentation flow is visible in one place
- **Easy Maintenance**: Add, remove, or reorder slides by editing JSON
- **Metadata Management**: Centralized location for slide metadata
- **Section Organization**: Logical grouping of related slides
- **Timing Information**: Duration tracking supports planning and rehearsal
- **Extensibility**: New fields can be added over time
- **Validation**: Structure can be checked before runtime

### Negative Consequences

- **Sync Requirement**: Component names in JSON must match actual registered components
- **Runtime Errors**: Typos in component names may only surface at runtime
- **Two-Step Process**: New slides require JSON updates and component registration
- **Limited Static Safety**: JSON itself does not provide compile-time validation
- **Manual Validation**: Structure must be validated by tooling or runtime checks

### Neutral Consequences

- **File Size**: Small overhead for configuration storage
- **Load Time**: Minimal parsing cost
- **Maintenance Surface**: Adds one more file to maintain

---

## Alternatives Considered

### Alternative 1: Hardcoded Array in JavaScript

**Description**: Define slide configuration directly in [`App.jsx`](adr/0004-json-slide-configuration.md:85) as a JavaScript array

**Pros**:
- Easier integration with application code
- Potentially stronger static typing if TypeScript is added
- No separate file to maintain
- Simpler import flow

**Cons**:
- Requires code changes for slide reordering
- Harder for non-developers to edit
- Mixes configuration with implementation
- Less clean for content-focused reviews

**Reason for Rejection**: Configuration should remain separate from code so that slide ordering and metadata updates are easier to manage.

### Alternative 2: YAML Configuration

**Description**: Use YAML instead of JSON for configuration

**Pros**:
- More compact syntax
- Supports comments
- Friendly for some content-heavy workflows
- Good readability for nested structures

**Cons**:
- Requires YAML parsing support
- Less universal than JSON
- Not natively available in the browser
- Slightly higher tooling complexity

**Reason for Rejection**: JSON is more universal, easier to integrate with frontend tooling, and sufficient for the project's structure.

### Alternative 3: Database or CMS

**Description**: Store slide configuration in a database or content management system

**Pros**:
- Dynamic updates without redeploying
- Dedicated editing interface
- Built-in history in some systems
- Multi-user editing support

**Cons**:
- Requires backend infrastructure
- Adds significant complexity
- Overkill for a static or semi-static slide deck
- Introduces authentication and deployment concerns

**Reason for Rejection**: Too much infrastructure and complexity for a slide deck application that can be managed as code and data files.

### Alternative 4: Markdown with Frontmatter

**Description**: Define each slide in a Markdown file with metadata in frontmatter

**Pros**:
- Content and metadata live together
- Familiar authoring format
- Good fit for text-heavy documents
- Supports rich text

**Cons**:
- Requires Markdown parsing
- Less suitable for component-driven slides
- Slide order becomes more distributed
- Adds unnecessary complexity for a React component architecture

**Reason for Rejection**: The slides are React components, not primarily Markdown content, so this does not fit the architecture well.

---

## Implementation Notes

### JSON Structure

```json
{
  "slides": [
    {
      "id": "unique-slide-id",
      "component": "ComponentName",
      "title": "Slide Title",
      "section": "section-id",
      "duration": 120
    }
  ],
  "sections": [
    {
      "id": "section-id",
      "title": "Section Title",
      "startSlide": 2,
      "duration": "2m"
    }
  ]
}
```

### Slide Object Fields

- **id**: Unique identifier for the slide in kebab-case
- **component**: React component name that must match the [`slideComponents`](adr/0004-json-slide-configuration.md:132) registry
- **title**: Display title for navigation or table of contents
- **section**: Section ID that groups related slides
- **duration**: Expected presentation time in seconds

### Section Object Fields

- **id**: Unique identifier for the section
- **title**: Display title for the section
- **startSlide**: Index of the first slide in the section using 0-based indexing
- **duration**: Expected section duration in a human-readable format

### Component Registration

Components must be registered in [`App.jsx`](adr/0004-json-slide-configuration.md:130):

```javascript
const slideComponents = {
  IntroSlide,
  ContentSlide,
  // ... other components
};
```

### Migration Path

N/A - Initial implementation

### Validation

- ✅ All component names in JSON exist in the [`slideComponents`](adr/0004-json-slide-configuration.md:132) registry
- ✅ All section IDs referenced by slides exist
- ✅ Slide IDs are unique
- ✅ Section IDs are unique
- ✅ Duration values are positive numbers
- ✅ JSON is valid and parseable

---

## References

- [JSON Schema](https://json-schema.org/)
- Related ADRs: ADR-0001 (React Router), ADR-0005 (Component-Based Architecture)

---

## Revision History

| Date | Author | Changes |
|------|--------|---------|
| 2026-04-14 | Development Team | Initial version |

---

## Notes

### Adding a New Slide

1. Create the React component in [`src/components/slides/`](adr/0004-json-slide-configuration.md:153)
2. Import and register it in [`App.jsx`](adr/0004-json-slide-configuration.md:154)
3. Add a slide entry to [`slides.json`](adr/0004-json-slide-configuration.md:155) with the correct component name
4. Verify the component name matches exactly and is case-sensitive

### Reordering Slides

Reorder the slide objects in the [`slides`](adr/0004-json-slide-configuration.md:160) array in [`slides.json`](adr/0004-json-slide-configuration.md:160). No code changes are required.

### Future Extensibility

The JSON structure can be extended with additional fields such as:
- `notes` for speaker notes
- `transition` for custom transition effects
- `hidden` to exclude slides without deleting them
- `tags` for categorization
- `metadata` for other custom information

### Validation Strategy

Consider implementing:
- JSON Schema validation
- Runtime validation on application load
- Pre-commit hooks to validate JSON
- TypeScript types generated from a JSON schema