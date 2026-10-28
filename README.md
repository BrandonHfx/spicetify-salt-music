A modified version of [star ratings](https://github.com/brimell/spicetify-star-ratings) for https://my-music.site/music

# Star Ratings for Spotify V25

Rate songs, albums and playlists with stars directly inside the Spotify desktop app, then show your ratings off on a website.

**See it in action:** [my-music.site/music](https://my-music.site/music) is an example site built from a ratings export made with this extension. Click **Start exploring** to look around, or load your own file once you have one.

---

## What you get

- **Stars everywhere.** Rate from track lists, the now-playing bar, album pages and playlist pages.
- **Half and quarter stars** if you want finer ratings.
- **Keyboard shortcuts** to rate the current song without leaving what you're doing.
- **Weighted shuffle.** A **W** button next to shuffle plays higher-rated songs more often.
- **Weighted playlists, sort by rating, and auto-like/skip thresholds.**
- **Duplicate handling.** Remasters and re-releases of the same song are recognised so one song doesn't end up with two conflicting ratings.
- **Export and backup.** Save a `ratings-export.json` file, or sync automatically to a private GitHub Gist.

## How it works

Your ratings live in your own Spotify library. The extension creates a **Rated** folder of playlists, one per rating (for example `4.5`, `3.0`). Rating a song adds it to the matching playlist and removes it from the old one. When a playlist fills up (1,000 songs by default), a spillover playlist such as `4.5(1)` is created.

Nothing is stored on a server unless you turn on cloud sync, and that goes to a Gist you own.

---

## Part 1: Install

You need the Spotify **desktop** app. Spicetify does not work with the web player or mobile apps.

### 1. Install Spicetify

Spicetify is a free tool that adds extensions to Spotify.

**Windows (PowerShell):**

```powershell
iwr -useb https://raw.githubusercontent.com/spicetify/cli/main/install.ps1 | iex
```

**macOS / Linux:**

```bash
curl -fsSL https://raw.githubusercontent.com/spicetify/cli/main/install.sh | sh
```

Then run:

```bash
spicetify backup apply
```

If you get stuck, follow the official [getting-started guide](https://spicetify.app/docs/getting-started).

### 2. Add the extension

1. Download `star-ratings.js` from the [custom plugin directory](https://github.com/BrandonHfx/spicetify-salt-music).
2. Find your Spicetify folder:

   ```bash
   spicetify config-dir
   ```

3. Put `star-ratings.js` in the **Extensions** subfolder.

### 3. Turn it on

```bash
spicetify config extensions star-ratings.js
spicetify apply
```

Restart Spotify if it doesn't reload on its own. You should now see a **Star Ratings** entry in the profile menu (your avatar, top right).

---

## Part 2: Rate your music

| Where | How |
| --- | --- |
| Track lists | Hover a row and click the stars in the **Ratings** column |
| Now playing bar | Click the stars next to the current song |
| Album page | Stars by the action bar show the average of your rated tracks |
| Playlist page | Stars by the action bar rate the playlist as a whole, separate from its songs |
| Keyboard | `Ctrl` + `Alt` + a number key while a song plays |

**Keyboard shortcuts**

| Keys | Rating | Keys | Rating |
| --- | --- | --- | --- |
| `Ctrl` `Alt` `1` | 0.5 | `Ctrl` `Alt` `6` | 3.0 |
| `Ctrl` `Alt` `2` | 1.0 | `Ctrl` `Alt` `7` | 3.5 |
| `Ctrl` `Alt` `3` | 1.5 | `Ctrl` `Alt` `8` | 4.0 |
| `Ctrl` `Alt` `4` | 2.0 | `Ctrl` `Alt` `9` | 4.5 |
| `Ctrl` `Alt` `5` | 2.5 | `Ctrl` `Alt` `0` | 5.0 |

Click the same star again to remove a rating.

### Settings worth knowing

Open them from the profile menu, then **Star Ratings**.

- **Half / quarter star ratings.** Choose your precision.
- **Auto-like threshold.** Automatically like songs rated at or above a value.
- **Skip threshold.** Automatically skip songs rated at or below a value.
- **Default rating for unrated songs.** Used by weighted shuffle.
- **Play.** Limit playback to all songs, only rated, or only unrated.
- **Average ratings.** Keep every rating you give and use a time-weighted average instead of only the latest.
- **Show exact rating.** Display the number next to the stars.
- **Rating to weight conversion.** Linear, or exponential with a base you choose, to control how strongly weighted shuffle favours top-rated songs.
- **Max playlist items.** Smaller playlists make adding ratings faster.

### Weighted shuffle and playlists

- Click the **W** button next to shuffle to turn on weighted shuffle for the current playlist, album or library. Songs are queued at random, weighted by rating.
- Right-click a playlist and choose **Create weighted Playlist** to generate a new playlist of a size you pick.
- Right-click a playlist and choose **Sort by rating** to reorder it (this changes the playlist's Custom order).

### Duplicates

Songs with the same title and artists count as one song, remasters included. A rated song's duplicates show a small notch in their stars.

- Rating a likely duplicate prompts you first: **Rate anyway**, **Cancel**, or **Not a duplicate, rate it**.
- **Review Duplicates** in settings lets you apply one rating to all copies or mark them as separate songs.
- **Fix Duplicate Ratings** lowers mismatched copies to the lowest rating in the group in one step.

---

## Part 3: Export your ratings

Open **Star Ratings** settings and find **Export ratings**:

- **Export To File** saves `ratings-export.json`. This is the file you load on the website.
- **Copy Ratings To Clipboard** copies the same data as text.
- **Sync Ratings Now** pushes to your Gist (see below).

---

## Part 4: View them on the website

1. Go to [my-music.site/music](https://my-music.site/music).
2. On the welcome screen, choose **Start exploring** to browse the example, or the **Upload them** link underneath to open the in-site guide.
3. Click the **load icon** in the header and pick your `ratings-export.json`, or **drag the file anywhere onto the page**.
4. Shift-click the load icon any time to reopen the welcome guide.

---

## Optional: automatic updates with cloud sync

Cloud sync backs your ratings up to a **private GitHub Gist** you own, so you don't have to export by hand. It is off by default and needs no account if you stick with **Export To File**.

1. **Create a GitHub token.** Go to [github.com/settings/tokens](https://github.com/settings/tokens) and choose **Generate new token (classic)**. Name it (for example *Star Ratings*), tick **only** the `gist` scope, pick an expiration, and generate. Copy the token that starts with `ghp_` right away. GitHub only shows it once.
2. **Create a Gist.** Go to [gist.github.com](https://gist.github.com). Name the file `ratings_export.json`, put `{}` as the content, and click **Create secret gist**.
3. **Copy the Gist ID.** It's the long string of letters and numbers at the end of the page URL.
4. **Connect the extension.** In **Star Ratings** settings, paste the token and Gist ID, then turn on **Cloud sync (GitHub Gist)**.

Once enabled, ratings sync about four seconds after each change and again every 15 minutes. **How To Get Tokens** in settings shows these same steps inside Spotify.

> **Keep your token private.** It is stored only in Spotify's local storage on your computer. Never share it or post it online.

---

## Troubleshooting

**No stars appear.** Run `spicetify apply` again and restart Spotify. Confirm `star-ratings.js` is in the Extensions folder and listed by `spicetify config extensions`.

**Stars vanish after a Spotify update.** Updates reset Spicetify. Run `spicetify backup apply` again.

**Ratings seem missing.** The extension reads the **Rated** folder. If you moved or renamed it, right-click the folder and choose **Use as Rated folder**. To register an existing playlist as a rating playlist, right-click it and choose **Use as rating playlist**.

**"Enter your own GitHub token and Gist ID first."** Cloud sync needs both values filled in before it can be switched on.

**Gist sync failed.** Check that the token has the `gist` scope, hasn't expired, and that the Gist ID is correct and contains a `ratings_export.json` file.

**Something looks off.** Open Spotify's developer tools and check the console for messages starting with `[star-ratings]`.

---

## Example

Want to see what an exported collection looks like when browsed? Visit **[my-music.site/music](https://my-music.site/music)**.
