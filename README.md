# SoundCloud track ID (static site)

Single-page static app: `index.html` only. No build step.

## Vercel

**Option A — Git**

1. Put this folder in a Git repo and push to GitHub/GitLab/Bitbucket.
2. In [Vercel](https://vercel.com/new), import the repo.
3. Framework preset: **Other** (or “No framework”). Root directory: this folder if the repo is monorepo.
4. Build command: leave empty. Output directory: leave default (`.`).

**Option B — CLI**

```bash
cd soundcloud-id-tool
npx vercel
```

Follow prompts; accept defaults for a static site.

## Netlify

**Option A — Drag and drop**

1. Zip this folder (or use the folder as-is if the UI allows).
2. In [Netlify Drop](https://app.netlify.com/drop), deploy the folder.

**Option B — Git**

1. Push the repo to GitHub.
2. Netlify → **Add new site** → **Import an existing project**.
3. Build command: empty. Publish directory: `.` (or this folder’s path in the repo).

**Option C — CLI**

```bash
cd soundcloud-id-tool
npx netlify deploy --prod --dir .
```

(`netlify-cli` must be installed or use `npx`.)

## Notes

- Clipboard and `fetch` need **HTTPS**; both hosts provide that on the default URL.
- Keep `Downloads/soundcloud_id_extractor.html` in sync with `index.html` if you still use the Webflow embed (or copy from one to the other when you change styles or logic).
