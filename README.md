# webgl-builds

Unity WebGL builds served by GitHub Pages and embedded in
[andersongacfilho.github.io](https://andersongacfilho.github.io).

One folder per game, matching the `buildPath` of the matching file in the site's
`src/content/games/`:

```
starlight-protocol/   index.html + Build/ + TemplateData/
solar-system/
enhanced-shots/
```

Each game is then reachable at `https://andersongacfilho.github.io/webgl-builds/<folder>/`
and embedded in the site at `/play/<slug>`.

## Exporting from Unity — read this before building

GitHub Pages cannot set response headers, so it cannot send `Content-Encoding: br` or
`gzip`. A default Unity WebGL build therefore fails to decompress and the page stays black.
In **Project Settings → Player → Publishing Settings**, pick one:

- **Compression Format: Disabled** — simplest, larger files; or
- **Compression Format: Gzip** with **Decompression Fallback: enabled** — smaller files,
  the loader decompresses in JavaScript.

Also worth setting:

- **Color Space: Gamma** if you hit WebGL1 fallback issues on older browsers.
- **Data Caching: on** so returning visitors do not re-download the whole bundle.
- Template: Minimal or Default — the site frames the canvas, so the template chrome is
  mostly invisible.

## Limits to respect

- **100 MB per file** — a hard Git limit. If a `.data` file crosses it, reduce textures or
  audio, or split into Addressables.
- **~1 GB per Pages site** and **100 GB of bandwidth per month**, shared with the other
  repositories on this account.
- The site only loads a build when the visitor clicks play, which is what keeps that
  bandwidth budget realistic.

## Publishing a build

```bash
# from this repository
cp -r /path/to/UnityProject/Build/WebGL/. starlight-protocol/
git add starlight-protocol
git commit -m "Build WebGL do Starlight Protocol"
git push
```

Then flip `live: true` in `src/content/games/<slug>.md` on the site repository, and the
play button replaces the "coming soon" panel.

## Checking a build

Open `https://andersongacfilho.github.io/webgl-builds/<folder>/` directly and watch the
browser console. A decompression error there means the compression setting above was
missed.
