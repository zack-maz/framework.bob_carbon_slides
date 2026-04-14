# Handoff Documentation

**Project**: General Slide Builder Documentation  
**Date**: 2026-04-14  
**Status**: Active  
**Documentation Level**: Operationally Maintained

---

## Purpose

This document provides the information needed to maintain, update, and extend a web-based slide deck project built with Bob, React, Vite, Carbon Design System, and supporting browser validation workflows. The project leverages MCP servers (Carbon MCP and Playwright MCP) for enhanced development capabilities.

It is intended to help:

- new developers onboard quickly
- maintainers keep documentation and procedures current
- teams transfer project context without relying on tribal knowledge
- stakeholders understand how the documentation is organized and maintained

---

## Who Should Use This Document

| Audience | Primary Use |
|----------|-------------|
| New developers | Understand where to start and how to make safe changes |
| Maintainers | Follow recurring maintenance and review procedures |
| Documentation owners | Keep guides, runbooks, and ADRs current |
| Technical leads | Preserve architectural intent across iterations |

---

## First Steps by Role

### For New Developers

1. Read [README.md](README.md)
2. Complete [guides/getting-started.md](guides/getting-started.md)
3. Review [main/ARCHITECTURE.md](main/ARCHITECTURE.md)
4. Scan [guides/code-standards.md](guides/code-standards.md)

### For Maintainers

1. Review [runbooks/maintenance.md](runbooks/maintenance.md)
2. Review [runbooks/troubleshooting.md](runbooks/troubleshooting.md)
3. Review [runbooks/deployment.md](runbooks/deployment.md)
4. Confirm [main/INDEX.md](main/INDEX.md) reflects the current document set

### For Documentation Owners

1. Review [main/INDEX.md](main/INDEX.md)
2. Check ADR accuracy in [adr/](adr/)
3. Validate examples in guides and runbooks
4. Update document timestamps and cross-references as needed

### For Technical Leads

1. Review [main/ARCHITECTURE.md](main/ARCHITECTURE.md)
2. Review ADRs in [adr/](adr/)
3. Check [diagrams/](diagrams/) for architectural consistency
4. Confirm implementation guidance still matches runtime behavior

---

## Documentation Structure

### Core Documents

| Document | Purpose |
|----------|---------|
| [README.md](README.md) | Project overview and working model |
| [main/INDEX.md](main/INDEX.md) | Central navigation hub |
| [main/ARCHITECTURE.md](main/ARCHITECTURE.md) | Runtime structure and implementation model |
| [HANDOFF_DOCUMENTATION.md](HANDOFF_DOCUMENTATION.md) | Continuity, maintenance, and ownership guidance |

### Architecture Decision Records

ADRs record why major technical choices were made.

Current ADR set:

- [adr/0001-use-react-router.md](adr/0001-use-react-router.md)
- [adr/0002-carbon-design-system.md](adr/0002-carbon-design-system.md)
- [adr/0003-vite-build-tool.md](adr/0003-vite-build-tool.md)
- [adr/0004-json-slide-configuration.md](adr/0004-json-slide-configuration.md)
- [adr/0005-component-based-architecture.md](adr/0005-component-based-architecture.md)
- [adr/0006-path-aliases.md](adr/0006-path-aliases.md)

### Guides

Guides focus on onboarding, contribution, and implementation expectations.

- [guides/getting-started.md](guides/getting-started.md)
- [guides/contributing.md](guides/contributing.md)
- [guides/code-standards.md](guides/code-standards.md)

### Runbooks

Runbooks describe repeatable operational procedures.

- [runbooks/development-setup.md](runbooks/development-setup.md)
- [runbooks/deployment.md](runbooks/deployment.md)
- [runbooks/maintenance.md](runbooks/maintenance.md)
- [runbooks/troubleshooting.md](runbooks/troubleshooting.md)

### Supporting References

- [data/data-dictionary.md](data/data-dictionary.md)
- [data/data-lineage.md](data/data-lineage.md)
- [diagrams/system-context.md](diagrams/system-context.md)
- [diagrams/component-architecture.md](diagrams/component-architecture.md)
- [diagrams/data-flow.md](diagrams/data-flow.md)
- [diagrams/navigation-flow.md](diagrams/navigation-flow.md)

---

## Maintenance Responsibilities

### Documentation Owners Should

- keep high-level guidance accurate
- update examples when implementation patterns change
- maintain cross-links between related documents
- ensure terminology remains general and reusable
- remove project-specific branding when it is not core to the workflow

