# Deployment Runbook

**Purpose**: Step-by-step guidance for deploying a React- and Vite-based slide deck application to static hosting or internal web infrastructure.  
**Audience**: DevOps, deployment engineers, maintainers, developers  
**Last Updated**: 2026-04-14

---

## Overview

This runbook covers deployment procedures for a static React SPA used as a web-based slide deck.

Because the application compiles to static assets, it can be deployed to many common hosting targets, including:

- GitHub Pages
- Netlify
- Vercel
- object storage plus CDN
- internal web servers
- other static hosting environments

Use this runbook for:

- pre-release validation
- production deployment
- rollback preparation
- post-deployment verification

---

## Pre-Deployment Checklist

Before deploying, verify:

- [ ] all intended changes are committed
- [ ] the working tree is clean
- [ ] the project builds successfully
- [ ] the preview build was tested locally
- [ ] required environment variables are configured
- [ ] deployment credentials are available
- [ ] a rollback path is defined

Suggested pre-deployment commands:

```bash
git status
git pull origin main
npm install
npm run build
npm run preview
```

During local preview, confirm:

- the app loads successfully
- key slides render
- navigation works
- no critical console errors appear

---

## Deployment Targets

### Option 1: GitHub Pages

**Best for**: simple public deployments connected to GitHub

#### One-time setup

Install [`gh-pages`](package.json:1) if your workflow uses it:

```bash
npm install --save-dev gh-pages
```

Add deployment scripts to [`package.json`](../package.json), for example:

```json
{
  "scripts": {
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  }
}
```

If deploying under a repository path, configure the correct `base` setting in [`vite.config.js`](../vite.config.js).

#### Deployment steps

```bash
git add .
git commit -m "chore(deploy): prepare release"
git push origin main
npm run deploy
```

#### Verification

- [ ] site loads at the expected GitHub Pages URL
- [ ] routes behave correctly for the hosting setup
- [ ] styles and assets load correctly
- [ ] no blocking console errors appear

---

### Option 2: Netlify

**Best for**: managed static hosting with automatic builds and previews

#### Typical configuration

- build command: `npm run build`
- publish directory: `dist`

Optional [`netlify.toml`](../netlify.toml) example:

