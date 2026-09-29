# calibre-respekt

A [Calibre](https://calibre-ebook.com) recipe that downloads the latest weekly issue of the Czech magazine [Respekt](https://www.respekt.cz) as an EPUB.

It needs a respekt.cz account with a subscription that includes the EPUB download.

## How it works

The recipe does what the website does when you click "Stáhnout EPUB":

1. Logs in with your email and password (`POST /api/login`).
2. Finds the latest weekly issue in the archive (`/archiv/<year>`).
3. Downloads that issue's EPUB (`/api/downloadEPub?issueId=...`) and unpacks it. Calibre then converts it like any other recipe output.

## Usage

### Calibre GUI

1. Go to **Fetch news → Add a custom news source → Load recipe from file** and pick `respekt.recipe`.
2. Go to **Fetch news → Schedule news download** and choose **Respekt**.
3. Enter your respekt.cz email as the username, plus your password.
4. Click **Download now**, or set a schedule. Respekt comes out weekly, on Monday.

Calibre stores the credentials in its own config, so they are not kept in this repo.

### Command line

```
ebook-convert respekt.recipe respekt.epub --username you@example.com --password 'yourpassword'
```

## Options

By default the latest weekly issue is downloaded. To get an older one, set the `issue` option to `YEAR/NUMBER`:

```
ebook-convert respekt.recipe respekt.epub \
    --username you@example.com --password 'yourpassword' \
    --recipe-specific-option issue:2026/39
```

Special issues are skipped.

## Troubleshooting

- **"Přihlášení na respekt.cz se nezdařilo"**: wrong email or password.
- **"Vydání ... se nepodařilo stáhnout"**: the login worked, but the site did not return an EPUB. Check that your subscription includes EPUB downloads.
- **Other errors after it worked before**: respekt.cz may have changed its site. The recipe relies on the `/api/login` and `/api/downloadEPub` endpoints and on the issue list embedded in the archive page.
