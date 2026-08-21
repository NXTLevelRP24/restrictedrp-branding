# Restricted RP — branding assets

Public image host for the FiveM server browser banners.

## Why this repo is PUBLIC and must stay that way

`sets banner_detail` / `sets banner_connecting` in `configs/server.cfg` are **not**
local-file commands. They are plain strings the **player's own game client** fetches
over HTTP. A private repo's `raw.githubusercontent.com` URL requires an auth token,
so the client gets a 404 and shows no banner.

## Why not imgur (2026-08-22)

Imgur **blocks the FiveM client by user-agent**. Proven from the server, same URL:

| Request as              | Result             |
|-------------------------|--------------------|
| Browser                 | 200 image/png      |
| No user-agent           | 200 image/png      |
| curl default            | 200 image/png      |
| `CitizenFX/1.0`         | **429 Too Many Requests** |

It fails silently — no error in any console, the banner is just blank. Verifying an
image URL in a browser proves nothing; it must be tested with the FiveM user-agent:

    curl -A "CitizenFX/1.0" -o /dev/null -w "%{http_code} %{content_type}\n" <url>

`raw.githubusercontent.com` returns 200 image/png to that user-agent, which is why
the banners live here.

Also avoid Discord CDN links — they expire.

## Files

| File                    | Used by                  | Size      |
|-------------------------|--------------------------|-----------|
| `banner_detail.png`     | `sets banner_detail`     | 1895x250  |
| `banner_connecting.png` | `sets banner_connecting` | 1200x480  |

Replacing a banner = commit a new file with the **same name** (the URL in
`server.cfg` stays valid). Keep the filenames stable.
