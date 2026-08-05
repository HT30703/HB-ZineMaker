# The Honey Bunny — desktop & phone install

This folder turns your zine maker into a **downloadable Mac app** (built for you by
GitHub, no build tools needed on your machine) and explains the **simple iPhone install**.

Your whole app is the single file `dist/index.html` (bundled offline — the Mac app works
with no internet). To ship a new version, replace that file and push again.

---

## Part A — Mac app (.dmg) via GitHub

You don't install Rust/Xcode locally; GitHub's Mac servers build it. You only push files.

### 1. Create a GitHub repo
- Go to https://github.com/new → name it e.g. `the-honey-bunny` → **Create repository**.

### 2. Upload this folder
Easiest (no command line):
- On the new repo page click **uploading an existing file**.
- Drag in **everything inside `honey-bunny-app/`** — keep the folder structure
  (`dist/`, `src-tauri/`, `.github/`, `package.json`). GitHub preserves subfolders when
  you drag a folder in on desktop Chrome.
- Commit.

Or with the command line (if you use git):
```
cd honey-bunny-app
git init
git add .
git commit -m "The Honey Bunny desktop app"
git branch -M main
git remote add origin https://github.com/<you>/the-honey-bunny.git
git push -u origin main
```

### 3. Build the app
Two ways to trigger the build:
- **Manual:** repo → **Actions** tab → “Build macOS app” → **Run workflow**.
- **On a version tag:** create a release tagged `v1.0.0` and it builds + attaches the `.dmg`
  automatically (Releases tab).

Wait ~5–10 min for the green check.

### 4. Download & install
- If you tagged a release: repo → **Releases** → download the `.dmg`.
- If you ran it manually: **Actions** → the finished run → **Artifacts** → `the-honey-bunny-macos`.
- Open the `.dmg`, drag **The Honey Bunny** to Applications.

### First launch (unsigned app)
The app isn't code-signed, so macOS will warn the first time. To open it:
- **Right-click the app → Open → Open**, once. After that it launches normally.
- (Optional, for a warning-free app you distribute to others: an Apple Developer
  account, $99/yr — then add the signing certs as GitHub repo *secrets*. Ask me and
  I'll wire the workflow for signing + notarization.)

### Updating later
Replace `dist/index.html` with a newer copy, push, bump the tag (e.g. `v1.0.1`), and a new
`.dmg` builds.

---

## Part B — iPhone (the simple way: Add to Home Screen)

No Xcode, no AltStore, no 7-day expiry, no developer account. Your app is a PWA, so:

1. On your iPhone, open **Safari** and go to **https://diazinemaker.netlify.app**
2. Tap the **Share** button (the square with the up-arrow).
3. Scroll down and tap **Add to Home Screen** → **Add**.
4. It appears as an app icon, opens full-screen, and works offline.

That's the whole install. It behaves like a native app for making zines.

> Why not a “real” sideloaded `.ipa`? Apple blocks free `.ipa` installs unless you re-sign
> every 7 days (AltStore/SideStore) or pay for a developer account. For this app the
> Home-Screen PWA gives you the same day-to-day result with none of that friction. If you
> ever do want the native `.ipa`, tell me and I'll add a Capacitor iOS build to the repo.

---

## What each file is
- `dist/` — your actual app (the single `index.html`, plus manifest/sw/icon). **This is what you update.**
- `src-tauri/` — the thin native Mac wrapper (Rust config; you never edit the code).
- `.github/workflows/build-macos.yml` — the cloud build recipe.
- `package.json` — lets the Tauri CLI run in the build.

## Android (optional)
If you ever want a sideloadable `.apk` (Android allows this freely, no account/expiry),
say the word and I'll add a Capacitor + GitHub Actions Android build alongside the Mac one.
