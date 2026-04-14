# Troubleshooting Runbook

**Purpose**: Diagnose and resolve common issues in a web-based slide deck application built with React, Vite, Carbon, and route-driven navigation.  
**Audience**: Developers, operators, maintainers, presenters  
**Last Updated**: 2026-04-14

---

## Quick Diagnosis

Use this table to identify the likely issue category:

| Symptom | Category |
|---------|----------|
| Blank screen or nothing renders | Rendering issues |
| Slides do not navigate | Navigation issues |
| Styles are missing or broken | Styling issues |
| Console errors appear | JavaScript or runtime issues |
| `npm run build` fails | Build issues |
| Navigation or rendering feels slow | Performance issues |
| Dev server will not start | Server issues |

---

## Rendering Issues

### Blank screen on load

#### Symptoms

- browser opens but shows a blank page
- no visible slide content
- console may or may not contain errors

#### Common causes

##### Component is not registered

Check that the component is:

1. created
2. imported into the application shell
3. added to the slide component mapping
4. referenced correctly in [`slides.json`](../src/data/slides.json)

##### Route is invalid

Verify the URL points to a valid slide route, such as:

```text
http://localhost:3000/slide/1
```

If the project uses 1-based routes, paths such as `/slide/0` may be invalid.

##### Runtime error during mount

Open the browser console and inspect the first error. Fix syntax, import, or null-handling issues before looking elsewhere.

---

### One slide does not render

#### Symptoms

- most slides work
- one specific slide fails or appears blank
- navigation may skip or break on that slide

#### Checks

- verify the component file exists
- verify the `component` field in [`slides.json`](../src/data/slides.json:1)
- verify the component is registered in [`App.jsx`](../src/App.jsx)
- inspect the slide component for syntax or runtime errors

#### Common fix

Ensure the metadata component name exactly matches the registered component key.

Example:

```json
{
  "component": "IntroSlide"
}
```

---

## Navigation Issues

### Keyboard navigation does not work

#### Symptoms

- arrow keys do nothing
- `Home`, `End`, `PageUp`, or `PageDown` do not work

#### Checks

- confirm the browser tab is focused
- confirm focus is not inside an input or textarea
- verify the keyboard navigation hook is mounted
- inspect the console for event handler errors

#### Fixes

- click outside any editable control
- verify shared keyboard navigation is wired correctly
- confirm slide navigation logic still updates the route as expected

---

### Navigation buttons do not work

#### Symptoms

- previous and next controls appear
- clicking them does nothing

#### Checks

- confirm the slide layout receives the correct handler
- confirm the shared `onNavigate()` path still calls the route update function
- inspect the console for click handler or prop errors

---

### URL changes but slide does not update

#### Symptoms

- browser URL changes
- rendered slide does not change

#### Fixes

- hard refresh the page
- verify React Router configuration
- verify the component mapping is based on the current route parameter
- confirm state is derived from the route rather than stale local values

---

## Styling Issues

### Carbon styles do not load

#### Symptoms

- components look like unstyled HTML
- theme colors and spacing are missing

#### Checks

Verify the global stylesheet imports both base Carbon styles and any chart styles the project uses.

For example, check the global style entry referenced from [`main.jsx`](../src/main.jsx).

#### Fixes

- verify Carbon packages are installed
- verify the theme provider is applied
- verify the main global stylesheet is imported
- restart the dev server after dependency changes

---

### SCSS does not compile

#### Symptoms

- component SCSS changes do not apply
- build or console shows SCSS errors

#### Checks

```bash
npm list sass
```

#### Fixes

```bash
npm install sass --save-dev
```

Also inspect the SCSS file for:

- missing semicolons
- incorrect nesting
- invalid Carbon mixin usage
- unsupported variables or imports

---

### Styles leak between components

#### Symptoms

- one component’s styles affect another
- styling appears inconsistent or unpredictable

#### Fixes

- use unique component-scoped class names
- prefer BEM or another consistent naming convention
- reduce overly broad selectors
- avoid `!important` unless absolutely necessary

---

## JavaScript and Runtime Issues

### Cannot read property of undefined

