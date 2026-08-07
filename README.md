# FILES.PRJ_FILES

Hosted LUT collections and code-grant manifests for the FILES app.

The app reads from this repo when a user enters a code. Nothing here is
secret — this repo is public because the app downloads without logging in.
Do not put anything here you would not hand to a stranger.

## Layout

```
CODES/<code>.json     one file per code — the filename IS the code
FILES/<file>.cube     the LUT files, shared across any number of codes
```

## Codes

The code a user types is normalized to lowercase letters and numbers only,
then used as the filename. So `SUMMER25`, `summer25` and `Summer 25` all
resolve to `CODES/summer25.json`.

There is deliberately no index listing every code. A code that has no file
returns 404, which the app shows as "Invalid code". This keeps someone from
browsing a list of every code ever issued.

## Two kinds of grant

A manifest has EITHER `collections` OR `files`, never both.

**`collections`** — lands directly in the user's picker, no prompt:

```json
{
  "version": 1,
  "collections": [
    { "name": "Summer 2025", "luts": [
        { "name": "Kodak 250D", "file": "kodak250d.cube", "sha256": "..." }
    ]}
  ]
}
```

**`files`** — the user picks which collection it goes into:

```json
{
  "version": 1,
  "files": [
    { "name": "Cine Warm", "file": "cinewarm.cube", "sha256": "..." }
  ]
}
```

`sha256` is optional. When present the app verifies it after download and
rejects the whole grant on a mismatch.

## Editing

- New code: add a file to `CODES/`.
- Revoke a code: delete its file. Collections already granted stay on the
  user's device — there is no remote revocation.
- Change a code's contents: edit its file. Only affects future redemptions.

## Directory names are load-bearing

`CODES/` and `FILES/` are compiled into the app (CodeGrantService.swift).
Renaming either one breaks every published code until a new build ships.
