# Category Theory Wiki

An Obsidian vault (`wiki/`) built from *Seven Sketches in Compositionality*, *Category Theory for Scientists* ([arXiv:1302.6946](https://arxiv.org/abs/1302.6946), PDF in `Books/`), *The Dao of Functional Programming* and *Kittenlab.jl*: one note per concept, exercises with solutions, and Julia (Catlab), Lean (Mathlib) and Haskell code for each idea. Start with `wiki/Start Here.md`.

## Running the code tabs

- **Julia**: the snippets target Catlab **v0.16** (`] add Catlab@0.16`). Each Julia tab starts with a **Docs:** line linking the Catlab v0.16 / GATlab / ACSets documentation (or the Kittenlab lecture) for what it uses; a **Builds on:** line names notes whose Julia code must be run first (the Kittenlab-style mini-library of `Category`, `Preorder`, `VCategory`, …). Kittenlab itself: `] add https://github.com/AlgebraicJulia/Kittenlab.jl`. See `wiki/Catlab.md`.
- **Haskell**: plain GHC; most snippets run with `runghc` after adding a `main`.
- **Lean 4**: Mathlib declaration names (`#check`s); they follow current Mathlib naming and may drift as Mathlib evolves.

The vault is also published as a static site with [Quartz](https://quartz.jzhao.xyz) from the `site/` directory:

```bash
cd site
npm ci
npx quartz build -d ../wiki --serve   # preview at http://localhost:8080
```

Pushing to `master` runs `.github/workflows/deploy.yml`, which builds the site and deploys it to GitHub Pages (repository *Settings → Pages → Source: GitHub Actions*).

Site-specific pieces:

- `site/quartz/plugins/transformers/tikz.ts` renders the vault's ```` ```tikz ```` blocks to SVG at build time with `node-tikzjax`; results are cached in `site/.tikz-cache/` (committed, so CI only renders new diagrams).
- `site/quartz/plugins/transformers/tabs.ts` renders the ```` ````tabs ```` blocks of the Obsidian *Markdown Tabs* plugin as tabbed code panels; text outside the code fences in a tab (the **Docs:** lines) is rendered as Markdown.
- `site/quartz/plugins/transformers/ofm.ts` (patched): a wikilink whose alias contains math or code, e.g. `[[Corelation|$\mathbf{Corel}$]]`, is turned into a Markdown link so the alias is rendered by KaTeX instead of breaking the link. Inside tables, write the alias separator as `\|` (`[[Note\|alias]]`), which both Obsidian and Quartz understand.
- Math is rendered with KaTeX (`$…$`, `$$…$$`); diagrams are `tikz` blocks (tikz-cd, rendered at build time) — Mermaid blocks are supported by Quartz as well. Typst is not rendered by Quartz, so the vault uses LaTeX throughout.
- `wiki/index.md` is the site's landing page; it transcludes `Start Here`.
