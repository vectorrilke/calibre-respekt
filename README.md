# calibre-respekt

Recept pro [Calibre](https://calibre-ebook.com), který stáhne nejnovější týdenní vydání českého časopisu [Respekt](https://www.respekt.cz) jako EPUB.

Potřebujete účet na respekt.cz s předplatným, které zahrnuje stahování EPUB.

## Jak to funguje

Recept dělá to, co web po kliknutí na „Stáhnout EPUB“:

1. Přihlásí se vaším e-mailem a heslem (`POST /api/login`).
2. Najde nejnovější týdenní vydání v archivu (`/archiv/<rok>`).
3. Stáhne EPUB tohoto vydání (`/api/downloadEPub?issueId=...`) a rozbalí ho. Calibre ho pak převede jako výstup jakéhokoli jiného receptu.

## Použití

### Grafické rozhraní Calibre

1. Zvolte **Stáhnout zprávy → Přidat vlastní zdroj zpráv → Načíst recept ze souboru** a vyberte `respekt.recipe`.
2. Zvolte **Stáhnout zprávy → Naplánovat stahování zpráv** a vyberte **Respekt**.
3. Jako uživatelské jméno zadejte svůj e-mail na respekt.cz a k tomu heslo.
4. Klikněte na **Stáhnout nyní**, nebo nastavte plán. Respekt vychází týdně, v pondělí.

Calibre si přihlašovací údaje ukládá do vlastní konfigurace, takže nejsou v tomto repozitáři.

(Názvy voleb se mohou lišit podle jazyka rozhraní Calibre.)

### Příkazová řádka

```
ebook-convert respekt.recipe respekt.epub --username vas@email.cz --password 'vaseheslo'
```

## Volby

Ve výchozím stavu se stáhne nejnovější týdenní vydání. Starší vydání získáte nastavením volby `issue` ve tvaru `ROK/ČÍSLO`:

```
ebook-convert respekt.recipe respekt.epub \
    --username vas@email.cz --password 'vaseheslo' \
    --recipe-specific-option issue:2026/39
```

Speciální vydání se přeskakují.

## Řešení potíží

- **„Přihlášení na respekt.cz se nezdařilo“**: špatný e-mail nebo heslo.
- **„Vydání ... se nepodařilo stáhnout“**: přihlášení proběhlo, ale web nevrátil EPUB. Zkontrolujte, zda vaše předplatné zahrnuje stahování EPUB.
- **Jiné chyby, i když to dříve fungovalo**: respekt.cz mohl změnit web. Recept se spoléhá na endpointy `/api/login` a `/api/downloadEPub` a na seznam vydání vložený do stránky archivu.

---

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
