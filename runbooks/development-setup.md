# Development Setup Runbook

**Purpose**: Step-by-step instructions for setting up a local development environment for a React- and Vite-based slide deck project.  
**Audience**: Developers, maintainers, new team members  
**Last Updated**: 2026-04-14

---

## Overview

This runbook explains how to prepare a local environment for working on a slide deck application built with:

- React
- Vite
- Carbon Design System
- JSON-driven slide metadata
- optional browser automation or Playwright-based verification
- MCP servers (Carbon MCP and Playwright MCP) for enhanced development workflows

Use this runbook when:

- onboarding a new developer
- recreating a local environment
- validating that the project still builds and runs correctly

---

## Prerequisites

### Required software

| Software | Minimum Version | Recommended Version | Purpose |
|----------|-----------------|---------------------|---------|
| Node.js | 18.0.0 | Current LTS | JavaScript runtime |
| npm | 9.0.0 | Current stable | Package management |
| Git | 2.30.0 | Latest | Version control |
| VS Code | 1.80.0 | Latest | Recommended editor |

### System requirements

- Windows, macOS, or Linux
- 4 GB RAM minimum, 8 GB recommended
- enough disk space for the repository and dependencies
- internet access for initial dependency installation

---

## Step 1: Clone the repository

```bash
git clone <repository-url>
cd slides
```

Optional verification:

```bash
git status
git config --list
```

Expected result:

- repository is cloned successfully
- you are in the project root
- Git is available and configured

---

## Step 2: Verify runtime tools

Check installed versions:

```bash
node --version
npm --version
```

Expected result:

- Node.js is version 18 or higher
- npm is version 9 or higher

If versions are older than required, update Node.js before proceeding.

---

## Step 3: Install dependencies

```bash
npm install
```

Expected result:

- `node_modules/` is created
- dependency installation completes without blocking errors
- lockfile is created or updated according to repository conventions

Optional verification:

```bash
npm list react react-dom @carbon/react vite
```

---

## Step 4: Configure your editor

### Recommended VS Code extensions

Suggested extensions:

- ESLint
- Prettier
- SCSS IntelliSense
- Path Intellisense
- React snippet support

Optional installation commands:

```bash
code --install-extension dbaeumer.vscode-eslint
code --install-extension esbenp.prettier-vscode
code --install-extension mrmlnc.vscode-scss
code --install-extension christian-kohler.path-intellisense
code --install-extension dsznajder.es7-react-js-snippets
```

### Path alias support

Most projects expose aliases through [`vite.config.js`](../vite.config.js). If your editor does not resolve them automatically, add a [`jsconfig.json`](../jsconfig.json) file that mirrors the alias configuration.

Example:

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
  },
  "exclude": ["node_modules", "dist"]
}
```

---

## Step 5: Start the development server

```bash
npm run dev
```

Typical expected output includes a local URL such as:

```text
http://localhost:3000/
```

Open the app in the browser if it does not open automatically.

Expected result:

- the app loads successfully
- the first slide renders
- no blocking runtime errors appear in the browser console
- route navigation works

---

## Step 6: Verify hot reload

Make a small change to a slide or layout component and save the file.

Examples:

- change a heading in a slide component
- adjust spacing in a component SCSS file
- update metadata in [`slides.json`](../src/data/slides.json)

Expected result:

- the browser updates automatically
- the change appears without a full rebuild in most cases

---

## Step 7: Verify project structure

Confirm key files exist:

```bash
dir src
dir src\components
dir src\data
```

At a minimum, verify the presence of:

- [`src/App.jsx`](../src/App.jsx)
- [`src/main.jsx`](../src/main.jsx)
- [`src/data/slides.json`](../src/data/slides.json)
- [`vite.config.js`](../vite.config.js)
- [`package.json`](../package.json)

---

## Step 8: Run a production build

```bash
npm run build
```

Expected result:

- production assets are generated in `dist/`
- the build completes without blocking errors

Then preview the build:

```bash
npm run preview
```

Open the preview URL, often `http://localhost:4173/`, and verify:

- slides render correctly
- navigation works
- styles are present
- no major console errors appear

---

## Step 9: Verify core workflows

Before considering setup complete, confirm the following:

### Navigation

- keyboard navigation works
- previous and next controls work
- direct slide routes work if route-based navigation is enabled

### Rendering

- the first slide renders
- at least one additional slide renders correctly
- layout elements appear as expected

### Console health

- no blocking console errors
- no obvious asset loading failures
- expected dependencies load correctly

### Responsive behavior

Use browser dev tools to test a few viewport sizes and confirm the layout does not break in obvious ways.

---

## Troubleshooting

### `npm install` fails

Try:

```bash
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

On Windows, remove folders manually if shell support differs.

### Port 3000 is already in use

On Windows:

```bash
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

Or start on a different port:

```bash
npm run dev -- --port 3001
```

### Alias or module resolution errors

If imports such as `@components/...` fail:

- restart the dev server
- restart the editor
- verify alias configuration in [`vite.config.js`](../vite.config.js)
- add or correct [`jsconfig.json`](../jsconfig.json) if needed

### Styles are not loading

Verify required styling packages:

```bash
npm list sass
```

If missing:

```bash
npm install sass --save-dev
```

Also verify global styles are imported in [`main.jsx`](../src/main.jsx).

### Hot reload is not working

- save the changed file again
- restart the dev server
- check for syntax errors
- refresh the browser
- clear the browser cache if behavior seems stale

---

## Environment Variables

Some projects do not require environment variables. If this project introduces them later:

1. create a `.env` file at the project root
2. prefix browser-exposed values with `VITE_`
3. access them using `import.meta.env`
4. restart the dev server after changes

Example:

```text
VITE_API_URL=https://example.com/api
```

---

## Completion Checklist

Use this checklist to confirm setup is complete:

- [ ] Node.js is installed and supported
- [ ] npm is installed and supported
- [ ] repository is cloned locally
- [ ] dependencies install successfully
- [ ] dev server starts without blocking errors
- [ ] local app loads in the browser
- [ ] first slide renders correctly
- [ ] navigation works
- [ ] hot reload works
- [ ] production build succeeds
- [ ] preview build loads correctly

---

## Next Steps

After completing setup:

1. read [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
2. review [guides/getting-started.md](../guides/getting-started.md)
3. review [guides/code-standards.md](../guides/code-standards.md)
4. review [guides/contributing.md](../guides/contributing.md)
5. review [runbooks/troubleshooting.md](troubleshooting.md)

---

## Useful Commands

```bash
npm run dev
npm run build
npm run preview
npm install
npm update
npm audit
git status
git add .
git commit -m "msg"
git push
```

---

## References

- [README.md](../README.md)
- [main/INDEX.md](../main/INDEX.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- [guides/getting-started.md](../guides/getting-started.md)
- [runbooks/troubleshooting.md](troubleshooting.md)

---

**Last Updated**: 2026-04-14