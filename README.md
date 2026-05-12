# soundscloud-id-extractor

Small static page that turns a **SoundCloud track URL** into the **numeric track ID** SoundCloud uses in URLs and APIs (for example `https://api.soundcloud.com/tracks/123456789`).

Repo is a single file: **`index.html`** — no build step, no backend.

## How it works

1. **You paste a track link** — full URLs like `https://soundcloud.com/artist/track-name` or short links like `https://on.soundcloud.com/…` work as long as they resolve to a **public** track.

2. **The page calls SoundCloud’s oEmbed API** — in the browser it runs:

   `GET https://soundcloud.com/oembed?format=json&url=<your URL, URL-encoded>`

   That is SoundCloud’s documented way to get embed metadata for a resource. The JSON response includes a `title` and an `html` field: a snippet of HTML for the embed widget.

3. **The track ID is read from the embed HTML** — the widget markup references SoundCloud’s track API path, e.g. `https://api.soundcloud.com/tracks/123456789`. The script uses a regular expression to find that number (it also tolerates URL-encoded slashes, `%2F`, in the string).

4. **Results in the UI** — the numeric ID is shown, the oEmbed `title` is shown as a subtitle when present, and **Copy** writes the ID to the clipboard via the [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API).

There is **no SoundCloud client id or secret** in this project: everything goes through the public oEmbed endpoint and runs only in your browser.

## What works / what does not

| Works | Usually does not |
|--------|------------------|
| Public track URLs | Private or restricted tracks (oEmbed returns nothing useful) |
| Standard and `on.soundcloud.com` short links | Non-track URLs (playlists, profiles, etc. are out of scope) |

If oEmbed fails or the embed HTML does not contain a track id, the UI shows a short error asking for a public track URL.

## Running and hosting

- **Serve over HTTPS** in production. Clipboard access and predictable `fetch` behavior expect a [secure context](https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts). Opening `index.html` as `file://` may be inconsistent across browsers for cross-origin requests.

### Vercel

**Git**

1. Push this repo to GitHub (or GitLab / Bitbucket).
2. In [Vercel](https://vercel.com/new), import the project.
3. Framework: **Other** (or “No framework”). Build command: empty. Output directory: `.` (or the folder that contains `index.html`).

**CLI**

```bash
cd soundscloud-id-extractor
npx vercel
```

### Netlify

**Drag and drop** — upload the folder at [Netlify Drop](https://app.netlify.com/drop).

**Git** — import the repo; build command empty; publish directory `.`.

**CLI**

```bash
cd soundscloud-id-extractor
npx netlify deploy --prod --dir .
```

## Repository layout

| File | Role |
|------|------|
| `index.html` | Markup, styles, and script for the tool |