### Technical Maintainers Should

- validate that documented commands still work
- confirm routing and metadata guidance matches the app
- review architecture docs after structural changes
- update diagrams when component or data flow changes

### Operational Maintainers Should

- keep deployment and troubleshooting procedures current
- verify local setup instructions remain accurate
- test recovery procedures after important infrastructure or workflow changes

---

## Recurring Review Schedule

### Weekly

- check for broken workflows during active development
- note any documentation drift discovered during implementation
- confirm major examples still reflect current patterns

### Monthly

- review guides and runbooks for outdated steps
- verify command examples still work
- check links and internal references
- verify deployment notes still match the active release process

### Quarterly

- review all ADRs for continued relevance
- perform a full documentation audit
- confirm diagrams match actual architecture
- review terminology for clarity and consistency
- confirm handoff information still reflects team ownership

---

## Common Maintenance Tasks

### Adding a New Slide

1. Create the component under `src/components/slides/`
2. Register it in [`App.jsx`](src/App.jsx)
3. Add metadata in [`slides.json`](src/data/slides.json)
4. Validate routing and navigation behavior
5. Update docs if the change introduces a new reusable pattern

### Updating the Slide Model

If slide metadata changes:

1. Update [data/data-dictionary.md](data/data-dictionary.md)
2. Update [data/data-lineage.md](data/data-lineage.md)
3. Update any affected examples in guides or runbooks
4. Confirm [main/ARCHITECTURE.md](main/ARCHITECTURE.md) still reflects runtime behavior

### Changing Navigation Behavior

If routing or keyboard controls change:

1. Update [main/ARCHITECTURE.md](main/ARCHITECTURE.md)
2. Update [diagrams/navigation-flow.md](diagrams/navigation-flow.md)
3. Update [runbooks/troubleshooting.md](runbooks/troubleshooting.md) if failure modes change
4. Create or update an ADR if the change is architectural

### Changing Build or Tooling

If Vite, path aliases, testing strategy, or related tooling changes:

1. Update the relevant ADR
2. Update [guides/getting-started.md](guides/getting-started.md)
3. Update [runbooks/development-setup.md](runbooks/development-setup.md)
4. Update [main/ARCHITECTURE.md](main/ARCHITECTURE.md) if runtime implications exist

---

## Change Control Guidance

Update documentation when any of the following occurs:

- architectural decisions change
- build or deployment commands change
- routing behavior changes
- slide metadata schema changes
- new recurring maintenance procedures are introduced
- debugging or recovery steps change
- terminology becomes too project-specific or outdated

When updating documents:

1. Identify all affected files
2. Update the primary source document first
3. Fix cross-references and examples
4. Review related runbooks, guides, diagrams, and ADRs
5. Update timestamps where your team’s process requires them

---

## Knowledge Transfer Checklist

### For Incoming Team Members

- [ ] Read [README.md](README.md)
- [ ] Review [main/INDEX.md](main/INDEX.md)
- [ ] Complete [guides/getting-started.md](guides/getting-started.md)
- [ ] Review [main/ARCHITECTURE.md](main/ARCHITECTURE.md)
- [ ] Review ADRs in [adr/](adr/)
- [ ] Review coding expectations in [guides/code-standards.md](guides/code-standards.md)
- [ ] Review maintenance procedures in [runbooks/maintenance.md](runbooks/maintenance.md)

### For Outgoing Team Members

- [ ] Document in-progress work
- [ ] Note known issues and workarounds
- [ ] Update affected guides or runbooks
- [ ] Record architecture changes in ADRs when needed
- [ ] Confirm [main/INDEX.md](main/INDEX.md) still reflects the active document set

---

## Success Criteria

A healthy documentation set should make it possible to:

- onboard a developer quickly
- add or replace a slide safely
- troubleshoot common runtime issues without guesswork
- understand why major technical choices were made
- maintain the project without relying on original authors

---

## External References

- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)
- [React Router Documentation](https://reactrouter.com/)
- [Carbon Design System](https://carbondesignsystem.com/)
- [Playwright Documentation](https://playwright.dev/)
- [ADR Guidance](https://adr.github.io/)

---

## Conclusion

This handoff guide is designed to preserve continuity for a reusable slide-building project rather than a single branded presentation. It should evolve alongside the architecture, tooling, and team workflow.

**Last Updated**: 2026-04-14  
**Review Cadence**: Quarterly  
**Status**: Active