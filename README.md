# Friday's Host

A party game web app. Players join a room, write anonymous responses to prompts, then vote on who wrote what. Live at **[fridayshost.com](https://fridayshost.com)**.

## Stack

- Pure HTML/CSS/JS — no framework, no build step. The whole app is `index.html`.
- **Firebase Realtime Database** via the REST API (no SDK). See `database.rules.json`.
- Deployed on **Netlify** (auto-deploys on push to `main`).

## Files

| File | Purpose |
|------|---------|
| `index.html` | The entire single-page app (large — embeds base64 audio + prompt packs). |
| `howtoplay.html` | Standalone "How to Play" page. |
| `_redirects` / `netlify.toml` | Netlify routing (`/howtoplay` → `/howtoplay.html`). |
| `database.rules.json` | Firebase Realtime Database security rules. |

## Firebase security rules

Rules live in `database.rules.json`. To apply them:

1. Firebase Console → your project → **Realtime Database** → **Rules** tab.
2. Paste the contents of `database.rules.json` and click **Publish**.

**What they enforce:** the root is locked; data is reachable only under `rooms/{code}` (knowing the room code is the access key); writes are blocked once a room's `status` is `ended`; and every field is shape-/size-validated (capped string lengths, numeric bounds, unknown fields rejected on structured nodes) to limit abuse.

**Known limitation:** the app makes unauthenticated REST calls, so these rules cannot enforce per-user ownership (e.g. "only the host may end the game", "only you may edit your own player"). True authorization requires Firebase **Anonymous Auth** + auth-aware rules — a planned follow-up before public launch.

## Deploying

Push to `main` → Netlify builds and deploys automatically. No build command runs; the repo root is published as-is.