#### Symptoms

- component crashes during render
- console shows undefined access errors

#### Fixes

- inspect the data shape before use
- use optional chaining where appropriate
- provide safe fallback values
- verify required fields exist in [`slides.json`](../src/data/slides.json) or related JSON files

Example:

```javascript
const value = data?.property?.nested ?? 'default';
```

---

### Module not found

#### Symptoms

- imports fail
- build or dev server reports resolution errors

#### Checks

- verify alias configuration in [`vite.config.js`](../vite.config.js)
- verify import spelling
- verify the file exists
- restart the dev server after alias changes

Example of a common typo:

```javascript
// Wrong
import Slide from '@component/slides/IntroSlide';

// Correct
import Slide from '@components/slides/IntroSlide';
```

---

## Build Issues

### `npm run build` fails

#### Checks

- inspect the first error in the build output
- look for syntax errors, bad imports, or missing dependencies
- confirm recent edits did not break JSON or SCSS

#### Fixes

```bash
npm install
npm run build
```

If needed, clear cached artifacts and rebuild.

---

### Build succeeds but preview fails

#### Possible causes

- incorrect `base` configuration in [`vite.config.js`](../vite.config.js)
- broken asset paths
- route fallback not configured properly for the deployment environment

#### Fixes

- verify asset imports are compatible with production
- preview again locally with [`npm run preview`](../package.json)
- compare local preview behavior with the target hosting platform

---

## Performance Issues

### Slow initial page load

#### Symptoms

- app takes too long to render the first slide
- assets appear heavy

#### Checks

- inspect generated asset sizes after build
- run Lighthouse against local preview
- review newly added dependencies or media assets

#### Fixes

- reduce oversized images or data payloads
- remove unnecessary packages
- split or defer non-critical logic when appropriate

---

### Slow navigation between slides

#### Symptoms

- slide changes feel delayed
- keyboard input feels laggy
- heavy charts or transitions stutter

#### Fixes

- inspect for excessive re-rendering
- memoize expensive components where useful
- simplify heavy animations
- reduce expensive chart recalculations inside render paths

---

## Server Issues

### Port already in use

On Windows:

```bash
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

Alternative:

```bash
npm run dev -- --port 3001
```

---

### Hot reload is not working

#### Fixes

- restart the dev server
- confirm the edited file is saved
- inspect the console for syntax errors
- check whether the changed file is actually imported into the active route
- refresh the browser if HMR state is stale

---

## Deployment Troubleshooting

### Deployed site returns 404

Common causes:

- wrong publish directory
- wrong base path
- missing SPA rewrite rules

Check the deployment guidance in [deployment.md](deployment.md).

### Direct slide routes fail in production

Cause:

- the host is not rewriting unknown routes to `index.html`

Fix:

- configure the platform for SPA fallback routing

### Assets fail to load in production

Check:

- `base` configuration
- upload completeness
- CDN cache behavior
- path correctness for built assets

---

## Emergency Procedures

### Complete local reset

Use this only when simpler fixes fail.

```bash
rm -rf node_modules
rm -rf dist
rm package-lock.json
npm install
npm run dev
```

Adjust removal commands for your shell or operating system as needed.

### Roll back to last known good version

```bash
git log --oneline
git reset --hard <commit-hash>
npm install
npm run dev
```

Use this only when it matches your team’s Git workflow.

---

## Before Asking for Help

Check the following first:

1. browser console output
2. terminal output
3. recent changes in Git
4. route and metadata alignment
5. this runbook’s matching section

When escalating, provide:

- full error message
- steps to reproduce
- expected and actual behavior
- operating system and Node version
- recent changes you made
- screenshots or console output if helpful

---

## References

- [runbooks/development-setup.md](development-setup.md)
- [runbooks/deployment.md](deployment.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- [guides/getting-started.md](../guides/getting-started.md)
- [guides/code-standards.md](../guides/code-standards.md)
- [Carbon Documentation](https://carbondesignsystem.com/)
- [React Documentation](https://react.dev/)
- [Vite Documentation](https://vitejs.dev/)

---

**Last Updated**: 2026-04-14