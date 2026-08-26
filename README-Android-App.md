# Running this ERP as an Android app

## What I can't do here
I can't compile and sign a real `.apk` file inside this chat — that needs the Android
SDK and Google's build servers, which this sandbox has no access to. But there are two
free, no-coding-required ways to get this ERP installed on Android like a real app, and
I've already done the prep work for both (the files in this folder).

## Step 1 — Host these 6 files somewhere with HTTPS (required either way)
Android's "install as app" feature and APK generation both require the app to be served
over HTTPS — opening `index.html` directly from a phone's file storage won't work for
this part (the ERP itself still runs fine either way, you just won't get the install
prompt or be able to generate an APK from it).

Keep all 6 files together in one folder — `index.html`, `manifest.json`, `sw.js`, and the
3 icon PNGs — and upload that folder as-is to any of these (all free):

- **GitHub Pages** — create a new repo, upload these files, then turn on Pages in
  Settings → Pages → Deploy from branch. You'll get a URL like
  `https://yourname.github.io/costing-erp/`.
- **Netlify Drop** — go to `app.netlify.com/drop` and drag the folder in. No account
  needed for a quick one-off deploy; gives you an HTTPS URL immediately.
- **Vercel** — similar drag-and-drop / repo-import flow.

Whichever you pick, the important part is that `index.html`, `manifest.json`, `sw.js`
and the icons all end up **in the same folder** at that URL.

## Step 2 — Install it like an app (no APK needed for this part)
Open that HTTPS URL in Chrome on your Android phone. Chrome will either show an
"Install app" banner automatically, or you can tap the ⋮ menu → **Add to Home screen**.
This gives you a real app icon, a full-screen window with no browser bar, and the app
still opens with no signal (the service worker caches the shell). All your data still
lives in that browser's storage, exactly as before.

This alone covers "run it like an app on my phone" for most people — no `.apk` file
involved at all.

## Step 3 (optional) — Generate a real, side-loadable .apk
If you specifically want an installable `.apk` file (e.g. to share it directly with
someone, or publish to the Play Store), use **PWABuilder** — a free Microsoft tool, no
coding involved:

1. Go to `https://www.pwabuilder.com`
2. Paste in the HTTPS URL from Step 1 and click **Start**
3. It will check your manifest/service worker (both already set up correctly here) and
   let you download an **Android package** — a real, signed `.apk`/`.aab` you can
   install directly on a phone (enable "Install unknown apps" for your browser once,
   Android will ask) or upload to the Play Store.

This whole process takes about 5 minutes and requires no Android Studio, no SDK, and no
coding on your side.

## Storing the JSON "database" on Google Drive
Two options are already built into the app, ranging from zero-setup to fully automatic:

**Already works, no setup needed:**
- **Export → Share Backup (Drive/etc.)** button (next to Export Backup) opens your
  phone's native share sheet — tap **Google Drive** (or Dropbox, email, etc.) and the
  backup file saves straight there. No Google account setup or API keys needed.
- **Import Backup** already lets you pick a file from Google Drive too — Android's file
  picker includes Drive as a source automatically.

**Bigger option, if you want it — true auto-sync:**
A real Google Drive API integration (OAuth) so the app reads/writes the JSON file to
your Drive automatically, with no manual export/import step at all. This needs you to
create a free Google Cloud OAuth Client ID first (I can walk you through the exact
screens), and then I'd wire in the real Drive API calls. It's a bigger, separate piece
of work — let me know if you want to go ahead with it and I'll build it next.
