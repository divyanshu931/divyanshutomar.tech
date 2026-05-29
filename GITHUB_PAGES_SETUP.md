# GitHub Pages Deployment Guide

## Setup Instructions

### 1. Create a GitHub Repository
- Go to [GitHub](https://github.com/new) and create a new repository
- Repository name can be anything (e.g., `portfolio-website`)
- Do **NOT** initialize with README, .gitignore, or license

### 2. Upload Your Code to GitHub
```bash
git init
git add .
git commit -m "Initial commit"
git branch -main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

Replace `YOUR_USERNAME` and `YOUR_REPO` with your actual GitHub username and repository name.

### 3. Configure Repository Settings for GitHub Pages

1. Go to your repository on GitHub
2. Click **Settings** → **Pages** (in left sidebar)
3. Under "Source", select **Deploy from a branch**
4. Branch: Select `gh-pages`
5. Folder: Select `/ (root)`
6. Click **Save**

### 4. Automatic Deployment

The GitHub Actions workflow (`.github/workflows/deploy.yml`) will automatically:
- Build your project when you push to `main` or `master` branch
- Deploy the built files to the `gh-pages` branch
- GitHub Pages will host the content from the `gh-pages` branch

### 5. Access Your Site

Your portfolio will be available at:

**For User/Organization Pages** (if repo is named `USERNAME.github.io`):
- `https://USERNAME.github.io`

**For Project Pages** (any other repo name):
- `https://USERNAME.github.io/REPO_NAME`

### Important: Base Path Configuration (for Project Pages)

If you're using a **project repository** (not `USERNAME.github.io`), you need to set the base path in webpack.

Edit `bundler/webpack.common.js` and update the output:

```javascript
output: {
    hashFunction: 'xxhash64',
    filename: 'bundle.[contenthash].js',
    path: path.resolve(__dirname, '../public'),
    publicPath: '/YOUR_REPO_NAME/',  // Add this line
},
```

Replace `YOUR_REPO_NAME` with your repository name.

### Testing Locally

Before pushing to GitHub, test locally:

```bash
npm install
npm run build
npm run dev
```

Then visit `http://localhost:8080`

## Troubleshooting

### Site shows 404
- Wait 2-3 minutes after pushing - GitHub needs time to build
- Verify the `gh-pages` branch was created in Settings
- Check the Actions tab to see if the workflow completed successfully

### Assets not loading
- You likely need the `publicPath` setting in webpack.config
- Make sure it matches your repository name for project pages

### Build failed
- Check the Actions tab for error messages
- Verify all dependencies are correct in `package.json`
- Try running `npm install && npm run build` locally first

## Next Steps

- Add your own domain (optional): Settings → Pages → Custom domain
- Enable HTTPS (automatic for github.io domains)
- Update your portfolio content as needed!
