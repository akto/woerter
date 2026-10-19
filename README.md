# Wörter

A German vocabulary trainer with spaced repetition. The word list lives in a plain CSV file in this repository; the page reads it and schedules reviews in your browser.

## What's in the repo

| File | What it is |
|---|---|
| `index.html` | The whole app (no build step, no dependencies). |
| `Wörter.csv` | The word list. This is the file the app reads first. |
| `Wörter.json` | The same list as JSON. Used if the CSV is missing, or if you choose it in Settings. |
| `README.md` | This file. |

## Publish it with GitHub Pages

1. Sign in to GitHub and click **New repository** (top right **+**).
   Name it, for example, `woerter`, set it to **Public**, and click **Create repository**.
2. On the empty repo page, click **uploading an existing file**.
   Drag in `index.html`, `Wörter.csv`, `Wörter.json` and `README.md`, then click **Commit changes**.
3. Go to **Settings → Pages**.
   Under **Build and deployment**, set **Source** to **Deploy from a branch**, pick **main** and **/ (root)**, and click **Save**.
4. Wait a minute or two, then refresh the Pages settings screen. It shows your address:
   `https://<your-username>.github.io/woerter/`

That address is your app. Open it on your phone and add it to the home screen if you like.

### Optional: your own domain

If you own a domain (for example `woerter.yourname.com`):

1. In **Settings → Pages → Custom domain**, enter the domain and click **Save**.
2. At your domain registrar, add a **CNAME** record: name `woerter`, value `<your-username>.github.io`.
3. Once DNS works (minutes to a few hours), tick **Enforce HTTPS** on the same Pages screen.

### Using git instead of the browser

```bash
git clone https://github.com/<your-username>/woerter.git
cd woerter
# copy the four files in here, then:
git add .
git commit -m "First version of Wörter"
git push
```

## Adding words every day

Either way works; the app picks up the change about a minute after the commit.

**Paste rows by hand:** open `Wörter.csv` on GitHub, click the pencil icon, paste the new rows at the bottom, and click **Commit changes**. Do the same for `Wörter.json` (paste the new objects inside the `"words": [ ... ]` list, with a comma between objects).

**Replace the files:** click **Add file → Upload files**, drop in the new `Wörter.csv` and `Wörter.json` with exactly the same names, and commit. GitHub overwrites the old ones.

Then open the app and press **Settings → Reload from GitHub** (or just refresh the page).

Keep the CSV as the master copy. The app reads it first, so if the JSON falls behind nothing breaks.

## CSV format

UTF-8, comma-separated, first row is the header. Put a field in double quotes if it contains a comma (`"steigt um, stieg um, ist umgestiegen"`).

| Column | Required | Example | Notes |
|---|---|---|---|
| `date` | no | `2026-10-19` | Day the word was added. New words are introduced oldest first. |
| `german` | **yes** | `Bahnhof` | The word without its artikel. |
| `artikel` | nouns | `der` | `der`, `die` or `das`. For two genders write `der/das`. Leave empty for non-nouns. |
| `forms` | no | `Bahnhöfe` | Nouns: the plural (shown as "die Bahnhöfe"). Verbs: 3rd person, Präteritum, Perfekt. |
| `type` | no | `noun` | `noun`, `verb`, `adj`, `adv`, `phrase`, `prep`, `conj`. |
| `english` | yes | `train station` | Separate several meanings with `;`. |
| `turkish` | yes | `tren istasyonu` | |
| `example_de` | yes | `Der Zug kommt pünktlich am Bahnhof an.` | |
| `example_en` | no | `The train arrives at the station on time.` | |
| `example_tr` | no | `Tren istasyona zamanında varıyor.` | |
| `level` | no | `A2` | Used by the level filter. |
| `tags` | no | `bahn,technik` | Used by the tag filter. |
| `repetition` | no | `daily` | Starting box for the word. See below. Defaults to `daily`. |

### Repetition values

`daily`, `bidaily`, `weekly`, `biweekly`, `monthly`, `quarterly`, `retired`

Changing a word's `repetition` value in the CSV later moves it to that box the next time the app loads, and it becomes due that day. `retired` removes it from reviews.

## How reviewing works

- Each word sits in a box: daily, every 2 days, weekly, every 2 weeks, monthly, every 3 months, or retired.
- **Knew it** moves the word one box up. **Hard** keeps it in the same box. **Forgot** sends it back to daily (or one box down, if you choose that in Settings).
- **Move this word to** under the card puts the word straight into any box, including retired.
- The artikel stays hidden behind a **?** until you tap it or guess with **der / die / das**. Your guesses are tracked under **Progress**.
- **Practice** goes through all active words without changing their schedule. **Artikel drill** is nouns only.
- The **Words** tab lists everything with search and a box selector for each word.
- **Settings** lets you change how many days each box waits, how many new words come per day, which language is on the card front (German, English, Turkish or mixed), and the theme.

Keyboard: `Space` show answer, `A` show artikel, `J` `K` `L` guess der/die/das, `1` `2` `3` Forgot/Hard/Knew it, `S` listen.

## Where progress is stored

Review progress is saved in the browser you use (localStorage), not in the repository. Visitors to your public page get their own separate progress. To move yours between phone and computer, use **Settings → Back up progress**, then **Restore from backup** on the other device.

## Trying it on your computer first

Double-clicking `index.html` opens it as a local file, and browsers block it from reading `Wörter.csv` next to it. The page then offers **Choose CSV or JSON file**, which works fine. To test it exactly as GitHub serves it, run this in the folder and open `http://localhost:8000`:

```bash
python -m http.server 8000
```

## If the page says "No word list found"

- The file name must be exactly `Wörter.csv` (capital W, ö). The app also accepts `Woerter.csv` if the umlaut causes trouble.
- The files must be in the repository root, next to `index.html`, not in a folder.
- After a commit, GitHub Pages needs about a minute to publish.
