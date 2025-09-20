.github/workflows/pages-deploy.yml
echo "# Compliance Monitoring Platform" > README.md
git add .
git commit -m "Setup monorepo structure with frontend, backend, infra folders"
git push origin main


name: Build and deploy frontend to GitHub Pages
on:
  push:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '18'
      - name: Install deps
        run: |
          cd frontend
          npm ci
      - name: Build
        run: |
          cd frontend
          npm run build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: frontend/dist   # change to frontend/build if CRA
  deploy:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        uses: actions/deploy-pages@v1
