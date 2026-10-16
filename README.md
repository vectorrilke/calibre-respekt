# calibre-respekt

Recept pro [Calibre](https://calibre-ebook.com), který stáhne nejnovější týdenní vydání českého časopisu [Respekt](https://www.respekt.cz) jako EPUB.

Potřebujete účet na respekt.cz s předplatným, které zahrnuje stahování EPUB.

## Jak to funguje

Recept dělá to, co web po kliknutí na „Stáhnout EPUB“:

1. Přihlásí se vaším e-mailem a heslem (`POST /api/login`).
2. Najde nejnovější týdenní vydání v archivu (`/archiv/<rok>`).
3. Stáhne EPUB tohoto vydání (`/api/downloadEPub?issueId=...`) a rozbalí ho. Calibre ho pak převede jako výstup jakéhokoli jiného receptu.
4. Nastaví knize název, autora, štítky, datum vydání a obálku podle údajů o vydání z archivu.

## Metadata knihy

- **Název:** `Respekt ROK/ČÍSLO: titulek vydání`, např. `Respekt 2026/42: Jak Babiš pomohl na svět lídrovi`
- **Autor:** `Respekt`
- **Datum vydání:** první den týdne, pro který vydání platí. U čísla „42/2026 • 12.–18. 10. 2026“ je to 12. 10. 2026.
- **Obálka:** obálka vydání z respekt.cz, stejná jako vedle tlačítka „Stáhnout EPUB“ (šířka 720 px)
- **Štítky:** `News, Respekt, magazine`

Při stahování v grafickém rozhraní (Stáhnout zprávy) Calibre autora a štítky ze souboru nepoužije:

- Autora vždy nastaví na `calibre`, podle toho pozná zprávy, které má automaticky mazat.
- Štítky sestaví z `News`, názvu receptu (pokud je zapnuto přidání názvu jako štítku) a dalších štítků z pokročilého nastavení plánu. Štítky `Respekt` a `magazine` tedy zadejte v **Naplánovat stahování zpráv → Respekt - aktuální vydání**, na kartě pokročilého nastavení do pole dalších štítků.

## Použití

### Grafické rozhraní Calibre

1. Zvolte **Stáhnout zprávy → Přidat vlastní zdroj zpráv → Načíst recept ze souboru** a vyberte `respekt.recipe`.
2. Zvolte **Stáhnout zprávy → Naplánovat stahování zpráv** a vyberte **Respekt - aktuální vydání**.
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

## Změny

- **2026-10-16:** Opravena obálka. Kniha dříve dostávala obálku z EPUB místo obálky vydání, protože Calibre dal přednost obálce uvedené v samotném EPUB. Nově se obálka vydání předává Calibre přímo při převodu. Zároveň přibyl název s ročníkem a číslem vydání, autor `Respekt`, datum vydání podle týdne vydání a štítky; recept se nyní jmenuje „Respekt - aktuální vydání“.

---

# calibre-respekt

A [Calibre](https://calibre-ebook.com) recipe that downloads the latest weekly issue of the Czech magazine [Respekt](https://www.respekt.cz) as an EPUB.

It needs a respekt.cz account with a subscription that includes the EPUB download.

## How it works

The recipe does what the website does when you click "Stáhnout EPUB":

1. Logs in with your email and password (`POST /api/login`).
2. Finds the latest weekly issue in the archive (`/archiv/<year>`).
3. Downloads that issue's EPUB (`/api/downloadEPub?issueId=...`) and unpacks it. Calibre then converts it like any other recipe output.
4. Sets the book's title, author, tags, publication date and cover from the issue's data in the archive.

## Book metadata

- **Title:** `Respekt YEAR/NUMBER: issue headline`, e.g. `Respekt 2026/42: Jak Babiš pomohl na svět lídrovi`
- **Author:** `Respekt`
- **Publication date:** the first day of the issue's week. For "42/2026 • 12.–18. 10. 2026" that is 12 October 2026.
- **Cover:** the issue's cover from respekt.cz, the same one shown next to the "Stáhnout EPUB" button (720 px wide)
- **Tags:** `News, Respekt, magazine`

When you download through the GUI (Fetch news), calibre ignores the author and tags in the file:

- It always sets the author to `calibre`, which is how it recognizes news to delete automatically.
- It builds the tags from `News`, the recipe title (if "Add title as tag" is on) and the extra tags in the schedule's Advanced tab. So add `Respekt` and `magazine` under **Schedule news download → Respekt - aktuální vydání → Advanced → Extra tags**.

## Usage

### Calibre GUI

1. Go to **Fetch news → Add a custom news source → Load recipe from file** and pick `respekt.recipe`.
2. Go to **Fetch news → Schedule news download** and choose **Respekt - aktuální vydání**.
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

## Changes

- **2026-10-16:** Fixed the cover. The book used to get the cover from inside the EPUB instead of the issue's cover, because calibre preferred the cover listed in the EPUB itself. The issue's cover is now passed to calibre directly during conversion. Also added a title with the year and issue number, author `Respekt`, a publication date based on the issue's week, and tags; the recipe is now called "Respekt - aktuální vydání".