```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

#### Deployment

Automatic deployment usually occurs after pushing to the tracked branch.

Manual CLI deployment example:

```bash
npm install -g netlify-cli
netlify login
npm run build
netlify deploy --prod --dir=dist
```

#### Verification

- [ ] build succeeds
- [ ] site loads at the deployed URL
- [ ] SPA routing works
- [ ] assets and styles load correctly

---

### Option 3: Vercel

**Best for**: fast managed deployments for front-end applications

#### Typical configuration

Vercel usually detects the Vite project automatically.

Optional [`vercel.json`](../vercel.json) example:

```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "devCommand": "npm run dev",
  "installCommand": "npm install"
}
```

#### Deployment

Automatic deployment can be triggered by pushing to the connected branch.

Manual deployment example:

```bash
npm install -g vercel
vercel login
vercel --prod
```

#### Verification

- [ ] build succeeds
- [ ] site loads at the deployed URL
- [ ] route-based navigation works
- [ ] performance is acceptable

---

### Option 4: Object storage plus CDN

**Best for**: scalable deployments on cloud infrastructure

This pattern works with providers such as AWS, Azure, GCP, or other storage-plus-CDN combinations.

#### General process

1. build the project
2. upload `dist/` to static storage
3. configure SPA fallback routing if needed
4. invalidate CDN cache if applicable

Example command pattern:

```bash
npm run build
```

Then upload the generated `dist/` directory using your platform’s CLI or console.

#### Verification

- [ ] all build artifacts uploaded
- [ ] CDN or storage endpoint serves the app
- [ ] index fallback is configured for SPA routes
- [ ] caching rules behave as expected
- [ ] HTTPS works if required

---

### Option 5: Internal web server

**Best for**: internal presentation environments or corporate hosting

#### General process

1. build the app
2. copy the contents of `dist/` to the target server
3. configure the web server for SPA route fallback
4. verify permissions, paths, and caching

Example packaging and transfer pattern:

```bash
npm run build
tar -czf slide-deck-deployment.tar.gz dist/
```

Then transfer and unpack according to your server environment.

#### Example Nginx configuration

```nginx
server {
    listen 80;
    server_name slides.example.com;
    root /var/www/slides;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

#### Example Apache rewrite rule

```apache
<IfModule mod_rewrite.c>
  RewriteEngine On
  RewriteBase /
  RewriteRule ^index\.html$ - [L]
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_FILENAME} !-d
  RewriteRule . /index.html [L]
</IfModule>
```

---

## Post-Deployment Verification

### Manual verification checklist

After deployment, verify:

- [ ] the root URL loads
- [ ] multiple slide routes load correctly
- [ ] keyboard navigation works
- [ ] UI controls work
- [ ] no critical console errors appear
- [ ] styles render correctly
- [ ] charts or visualizations render correctly
- [ ] responsive behavior is acceptable
- [ ] page load performance is reasonable

### Command-line verification examples

```bash
curl -I https://your-domain.example
curl -I https://your-domain.example/slide/1
```

These checks help confirm that the app and a direct route both resolve successfully.

### Performance verification

Lighthouse can be used for a basic check:

```bash
npx lighthouse https://your-domain.example --view
```

---

## Rollback Procedures

### Quick rollback

Use the hosting platform’s simplest reliable rollback method.

Examples:

- republish the previous successful deploy in Netlify or Vercel
- redeploy a known-good artifact
- restore the previous storage sync to your static host
- revert the release commit and redeploy

Example Git-based rollback flow:

```bash
git revert HEAD
git push origin main
```

Then redeploy using the normal deployment path.

### Emergency rollback

If the deployment is severely broken:

1. identify the last known good version
2. restore or redeploy that version
3. verify routing and critical slides
4. investigate the failed release separately

Use force-push rollback only if it matches your team’s Git policy.

---

## Monitoring and Maintenance

After deployment, monitor:

- availability
- page load performance
- console or runtime errors
- asset delivery
- route behavior for direct slide URLs

Recommended review cadence:

### Weekly

- check deployment logs
- review reported errors
- verify uptime

### Monthly

- test core functionality
- verify content remains current
- review hosting or cache behavior

### Quarterly

- run a broader security and performance review
- retest browser compatibility
- confirm the deployment procedure remains accurate

---

## Troubleshooting Deployments

### Build fails

If [`npm run build`](../package.json) fails:

- check for syntax errors
- verify dependencies installed correctly
- clear cached build artifacts if needed
- reinstall dependencies

Example:

```bash
npm install
npm run build
```

### Site shows 404

Possible causes:

- incorrect base path configuration
- incorrect publish directory
- missing SPA route fallback
- deployment to the wrong target path

Check [`vite.config.js`](../vite.config.js) and hosting rewrite rules.

### Assets do not load

Possible causes:

- broken asset paths
- incorrect `base` configuration
- partial upload of `dist/`
- incorrect caching or CDN behavior

### Direct slide routes return 404

Cause:

- the host is not configured to rewrite unknown routes to `index.html`

Fix:

- add SPA rewrite or redirect rules for the hosting platform

---

## Security Considerations

Before deployment, verify:

- [ ] no secrets are committed to the repository
- [ ] no client-side API keys expose sensitive access
- [ ] dependencies are reasonably current
- [ ] `npm audit` findings have been reviewed
- [ ] HTTPS is enabled where required
- [ ] security headers are configured where possible

Example security headers to consider:

```text
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline';
X-Frame-Options: SAMEORIGIN
X-Content-Type-Options: nosniff
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

Adjust these headers to match your actual hosting needs and any third-party assets you use.

---

## Deployment Checklist

### Pre-deployment

- [ ] code reviewed
- [ ] build verified locally
- [ ] preview verified locally
- [ ] rollback path confirmed
- [ ] deployment target prepared

### Deployment

- [ ] production build created
- [ ] artifacts deployed
- [ ] deployment completed successfully
- [ ] post-deployment checks executed

### Post-deployment

- [ ] routing verified
- [ ] styling and assets verified
- [ ] no critical console errors
- [ ] performance acceptable
- [ ] documentation updated if deployment procedure changed

---

## References

- [README.md](../README.md)
- [main/ARCHITECTURE.md](../main/ARCHITECTURE.md)
- [runbooks/troubleshooting.md](troubleshooting.md)
- [runbooks/maintenance.md](maintenance.md)
- [Vite Documentation](https://vitejs.dev/)
- [Netlify Documentation](https://docs.netlify.com/)
- [Vercel Documentation](https://vercel.com/docs)

---

**Last Updated**: 2026-04-14