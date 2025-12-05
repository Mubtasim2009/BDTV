# BDTV by Moonim

A modern BDIX IPTV **web player** that plays live TV channels directly from M3U playlists in your browser.

- Multi‑source playlists (Mirror, Merge, Falcon).
- HTML5 player (no plugins needed).
- Channel search and grouping.
- Optional **login gate** with “remember me” and logout.
- Mobile‑friendly responsive layout.

---

## Demo Credentials

> These are **client‑side only** and meant for basic gating, not real security.

- **Username:** `Entertainment`  
- **Password:** `Pass`

If “Remember me” is checked during login, you’ll be signed in automatically on your next visit (stored in `localStorage` as a boolean only).

To force logout / clear remember:

- Click **Logout** in the top‑right header, or
- Clear the site’s storage (localStorage) in your browser.

---

## Features

### Player

- HTML5 `<video>` playback.
- Works directly with HTTP(S) playlist entries:
  - HLS (`.m3u8`)
  - DASH (`.mpd`)
  - Direct MP4/TS streams where supported.
- Autodetects format and shows it in the **quality pill** (HLS / DASH / Auto).
- Spacebar shortcut for **play/pause**.

### Playlists

Configured in JS:

```js
const PLAYLISTS = {
  ayna: {
    name: "Mirror BDIX Server",
    url: "https://raw.githubusercontent.com/abusaeeidx/Ayna-BDIX-IPTV-Playlist/refs/heads/main/ayna-playlist.m3u",
  },
  mrgify: {
    name: "Merge BDIX",
    url: "https://raw.githubusercontent.com/abusaeeidx/Mrgify-BDIX-IPTV/main/playlist.m3u",
  },
  falcon: {
    name: "Falcon Cast",
    url: "https://raw.githubusercontent.com/FunctionError/PiratesTv/refs/heads/main/combined_playlist.m3u",
  },
};
```

You can switch sources via the dropdown in the header:

- **Mirror BDIX Server**
- **Merge BDIX**
- **Falcon Cast**

The app:

- Fetches the remote M3U over HTTPS.
- Parses `#EXTINF` lines for:
  - `tvg-logo`
  - `tvg-id`
  - `tvg-name`
  - `group-title`
- Renders a styled channel list with:
  - Logo (from `tvg-logo`) or initials fallback.
  - Group name and “HD/SD” badge.
  - Channel index (`#001`, `#002`, …).

### Login Layer

Front‑end login overlay shown before the main app:

- Simple credential check:
  - **Username:** `Entertainment`
  - **Password:** `Pass`
- “Remember me”:
  - Stores `bdtv_remembered_login_v1 = "true"` in `localStorage`.
  - No password or username is stored.
- Basic lockout:
  - After **5 failed attempts**, login is locked for **30 seconds**.
  - Stored in `localStorage` as failure counter and lock‑until timestamp.
- Logout:
  - Stops playback.
  - Clears remember flag and failed attempts.
  - Resets UI and shows the login screen again.

> **Important:** This login is *not* real security. It’s purely client‑side and can be bypassed by anyone who edits the HTML/JS. For a secure system, you must implement a backend (API, tokens, sessions, etc).

---

## Project Structure

This project is a single‑file static app:

- `index.html`
  - All **HTML, CSS, and JavaScript** are embedded.
  - No build tools, no bundlers required.

---

## Running Locally

1. Save `index.html` (and optionally this `README.md`) in a folder.
2. Open `index.html` in a modern browser (Chrome, Edge, Firefox).

For best results (CORS and HTTPS with remote playlists):

- Serve it via a local web server, for example:

```bash
# Python 3
python -m http.server 8080

# Node (http-server)
npx http-server -p 8080
```

Then open:

```text
http://localhost:8080/index.html
```

---

## Customization

### Change Login Credentials

In `index.html` (script section):

```js
const DEMO_USER = "Entertainment";
const DEMO_PASS = "Pass";
```

Change these to whatever you want. Remember they are visible in the page source.

### Add or Edit Playlists

Extend the `PLAYLISTS` object:

```js
const PLAYLISTS = {
  // existing
  ayna: { ... },
  mrgify: { ... },
  falcon: { ... },

  mycustom: {
    name: "My Custom Source",
    url: "https://example.com/my-playlist.m3u",
  },
};
```

Also add an `<option>` in the playlist `<select>`:

```html
<option value="mycustom">My Custom Source</option>
```

The rest of the app will work automatically.

### Branding

Update these image URLs and texts:

- Favicon and Apple icon in `<head>`.
- Logo inside `.brand-logo` and `.login-logo`.
- Title and subtitle texts:
  - `BDTV by Moonim`
  - `BDIX IPTV Web Player`
  - `Secure Access`

---

## Requirements & Limitations

- Modern browser with:
  - ES6 JavaScript support.
  - Fetch API.
  - Clipboard API (for “Copy Stream URL”).
- Remote playlists must be publicly accessible over HTTPS.
- Some stream formats or DRM‑protected streams may **not** play in browser.

---

## Security Notes

- All authentication is **client‑side**.
- No passwords are transmitted to a server.
- Anyone can bypass the login by:
  - Disabling JS, or
  - Editing the HTML/JS in devtools.

Use this login only as:

- A soft gate / parental control hint.
- A basic UX layer, not as real access control.

For real security:

- Implement server‑side auth (JWT/session cookies).
- Validate playlists / tokens on the server.
- Serve this UI from a backend that enforces permissions.

---

## Credits

- UI and layout: custom design inspired by modern streaming dashboards.
- Playlists:
  - Mirror BDIX / Merge BDIX by their original authors.
  - Falcon Cast (`PiratesTv`) by its original author.
- Created and customized by **Moonim** (`Moonim.live`).
````
