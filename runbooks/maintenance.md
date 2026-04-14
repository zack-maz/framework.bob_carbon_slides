# Maintenance Runbook

**Purpose**: Regular maintenance procedures for a web-based slide deck project built with React, Vite, Carbon, and JSON-driven content.  
**Audience**: Developers, maintainers, DevOps  
**Last Updated**: 2026-04-14

---

## Overview

This runbook covers routine maintenance tasks that help keep the application reliable, current, and easy to support.

It is designed for projects where:

- slide content is implemented in React
- presentation structure is configuration-driven
- deployment targets are static-hosting friendly
- documentation and architectural decisions are maintained alongside the code

---

## Maintenance Schedule

| Task | Frequency | Estimated Time | Priority |
|------|-----------|----------------|----------|
| Dependency updates | Weekly | 15–30 min | High |
| Security audit | Weekly | 10–15 min | Critical |
| Content and documentation review | Monthly | 30–60 min | Medium |
| Performance audit | Monthly | 20–30 min | Medium |
| Backup verification | Monthly | 10–20 min | High |
| Full system review | Quarterly | 2–3 hours | High |

---

## Weekly Maintenance

### Task 1: Dependency updates

**Objective**: Keep dependencies current and reduce drift.

#### Procedure

```bash
npm outdated
npm update
npm run dev
npm run build
npm run preview
```

For major version updates:

- review release notes before updating
- update one major dependency at a time
- validate core workflows after each change

#### Verification checklist

- [ ] dependency updates reviewed
- [ ] no breaking changes introduced
- [ ] dev server still starts
- [ ] build succeeds
- [ ] preview works
- [ ] key slides render correctly

#### Rollback

```bash
git revert HEAD
npm install
```

---

### Task 2: Security audit

**Objective**: Identify and address vulnerabilities.

#### Procedure

```bash
npm audit
npm audit fix
npm run build
```

Use `npm audit fix --force` only after reviewing the potential impact.

#### If critical issues are found

1. assess whether production is affected
2. prioritize the smallest safe fix
3. test thoroughly
4. deploy quickly if the risk is real
5. document the remediation

---

## Monthly Maintenance

### Task 3: Content and documentation review

**Objective**: Keep content, examples, and workflows accurate.

#### Review areas

- slide metadata in [`slides.json`](../src/data/slides.json)
- supporting data in `src/data/`
- [README.md](../README.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- guides and runbooks
- ADR relevance and completeness

#### Verification checklist

- [ ] outdated content identified and updated
- [ ] examples still reflect current implementation
- [ ] document links still work
- [ ] new workflow changes documented
- [ ] terminology remains generalized and consistent

---

### Task 4: Performance audit

**Objective**: Maintain acceptable performance for local preview and deployed use.

#### Procedure

```bash
npm run build
npx lighthouse http://localhost:4173/ --view
```

Also inspect generated assets under `dist/`.

#### Review focus

- total build size
- route load behavior
- responsiveness during navigation
- chart or visualization performance
- unnecessary dependency growth

#### If performance degrades

- identify newly added heavy dependencies
- review repeated rendering or expensive calculations
- optimize large media assets
- look for opportunities to extract or lazy-load non-critical code

---

### Task 5: Backup verification

**Objective**: Ensure the project can be restored from source and artifacts if needed.

#### Procedure

```bash
git remote -v
npm run build
```

Recommended backup steps:

1. confirm the remote repository is accessible
2. create an archive excluding `node_modules/` and build output
3. test restore in a temporary directory
4. confirm the restored project installs and builds

#### Verification checklist

- [ ] repository remote is accessible
- [ ] backup archive created
- [ ] restore procedure tested
- [ ] restored copy builds successfully
- [ ] obsolete backups rotated according to team policy

---

## Quarterly Maintenance

### Task 6: Full system review

**Objective**: Review architecture, dependencies, accessibility, documentation, and operations as a whole.

#### Suggested review areas

##### 1. Major dependency updates

- inspect `npm outdated`
- read changelogs for major packages
- test updates incrementally
- merge only after full validation

##### 2. Code quality review

Inspect for:

- duplicate logic
- oversized components
- unclear naming
- undocumented behavior
- stale TODO or FIXME items

##### 3. Documentation review

Review:

- top-level docs
- guides
- runbooks
- ADRs
- diagrams
- data references

Make sure docs reflect the current implementation rather than earlier planning states.

##### 4. Accessibility audit

Use Lighthouse and manual keyboard testing to confirm:

- focus indicators remain visible
- navigation is keyboard accessible
- labels are clear
- color contrast remains acceptable

##### 5. Browser compatibility review

Test in current versions of:

- Chrome
- Firefox
- Safari
- Edge

##### 6. Security review

Review:

- dependency vulnerability state
- accidental secret exposure
- deployment security headers
- client-side assumptions about public data

---

## Emergency Maintenance

### When to use emergency maintenance

Use this path when:

- a critical vulnerability is discovered
- production is unavailable
- routing is broken in production
- a deployment causes major rendering or navigation failure

### Procedure

1. assess severity and blast radius
2. create a minimal hotfix branch if needed
3. implement the smallest safe fix
4. validate locally
5. deploy through the normal process if possible
6. verify the fix in production
7. document what happened and what changed

Example flow:

```bash
git checkout -b hotfix/critical-issue
npm run dev
npm run build
npm run preview
```

---

## Maintenance Records

Track maintenance activity in simple logs or team systems.

Recommended categories:

- dependency updates
- security remediations
- performance checks
- documentation audits
- incident and rollback records

Example plain-text record:

```text
2026-04-14: Weekly dependency review completed, no major issues found
2026-04-14: Security audit completed, no critical vulnerabilities
2026-04-14: Documentation reviewed for generalized terminology
```

---

## Maintenance Checklists

### Weekly

- [ ] check dependency updates
- [ ] run security audit
- [ ] review recent errors or regressions
- [ ] verify deployment path is still healthy

### Monthly

- [ ] review content and docs
- [ ] run performance audit
- [ ] verify backups
- [ ] test key workflows end to end

### Quarterly

- [ ] review major dependencies
- [ ] review code quality
- [ ] review all documentation
- [ ] run accessibility checks
- [ ] test browser compatibility
- [ ] review security posture

---

## Troubleshooting Maintenance Issues

### Dependency update breaks the build

```bash
git revert HEAD
npm install
```

Then reintroduce updates one at a time.

### Security fix introduces breaking changes

- inspect the package changelog
- identify the breaking API surface
- adapt the app if the upgrade is worthwhile
- otherwise pin or replace the dependency according to team policy

### Performance regresses after an update

- identify the change that caused the regression
- compare build output before and after
- inspect runtime behavior with browser dev tools
- roll back or optimize as needed

---

## Best Practices

### Do

- keep updates incremental
- test after every meaningful change
- document procedure changes
- maintain a regular review cadence
- keep backups and rollback paths current

### Avoid

- updating everything at once without testing
- ignoring audit results
- skipping preview verification
- leaving documentation drift unresolved
- making emergency fixes without recording what changed

---

## References

- [README.md](../README.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- [runbooks/deployment.md](deployment.md)
- [runbooks/troubleshooting.md](troubleshooting.md)
- [guides/contributing.md](../guides/contributing.md)
- [Vite Documentation](https://vitejs.dev/)
- [React Documentation](https://react.dev/)
- [Carbon Documentation](https://carbondesignsystem.com/)

---

**Last Updated**: 2026-04-14