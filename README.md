# gh-deployment-workflow

Link to deployed site on GitHub Pages: https://irwanpanai.github.io/gh-deployment-workflow/

Project URL: https://roadmap.sh/projects/github-actions-deployment-workflow

## Here's a complete step-by-step guide to deploy a static website to GitHub Pages using GitHub Actions:

1. **Create a New Repository**
   - Go to GitHub.com
   - Click "New repository"
   - Name it `gh-deployment-workflow` (or any name you prefer)
   - Make it Public
   - Check "Add a README file"
   - Click "Create repository"

2. **Create Required Files**
   First, create `index.html`:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>GitHub Actions Deployment</title>
   </head>
   <body>
       <h1>Hello, GitHub Actions!</h1>
       <p>This page is deployed using GitHub Actions</p>
   </body>
   </html>
   ```

3. **Create Workflow File**
   - Create a new folder structure: `.github/workflows/`
   - Inside the workflows folder, create `deploy.yml`:



```yaml
name: Deploy Static Website

on:
  push:
    branches: [ main ]
    paths:
      - 'index.html'
      - '**.js'
      - '**.css'
  workflow_dispatch:

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
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v4
        
      - name: Build with Jekyll
        uses: actions/jekyll-build-pages@v1
        with:
          source: ./
          destination: ./_site

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./_site

  deploy:
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

4. **Configure GitHub Pages**
   - Go to your repository settings
   - Click on "Pages" in the left sidebar
   - Under "Source", select "GitHub Actions"
   - Save the changes

5. **Push Your Changes**
   ```bash
   git add .
   git commit -m "Initial commit with GitHub Actions workflow"
   git push
   ```

6. **Verify Deployment**
   - Go to the "Actions" tab in your repository
   - You should see your workflow running
   - Wait for it to complete (should take 1-2 minutes)
   - Once done, click on the deployment URL in the workflow run

7. **Access Your Website**
   - Your website will be available at:
   `https://<username>.github.io/<repository-name>/`
   - For example: `https://john-doe.github.io/gh-deployment-workflow/`

**Troubleshooting Tips:**
- If the workflow doesn't start automatically, try pushing a change to `index.html`
- Make sure all permissions are correctly set in repository settings
- Check the Actions tab for any error messages
- Verify that GitHub Pages is enabled in repository settings

**Additional Notes:**
- The website will update automatically when you push changes to `index.html`, JS, or CSS files
- You can also manually trigger the deployment from the Actions tab
- The workflow includes concurrency control to prevent multiple deployments running simultaneously

Would you like me to explain any specific part in more detail?
