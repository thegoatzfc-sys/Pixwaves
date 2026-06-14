# 🎮 Deploy Pixwaves to GitHub Pages - Complete Beginner Guide

## Step 1: Create the Workflow File on GitHub (Easiest Way!)

1. **Go to your repository** on GitHub: https://github.com/thegoatzfc-sys/pixwaves
2. **Click on the "Add file" button** (top right, green button)
3. **Select "Create new file"**
4. **In the filename box, type this EXACT path:**
   ```
   .github/workflows/deploy.yml
   ```
   (GitHub will create the folders automatically)

5. **Copy and paste this code into the file:**
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

6. **Scroll to the bottom and click "Commit changes"**
7. **Add a message** (it's optional, but nice): `Add GitHub Pages deployment workflow`
8. **Click "Commit changes" button again**

---

## Step 2: Turn On GitHub Pages

1. **Go to your repo Settings** (click the gear icon ⚙️ at the top)
2. **Scroll down the left sidebar and click "Pages"**
3. **Under "Build and deployment" section:**
   - **Source**: Select **"GitHub Actions"** (dropdown menu)
4. **Done!** Leave everything else as is

---

## Step 3: Wait for Deployment

1. **Go to the "Actions" tab** in your repository
2. **You should see a workflow running** (it might say "Deploy to GitHub Pages")
3. **Wait for it to finish** (look for a ✅ green checkmark)
4. **This takes about 1-2 minutes**

---

## Step 4: Visit Your Game! 🎮

Once the workflow finishes with ✅:

**Your game is now live at:**
```
https://thegoatzfc-sys.github.io/pixwaves/
```

**Just open that link in your browser and enjoy!**

---

## What If Something Goes Wrong?

### ❌ The workflow shows a red X
1. Click on the failed workflow
2. Scroll down to see the error
3. Common issues:
   - Missing `npm install` - already handled ✅
   - Build errors - check your code

### ❌ The page shows 404 (not found)
1. Go back to Settings → Pages
2. Make sure **Source is set to "GitHub Actions"**
3. Wait 1-2 minutes and refresh

### ❌ The game loads but images/assets are broken
- This is already fixed! ✅ We set the correct path in `vite.config.ts`

---

## Future Updates

Every time you want to update the game:

**Option A: Using GitHub Web Editor**
1. Click the file you want to edit on GitHub
2. Click the pencil icon ✏️
3. Make your changes
4. Click "Commit changes"
5. Workflow runs automatically → site updates in 1-2 minutes

**Option B: Using Command Line (if you're comfortable)**
```bash
git add .
git commit -m "Update game"
git push origin main
```

---

## That's It! 🎉

Your Pixwaves game is now a live website!

**Share the link with friends:**
```
https://thegoatzfc-sys.github.io/pixwaves/
```

---

## Quick Checklist

- ✅ Created `.github/workflows/deploy.yml` file
- ✅ Set Source to "GitHub Actions" in Pages settings
- ✅ Waited for workflow to finish with green ✅
- ✅ Visited the GitHub Pages URL
- ✅ Game is running!

If you get stuck on any step, just let me know! 💬
