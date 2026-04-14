# Documentation Index

**Central navigation hub for the slide-building documentation set**

---

## Start Here

Recommended reading order for new contributors and maintainers:

1. **[README](../README.md)** - Project overview and workflow summary
2. **[Getting Started Guide](../guides/getting-started.md)** - Environment setup and first changes
3. **[Architecture Overview](ARCHITECTURE.md)** - Application structure and runtime model
4. **[Handoff Documentation](../HANDOFF_DOCUMENTATION.md)** - Maintenance, review, and ownership guidance

---

## Documentation Categories

### Architecture

| Document | Description | Status |
|----------|-------------|--------|
| [Architecture Overview](ARCHITECTURE.md) | System architecture and runtime model | Active |
| [0001](../adr/0001-use-react-router.md) | Use React Router for slide navigation | Accepted |
| [0002](../adr/0002-carbon-design-system.md) | Adopt Carbon Design System | Accepted |
| [0003](../adr/0003-vite-build-tool.md) | Use Vite as build tool | Accepted |
| [0004](../adr/0004-json-slide-configuration.md) | Use JSON-based slide configuration | Accepted |
| [0005](../adr/0005-component-based-architecture.md) | Use component-based slide architecture | Accepted |
| [0006](../adr/0006-path-aliases.md) | Use path aliases for import management | Accepted |

### Guides

| Guide | Description | Status |
|-------|-------------|--------|
| [Getting Started](../guides/getting-started.md) | Onboarding and first-run guide | Active |
| [Contributing](../guides/contributing.md) | Contribution workflow and review expectations | Active |
| [Code Standards](../guides/code-standards.md) | Coding conventions and implementation guidance | Active |

### Runbooks

| Runbook | Description | Status |
|---------|-------------|--------|
| [Development Setup](../runbooks/development-setup.md) | Local environment setup and verification | Active |
| [Deployment](../runbooks/deployment.md) | Build, release, and hosting procedures | Active |
| [Maintenance](../runbooks/maintenance.md) | Recurring maintenance and review checklist | Active |
| [Troubleshooting](../runbooks/troubleshooting.md) | Diagnosis and recovery procedures | Active |

### Data Documentation

| Document | Description | Status |
|----------|-------------|--------|
| [Data Dictionary](../data/data-dictionary.md) | Reference for slide and content data structures | Active |
| [Data Lineage](../data/data-lineage.md) | Data flow through the application | Active |

### Diagrams

| Diagram | Description | Status |
|---------|-------------|--------|
| [System Context](../diagrams/system-context.md) | High-level system boundaries and actors | Active |
| [Component Architecture](../diagrams/component-architecture.md) | Relationships among runtime components | Active |
| [Data Flow](../diagrams/data-flow.md) | Content and configuration flow through the app | Active |
| [Navigation Flow](../diagrams/navigation-flow.md) | Routing and movement between slides | Active |

### Supporting Documentation

| Document | Description | Status |
|----------|-------------|--------|
| [History](HISTORY.md) | Documentation generalization history | Historical |
| [Handoff Documentation](../HANDOFF_DOCUMENTATION.md) | Operational ownership and continuity guide | Active |

---

## Documentation by Audience

### For New Developers

1. [README](../README.md)
2. [Getting Started](../guides/getting-started.md)
3. [Architecture Overview](ARCHITECTURE.md)
4. [Code Standards](../guides/code-standards.md)

### For Contributors

1. [Contributing](../guides/contributing.md)
2. [Code Standards](../guides/code-standards.md)
3. [Development Setup](../runbooks/development-setup.md)
4. [Architecture ADRs](../adr/)

### For Maintainers

1. [Handoff Documentation](../HANDOFF_DOCUMENTATION.md)
2. [Maintenance](../runbooks/maintenance.md)
3. [Troubleshooting](../runbooks/troubleshooting.md)
4. [Deployment](../runbooks/deployment.md)

### For Architects

1. [Architecture Overview](ARCHITECTURE.md)
2. [All ADRs](../adr/)
3. [System Diagrams](../diagrams/)
4. [Data Documentation](../data/)

---

## How to Use This Documentation

### When building a new deck

Start with:

- [README](../README.md)
- [Getting Started](../guides/getting-started.md)
- [Architecture Overview](ARCHITECTURE.md)

### When changing implementation details

Review:

- [Architecture Overview](ARCHITECTURE.md)
- [Code Standards](../guides/code-standards.md)
- relevant ADRs in [../adr/](../adr/)

### When changing operational procedures

Update:

- the relevant runbook in [../runbooks/](../runbooks/)
- cross-references in [INDEX.md](INDEX.md)
- any affected overview guidance in [../README.md](../README.md) or [../HANDOFF_DOCUMENTATION.md](../HANDOFF_DOCUMENTATION.md)

### When changing data structures

Update:

- [Data Dictionary](../data/data-dictionary.md)
- [Data Lineage](../data/data-lineage.md)
- any affected examples in guides or runbooks

---

## External Resources

### Core Technologies

- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)
- [React Router Documentation](https://reactrouter.com/)
- [Carbon Design System](https://carbondesignsystem.com/)
- [Playwright Documentation](https://playwright.dev/)
- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [Mermaid Documentation](https://mermaid.js.org/)

### Documentation Practices

- [ADR Guidance](https://adr.github.io/)
- [Markdown Guide](https://www.markdownguide.org/)

---

## Documentation Maintenance

Update this index when:

- a document is added or removed
- a file is renamed
- the intended audience changes
- a category or workflow changes significantly
- architectural or operational guidance is restructured

**Last Updated**: 2026-04-14  
**Review Cadence**: Quarterly  
**Scope**: Generalized slide-building documentation