# dribbble-shot

Static shots deployed to Cloudflare Pages at **elx.onl**. No build step — what's in
the repo is what's served.

## Layout

```
/
├── index.html          → elx.onl          (index of shots)
├── _headers            → caching + MIME rules
└── bowld-hero/
    ├── index.html      → elx.onl/bowld-hero
    └── assets/         → self-contained; shots never share assets
```

## Adding a shot

1. `mkdir my-shot` and drop an `index.html` plus its own `assets/` inside.
2. Reference assets **relatively** (`assets/foo.webp`), never `../`.
3. Add a row to the `<ol class="shots">` list in the root `index.html`.
4. Commit and push — Pages redeploys on push to `main`.

The folder name is the URL. Keep it lowercase and hyphenated.

Each shot keeps its own copy of what it needs. It costs a few duplicated files and
buys the guarantee that reworking one shot can never break another.

## Cloudflare Pages setup (once)

| Setting | Value |
| --- | --- |
| Framework preset | None |
| Build command | *(leave empty)* |
| Build output directory | `/` |
| Root directory | `/` |
| Production branch | `main` |

Then **Custom domains → Set up a domain → `elx.onl`**.

## Local preview

The 3D pouches are fetched with `fetch()`, which Chrome blocks on `file://`. Opening
`index.html` by double-clicking gives a blank canvas. Serve it instead:

```bash
npx serve .
# → http://localhost:3000/bowld-hero/
```

## Note on size

The three `.glb` pouches are ~14 MB total, already Draco-compressed and simplified
from ~380 MB of raw Meshy exports. Every future shot with 3D adds to the clone size.
If the repo gets uncomfortable, move `*.glb` to Git LFS:

```bash
git lfs install
git lfs track "*.glb"
git add .gitattributes
```
