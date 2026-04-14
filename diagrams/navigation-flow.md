# Navigation Flow Diagram

**Purpose**: Visualize navigation patterns and user interactions.

**Last Updated**: 2026-04-14

---

## Navigation Methods

```mermaid
graph TD
    A[User] -->|Arrow Keys| B[Keyboard Navigation]
    A -->|Click Buttons| C[UI Navigation]
    A -->|Click TOC| D[Section Navigation]
    A -->|Type URL| E[Direct URL]
    
    B -->|useKeyboardNavigation| F[goToSlide]
    C -->|onClick| F
    D -->|onClick| F
    E -->|React Router| F
    
    F -->|navigate| G[Update URL]
    G -->|trigger| H[Re-render]
    H -->|display| I[New Slide]
    
    style A fill:#0f62fe,color:#fff
    style F fill:#4589ff,color:#fff
    style I fill:#8a3ffc,color:#fff
```

---

## Keyboard Navigation Flow

```mermaid
stateDiagram-v2
    [*] --> Slide1: Initial Load
    
    Slide1 --> Slide2: ArrowRight / Space / PageDown
    Slide2 --> Slide3: ArrowRight / Space / PageDown
    Slide3 --> SlideN: ... (continue)
    
    Slide2 --> Slide1: ArrowLeft / PageUp
    Slide3 --> Slide2: ArrowLeft / PageUp
    
    Slide1 --> Slide1: Home (already first)
    Slide2 --> Slide1: Home
    Slide3 --> Slide1: Home
    
    Slide1 --> SlideN: End
    Slide2 --> SlideN: End
    SlideN --> SlideN: End (already last)
    
    note right of Slide1
        Keyboard disabled when
        focus is in input or textarea
    end note
```

---

## URL-Based Navigation

```mermaid
graph LR
    A[URL: /slide/1] -->|Route Match| B[PresentationView]
    B -->|Parse| C[slideIndex = 1]
    C -->|Convert| D[currentIndex = 0]
    D -->|Lookup| E[slides[0]]
    E -->|Map| F[First Slide Component]
    F -->|Render| G[Display]
    
    H[Invalid URL: /slide/999] -->|Route Match| B
    B -->|Parse| I[slideIndex = 999]
    I -->|Validate| J[Out of bounds]
    J -->|Redirect| K[/slide/1]
```

---

## Section Navigation

```mermaid
sequenceDiagram
    participant U as User
    participant TOC as TableOfContents
    participant PL as PresentationLayout
    participant PV as PresentationView
    participant R as Router
    
    U->>TOC: Click Section Name
    TOC->>TOC: Find section.startSlide
    TOC->>PL: onNavigate(startSlide)
    PL->>PV: goToSlide(startSlide)
    PV->>R: navigate('/slide/X')
    R->>PV: Re-render with new slide
    PV->>U: Display section's first slide
```

---

## Navigation State Machine

```mermaid
stateDiagram-v2
    [*] --> Loading: App Start
    Loading --> FirstSlide: Data Loaded
    
    FirstSlide --> MiddleSlide: Navigate Forward
    MiddleSlide --> LastSlide: Navigate Forward
    MiddleSlide --> FirstSlide: Navigate Back
    LastSlide --> MiddleSlide: Navigate Back
    
    FirstSlide --> FirstSlide: Previous (blocked)
    LastSlide --> LastSlide: Next (blocked)
    
    MiddleSlide --> MiddleSlide: Same slide navigation
    
    FirstSlide --> LastSlide: Jump to End
    LastSlide --> FirstSlide: Jump to Home
    MiddleSlide --> FirstSlide: Jump to Home
    MiddleSlide --> LastSlide: Jump to End
```

---

## Navigation Controls State

```mermaid
graph TD
    A[Current Slide Index] -->|Check| B{Is First Slide?}
    A -->|Check| C{Is Last Slide?}
    
    B -->|Yes| D[Disable Previous Button]
    B -->|No| E[Enable Previous Button]
    
    C -->|Yes| F[Disable Next Button]
    C -->|No| G[Enable Next Button]
    
    D --> H[Render Controls]
    E --> H
    F --> H
    G --> H
```

---

## Table of Contents Interaction

```mermaid
graph TD
    A[User Clicks TOC Button] -->|Toggle| B{TOC Open?}
    B -->|No| C[Open TOC Panel]
    B -->|Yes| D[Close TOC Panel]
    
    C --> E[Display Sections]
    E --> F[User Clicks Section]
    F --> G[Navigate to Section]
    G --> H[Close TOC]
    H --> I[Display Slide]
    
    D --> J[Hide TOC Panel]
```

---

## Navigation Error Handling

```mermaid
graph TD
    A[Navigation Request] -->|Validate| B{Valid Index?}
    B -->|Yes| C[Update URL]
    B -->|No| D{Index < 0?}
    
    D -->|Yes| E[Navigate to First Slide]
    D -->|No| F{Index >= Total?}
    
    F -->|Yes| G[Navigate to Last Slide]
    F -->|No| H[Navigate to First Slide]
    
    C --> I[Render Slide]
    E --> I
    G --> I
    H --> I
```

---

## Keyboard Shortcuts Reference

| Key | Action | Condition |
|-----|--------|-----------|
| `ArrowRight` | Next slide | Not last slide |
| `ArrowLeft` | Previous slide | Not first slide |
| `Space` | Next slide | Not last slide |
| `PageDown` | Next slide | Not last slide |
| `PageUp` | Previous slide | Not first slide |
| `Home` | First slide | Always |
| `End` | Last slide | Always |

**Disabled When**: Focus is in `input` or `textarea` elements

---

## Navigation Performance

```mermaid
graph LR
    A[Navigation Event] -->|< 50ms| B[Update URL]
    B -->|< 100ms| C[Component Re-render]
    C -->|< 200ms| D[Slide Displayed]
    
    style A fill:#e1f5ff
    style B fill:#fff4e1
    style C fill:#ffe1f5
    style D fill:#e1ffe1
```

---

## References

- [`system-context.md`](./system-context.md)
- [`component-architecture.md`](./component-architecture.md)
- [`ADR-0001`](../adr/0001-use-react-router.md)

---

**Last Updated**: 2026-04-14  
**Version**: 1.0