# FILES.PRJ_FILES

Hosted LUT collections and code-grant manifests for the FILES app.

The app reads from this repo when a user enters a code. Nothing here is
secret — this repo is public because the app downloads without logging in.
Do not put anything here you would not hand to a stranger.

## Layout

```
grants/<code>.json    one file per code — the filename IS the code
luts/<file>.cube      the LUT files, shared across any number of codes
```

## Codes

The code a user types is normalized to lowercase letters and numbers only,
then used as the filename. So `SUMMER25`, `summer25` and `Summer 25` all
resolve to `grants/summer25.json`.

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

- New code: add a file to `grants/`.
- Revoke a code: delete its file. Collections already granted stay on the
  user's device — there is no remote revocation.
- Change a code's contents: edit its file. Only affects future redemptions.

## Test codes

- `TESTPACK` — collection grant, two LUTs, lands directly
- `TESTFILE` — files grant, one LUT, prompts for a destination

Both use generated test LUTs (obvious black-and-white and warm shifts) so a
test either clearly worked or clearly did not.
