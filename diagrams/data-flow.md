# Data Flow Diagram

**Purpose**: Visualize how data moves through the application.

**Last Updated**: 2026-04-14

---

## Complete Data Flow

```mermaid
graph TB
    subgraph "Data Sources"
        JSON1[slides.json]
        JSON2[content-data.json]
        JSON3[chart-data.json]
    end
    
    subgraph "Import Layer"
        APP[App.jsx]
        SLIDE1[Slide Component 1]
        SLIDE2[Slide Component 2]
        SLIDE3[Slide Component N]
    end
    
    subgraph "Processing Layer"
        PVIEW[PresentationView]
        PLAYOUT[PresentationLayout]
    end
    
    subgraph "Rendering Layer"
        COMP[Slide Components]
        CHART[Chart Components]
        UI[Carbon UI Components]
    end
    
    JSON1 -->|import| APP
    JSON2 -->|import| SLIDE1
    JSON3 -->|import| SLIDE2
    
    APP -->|slidesData| PVIEW
    PVIEW -->|currentSlide, sections| PLAYOUT
    PLAYOUT -->|props| COMP
    
    SLIDE1 -->|data| CHART
    SLIDE2 -->|data| CHART
    
    COMP -->|render| UI
    CHART -->|render| UI
    
    style JSON1 fill:#e1f5ff
    style JSON2 fill:#e1f5ff
    style JSON3 fill:#e1f5ff
```

---

## Navigation Data Flow

```mermaid
sequenceDiagram
    participant URL as Browser URL
    participant Router as React Router
    participant PV as PresentationView
    participant PL as PresentationLayout
    participant SC as Slide Component
    
    URL->>Router: /slide/5
    Router->>PV: slideIndex = "5"
    PV->>PV: Parse to index 4
    PV->>PV: Lookup slides[4]
    PV->>PV: Map to component
    PV->>PL: Pass currentSlide, goToSlide
    PL->>SC: Pass onNavigate, sections
    SC->>SC: Render content
    
    Note over SC: User presses arrow key
    SC->>PL: onNavigate(5)
    PL->>PV: goToSlide(5)
    PV->>Router: navigate('/slide/6')
    Router->>URL: Update to /slide/6
```

---

## Chart Data Flow

```mermaid
graph LR
    A[JSON Data File] -->|import| B[Slide Component]
    B -->|extract data| C[Data Array]
    B -->|extract options| D[Chart Options]
    C -->|pass as prop| E[Chart Component]
    D -->|pass as prop| E
    E -->|render with| F[Carbon Chart]
    F -->|display| G[Browser DOM]
```

---

## State Management

```mermaid
graph TD
    A[URL State] -->|slideIndex| B[PresentationView]
    B -->|currentIndex| C[Local State]
    C -->|derived| D[currentSlide]
    C -->|derived| E[totalSlides]
    D -->|prop| F[PresentationLayout]
    E -->|prop| F
    F -->|prop| G[Child Components]
    
    H[User Action] -->|keyboard/click| I[goToSlide function]
    I -->|navigate| A
```

---

## References

- [`data-dictionary.md`](../data/data-dictionary.md)
- [`data-lineage.md`](../data/data-lineage.md)
- [`system-context.md`](./system-context.md)

---

**Last Updated**: 2026-04-14  
**Version**: 1.0