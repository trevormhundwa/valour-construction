# Valour Construction - Deployment Guide

## Project Structure

- **main branch**: Source code (Astro project)
- **deploy branch**: Built static files only

## Development Workflow

### 1. Make Changes (on main branch)
```bash
# Edit source files in src/
npm run dev  # Preview locally at localhost:4321
```

### 2. Build the Project
```bash
npm run build  # Creates dist/ folder
```

### 3. Update Deploy Branch
```bash
# Switch to deploy branch
git checkout deploy

# Remove old files (keep .git and .cpanel.yml)
rm -rf about assets contact images projects services *.html *.xml *.txt *.svg

# Copy new build files
cp -r ../newsite-2/dist/* .
# OR if you're in the same repo:
git checkout main -- dist/
cp -r dist/* .
rm -rf dist

# Commit and push
git add .
git commit -m "Deploy update"
git push origin deploy

# Switch back to main
git checkout main
```

### 4. Deploy to Server (via SSH/Terminal)

Connect to cPanel Terminal and run:

```bash
cd /home/valourcon/repositories/new_site1
git pull origin deploy
```

Then copy files to public_html:

```bash
/bin/cp index.html /home/valourcon/public_html/
/bin/cp 404.html /home/valourcon/public_html/
/bin/cp favicon.svg /home/valourcon/public_html/
/bin/cp robots.txt /home/valourcon/public_html/
/bin/cp sitemap-0.xml /home/valourcon/public_html/
/bin/cp sitemap-index.xml /home/valourcon/public_html/
/bin/cp -R about /home/valourcon/public_html/
/bin/cp -R assets /home/valourcon/public_html/
/bin/cp -R contact /home/valourcon/public_html/
/bin/cp -R images /home/valourcon/public_html/
/bin/cp -R projects /home/valourcon/public_html/
/bin/cp -R services /home/valourcon/public_html/
```

## Quick Deploy Script

You can create a script on the server at `/home/valourcon/deploy.sh`:

```bash
#!/bin/bash
cd /home/valourcon/repositories/new_site1
git pull origin deploy
cp index.html /home/valourcon/public_html/
cp 404.html /home/valourcon/public_html/
cp favicon.svg /home/valourcon/public_html/
cp robots.txt /home/valourcon/public_html/
cp sitemap-0.xml /home/valourcon/public_html/
cp sitemap-index.xml /home/valourcon/public_html/
cp -R about /home/valourcon/public_html/
cp -R assets /home/valourcon/public_html/
cp -R contact /home/valourcon/public_html/
cp -R images /home/valourcon/public_html/
cp -R projects /home/valourcon/public_html/
cp -R services /home/valourcon/public_html/
echo "Deployment complete!"
```

Make it executable: `chmod +x /home/valourcon/deploy.sh`

Then deploy with: `~/deploy.sh`

## Repository URLs

- GitHub: https://github.com/trevormhundwa/valour-construction
- Branches: `main` (source), `deploy` (built files)

## Notes

- cPanel Git deployment UI has issues; use manual deployment instead
- Always build locally before updating deploy branch
- 74 project images (v1.jpg - v74.jpg) in /images/projects/
