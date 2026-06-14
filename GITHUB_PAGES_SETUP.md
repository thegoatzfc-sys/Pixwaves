# GitHub Pages Deployment Guide

## Setup Instructions

### 1. Create GitHub Pages Workflow

Create a new file `.github/workflows/deploy.yml` in your repository:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm install
      
      - name: Build
        run: npm run build
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './dist'

  deploy:
    if: github.ref == 'refs/heads/main'
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

### 2. Configure GitHub Pages

1. Go to your repository **Settings**
2. Navigate to **Pages** section
3. Under "Build and deployment":
   - Select **Source**: "GitHub Actions"
   - This will use the workflow we created above

### 3. Enable GitHub Pages

Your site will be available at: `https://thegoatzfc-sys.github.io/pixwaves/`

### 4. Local Testing

Build locally and test before pushing:

```bash
npm run build
npm run preview
```

This will start a local server showing exactly what will be deployed.

## Build Configuration

The `vite.config.ts` already includes:
- `base: '/pixwaves/'` - Correct path for GitHub Pages subpath deployment
- `outDir: 'dist'` - Output directory for deployment

## Push & Deploy

Once you push to `main` branch:

1. GitHub Actions workflow runs automatically
2. Your app builds with the correct paths
3. Deploys to GitHub Pages
4. Live at `https://thegoatzfc-sys.github.io/pixwaves/`

## Troubleshooting

**Assets not loading?**
- Verify `base: '/pixwaves/'` is in `vite.config.ts`

**Workflow not running?**
- Check repository Settings > Actions > General
- Ensure "Allow all actions and reusable workflows" is selected

**Page shows 404?**
- Go to Settings > Pages
- Confirm "Source" is set to "GitHub Actions"
