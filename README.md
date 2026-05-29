# 🎵 Spotify Digger

> Find music the algorithm hides. Spotify pushes popular. This digs deeper.

Spotify Digger analyses your actual listening history and surfaces **obscure tracks** from artists you already like — and their related artists — filtered to only show songs below a popularity threshold you control.

**No server. No backend. No API keys stored anywhere. Runs 100% in your browser.**

---

## What it does

- Reads your **top artists**, **recently played**, or **saved tracks**
- Finds **related artists** you haven't listened to
- Pulls their tracks and **filters out anything too popular** (you choose the threshold)
- Excludes songs you already know
- Lets you **preview tracks** and add them directly to your Spotify playlists

---

## Setup (5 minutes, free)

### Step 1 — Create a Spotify Developer App

1. Go to [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard)
2. Log in with your Spotify account (free account works fine)
3. Click **Create app**
4. Fill in:
   - **App name**: anything you like (e.g. "Spotify Digger")
   - **App description**: anything
   - **Redirect URI**: your GitHub Pages URL — e.g. `https://yourusername.github.io/spotify-digger/`
     - ⚠️ This must **exactly** match your deployed URL, including the trailing slash
   - **Which API/SDKs are you planning to use?**: tick **Web API**
5. Click **Save**
6. On your app's dashboard, click **Settings** → copy your **Client ID**

### Step 2 — Add your Client ID

Open `index.html` and find this line near the top of the `<script>` section:

```js
const CLIENT_ID = 'YOUR_CLIENT_ID_HERE';
```

Replace `YOUR_CLIENT_ID_HERE` with your actual Client ID:

```js
const CLIENT_ID = 'abc123def456...';
```

### Step 3 — Deploy to GitHub Pages

1. Create a new GitHub repository (can be private or public)
2. Push `index.html` to the repo
3. Go to **Settings → Pages**
4. Set source to **Deploy from a branch** → `main` → `/ (root)`
5. Save — your site will be live at `https://yourusername.github.io/repo-name/`

> 💡 Make sure the URL in Step 1 matches exactly

### Step 4 — Use it

1. Visit your GitHub Pages URL
2. Click **Connect with Spotify**
3. Authorise the app (you'll be redirected back automatically)
4. Hit **Dig deeper** and discover music the algorithm hides

---

## Controls

| Control | What it does |
|---|---|
| **Discovery source** | Where to pull your seed artists from |
| **Time range** | How far back to look at your listening (top artists mode only) |
| **Max popularity** | The upper limit on track popularity (0–100). Lower = more obscure |
| **Results** | How many tracks to surface per run |

Popularity scores come directly from Spotify. 0 = totally unknown, 100 = globally massive.

---

## Privacy

- All API calls happen directly from your browser to Spotify
- No data is ever sent to any third-party server
- Your access token is stored only in `sessionStorage` (cleared when you close the tab)
- The app only requests the minimum permissions needed

---

## Troubleshooting

**"Add your Client ID" warning**
→ You haven't replaced `YOUR_CLIENT_ID_HERE` in `index.html`

**Redirect URI mismatch error from Spotify**
→ The URL in your Spotify app's Redirect URIs doesn't exactly match where you're accessing the app. Copy your browser's URL bar exactly (including trailing slash) and paste it into your Spotify app settings.

**No tracks found**
→ Try raising the max popularity slider, or switch to a different discovery source

**"Auth failed" after redirect**
→ Check that your Redirect URI is set correctly in the Spotify Developer Dashboard

---

## How it works (technical)

Uses the **Spotify Web API** with **PKCE OAuth** (no client secret needed, safe for public repos):

1. Fetches your top/recent/saved artists as seed data
2. Calls `/artists/{id}/related-artists` on each seed to build a pool of related artists
3. Fetches top tracks for each candidate artist
4. Filters by: popularity ≤ threshold, not in your known tracks, has a preview URL
5. Sorts by popularity ascending (most obscure first)

---

## License

MIT — do whatever you want with it.