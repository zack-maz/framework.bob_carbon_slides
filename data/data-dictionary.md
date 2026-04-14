# Data Dictionary

**Purpose**: Reference documentation for the data structures used in a generalized slide deck application.  
**Last Updated**: 2026-04-14

---

## Overview

This document defines the common data structures used by a slide deck application that relies on JSON configuration and supporting content files.

It focuses on:

- slide configuration
- section metadata
- chart-ready data
- reusable narrative or case-study content
- comparison or feature-card content

Not every project will use every structure described here. Treat this document as a reusable reference model.

---

## Data File Locations

Most data files live under `src/data/`.

Example layout:

```text
src/data/
├── slides.json                 # Slide configuration and metadata
├── chart-data.json             # Optional chart data
└── content-data.json           # Optional additional content (narrative, cards, etc.)
```

---

## Core Configuration

### [`slides.json`](../src/data/slides.json)

**Purpose**: Defines slide order, metadata, and section structure.

**Typical structure**:

```json
{
  "slides": [Slide],
  "sections": [Section]
}
```

---

## Slide Object

| Field | Type | Required | Constraints | Description | Example |
|-------|------|----------|-------------|-------------|---------|
| `id` | string | Yes | unique, kebab-case recommended | Stable slide identifier | `"title"` |
| `component` | string | Yes | must match a registered component | React component name | `"IntroSlide"` |
| `title` | string | Yes | non-empty | Display label for navigation and indexing | `"Introduction"` |
| `section` | string | Yes | should reference a valid section ID | Section membership | `"overview"` |
| `duration` | number | No | positive integer recommended | Estimated speaking time in seconds | `30` |

### Validation guidance

- `id` should be unique across all slides
- `component` should exist in the slide component registry
- `section` should reference an existing section
- `duration` should be realistic for the presentation context

### Example

```json
{
  "id": "intro",
  "component": "IntroSlide",
  "title": "Introduction",
  "section": "overview",
  "duration": 30
}
```

---

## Section Object

| Field | Type | Required | Constraints | Description | Example |
|-------|------|----------|-------------|-------------|---------|
| `id` | string | Yes | unique, kebab-case recommended | Stable section identifier | `"overview"` |
| `title` | string | Yes | non-empty | Display title for the section | `"Overview"` |
| `startSlide` | number | No | valid index if used | Index of first slide in the section | `0` |
| `duration` | string | No | project-defined format | Estimated section duration | `"3m"` |

### Example

```json
{
  "id": "overview",
  "title": "Overview",
  "startSlide": 0,
  "duration": "3m"
}
```

---

## Chart Data Structures

Many slide decks include supporting chart data stored in standalone JSON files.

### Generic chart dataset structure

```json
{
  "dataset": {
    "data": [DataPoint],
    "options": ChartOptions,
    "source": "Optional source attribution",
    "sourceUrl": "Optional source URL"
  }
}
```

### DataPoint Object

| Field | Type | Required | Description | Example |
|-------|------|----------|-------------|---------|
| `group` | string | Yes | Series, category, or label | `"Platform A"` |
| `value` | number | Yes | Numeric value | `1200` |
| `year` | string | No | Time label for trend data | `"2026"` |

### Example

```json
{
  "dataset": {
    "data": [
      { "group": "Platform A", "value": 1200 },
      { "group": "Platform B", "value": 900 }
    ],
    "options": {
      "title": "Comparison Chart",
      "theme": "g90"
    },
    "source": "Internal analysis"
  }
}
```

---

## Chart Options Object

Common chart configuration fields include:

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | No | Chart title |
| `resizable` | boolean | No | Whether the chart can resize |
| `height` | string | No | Height such as `"400px"` |
| `theme` | string | No | Carbon theme name |
| `axes` | object | No | Axis configuration |
| `legend` | object | No | Legend configuration |
| `toolbar` | object | No | Toolbar configuration |

The exact shape depends on the chart library and chart type being used.

---

## Additional Content Structures

Slide decks may include additional structured content beyond basic slides and charts. The exact structure depends on presentation needs.

### Common content patterns

Projects may define custom JSON structures for:
- Narrative or case study content
- Feature comparison cards
- Timeline or milestone data
- Team or speaker information
- Resource lists or references

### Example: Simple content item

```json
{
  "id": "content-1",
  "title": "Content Title",
  "description": "Content description",
  "items": [
    {
      "label": "Item 1",
      "value": "Details"
    }
  ]
}
```

---

### Example: Card-based content

```json
{
  "cards": [
    {
      "id": "card-1",
      "title": "Card Title",
      "icon": "IconName",
      "description": "Card description",
      "points": ["Point 1", "Point 2"]
    }
  ]
}
```

**Note**: Define content structures based on your specific presentation needs. The examples above are starting points that can be adapted or extended.

---

## Data Relationships

### Slide to section

- **Type**: many-to-one
- **Field**: `slide.section` → `section.id`
- **Expectation**: every slide belongs to a valid section if sections are used

### Slide to component

- **Type**: one-to-one at render time
- **Field**: `slide.component`
- **Expectation**: component key exists in the application shell mapping

### Slide to supporting data

- **Type**: optional dependency
- **Expectation**: slide components may import additional JSON files for charts, narrative content, or card-based layouts

---

## Validation Guidance

### Schema validation

Consider JSON Schema validation for critical files such as [`slides.json`](../src/data/slides.json).

Example outline:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "slides": {
      "type": "array"
    },
    "sections": {
      "type": "array"
    }
  }
}
```

### Runtime validation

Projects may also use lightweight runtime validation.

Example:

```javascript
function validateSlide(slide) {
  if (!slide.id || typeof slide.id !== 'string') {
    throw new Error('Slide must have a valid id');
  }

  if (!slide.component || typeof slide.component !== 'string') {
    throw new Error('Slide must have a valid component');
  }

  return true;
}
```

---

## Data Source Guidance

When storing external or researched data, capture:

- source name
- source URL if available
- last review date
- update cadence if the data is expected to change

Example metadata fields:

| Field | Meaning |
|-------|---------|
| `source` | Human-readable source attribution |
| `sourceUrl` | Direct URL to source material |
| `lastUpdated` | Date the data was last reviewed |
| `updateFrequency` | Review cadence such as quarterly or annual |

---

## Data Governance

### Quality standards

- data should come from credible sources
- required fields should always be present
- field names and types should remain consistent
- updates should preserve existing schema unless intentionally revised
- documentation should be updated when structures change

### Ownership guidance

Teams often assign ownership by category, for example:

| Data Category | Typical Owner |
|---------------|---------------|
| Slide configuration | Development team |
| Research or comparison data | Product or strategy team |
| Narrative examples | Marketing, product, or solutions team |
| Technical capability cards | Product or engineering team |

---

## Common Type Definitions

```typescript
type SlideId = string;
type ComponentName = string;
type SectionId = string;
type DurationSeconds = number;
type Percentage = number;
type IconName = string;
type Color = string;
```

Example section enumeration (adapt to your presentation structure):

```typescript
enum SectionIds {
  INTRO = 'intro',
  MAIN = 'main',
  CONCLUSION = 'conclusion'
}
```

---

## References

- [data-lineage.md](data-lineage.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- [adr/0004-json-slide-configuration.md](../adr/0004-json-slide-configuration.md)
- [JSON Schema](https://json-schema.org/)
- [Carbon Charts Data Format](https://charts.carbondesignsystem.com/)

---

**Last Updated**: 2026-04-14