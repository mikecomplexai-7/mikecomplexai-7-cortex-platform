
# Write README.md and deploy.yml
readme_md = '''# CINIS NEXUS | Cortex Intelligence Platform

Static GitHub Pages site for CINIS NEXUS INDUSTRY OGOJA.

## Files
- `index.html`
- `styles.css`
- `.github/workflows/deploy.yml`

## Deploy
1. Push these files to the `main` branch.
2. Go to **Settings → Pages**.
3. Set source to **GitHub Actions**.
4. Commit and push again.

## Notes
- The workflow publishes the entire repository root.
- Keep `index.html` and `styles.css` in the root.
- Update the canonical URL if your repo name changes.

## Future expansion
You can add:
- product pages,
- forms,
- dashboards,
- blog pages,
- API-powered modules,
- analytics and monetization layers.
'''

deploy_yml = '''name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: .
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
'''

with open('/mnt/agents/output/README.md', 'w', encoding='utf-8') as f:
    f.write(readme_md)

with open('/mnt/agents/output/deploy.yml', 'w', encoding='utf-8') as f:
    f.write(deploy_yml)

print("All files written successfully!")
print("\n--- File sizes ---")
import os
for fname in ['index.html', 'styles.css', 'README.md', 'deploy.yml']:
    path = f'/mnt/agents/output/{fname}'
    size = os.path.getsize(path)
    print(f"  {fname}: {size:,} bytes")
