# Category Theory Wiki

An Obsidian vault (`wiki/`) built from *Seven Sketches in Compositionality*, *The Dao of Functional Programming* and *Kittenlab.jl*: one note per concept, exercises with solutions, and Julia (Catlab), Lean (Mathlib) and Haskell code for each idea. Start with `wiki/Start Here.md`.

The vault is also published as a static site with [Quartz](https://quartz.jzhao.xyz) from the `site/` directory:

```bash
cd site
npm ci
npx quartz build -d ../wiki --serve   # preview at http://localhost:8080
```

Pushing to `master` runs `.github/workflows/deploy.yml`, which builds the site and deploys it to GitHub Pages (repository *Settings → Pages → Source: GitHub Actions*).

Site-specific pieces:

- `site/quartz/plugins/transformers/tikz.ts` renders the vault's ```` ```tikz ```` blocks to SVG at build time with `node-tikzjax`; results are cached in `site/.tikz-cache/` (committed, so CI only renders new diagrams).
- `site/quartz/plugins/transformers/tabs.ts` renders the ```` ````tabs ```` blocks of the Obsidian *Markdown Tabs* plugin as tabbed code panels.
- `wiki/index.md` is the site's landing page; it transcludes `Start Here`.
