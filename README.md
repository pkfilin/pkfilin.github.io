# pkfilin.github.io

Öffentlicher Ablageort für Meal & Deal:

| Pfad | Zweck |
|---|---|
| `/.well-known/assetlinks.json` | Bestätigt Android, dass `io.github.pkfilin.mealdeal` Links auf diese Domain öffnen darf |
| `/mealdeal/join/` | Hinweisseite für Einladungslinks, wenn die App (noch) nicht installiert ist |
| `/mealdeal/prospects.json` | Konfiguration der Prospekt-URLs (wird ab Phase 5 von der App geladen) |

## Wichtig

- Das Repository **muss** `pkfilin.github.io` heißen, sonst liefert GitHub Pages
  `/.well-known/assetlinks.json` nicht unter der nötigen URL aus.
- `.nojekyll` ist nötig, damit GitHub Pages den Ordner `.well-known` veröffentlicht.
- In `assetlinks.json` steht der SHA-256-Fingerabdruck des Signaturschlüssels
  (`SHA256: 25:48:C1:53:…:23:FC`). Wird der Schlüssel neu erzeugt, muss der
  Fingerabdruck hier aktualisiert werden – sonst öffnen Einladungslinks die App nicht.
- Einladungsschlüssel stehen im URL-Fragment (`#…`) und werden von Browsern nicht
  an den Server übertragen.

## Veröffentlichen

```bash
git init
git add .
git commit -m "Meal & Deal: Einladungsseite und assetlinks.json"
git branch -M main
git remote add origin git@github.com:pkfilin/pkfilin.github.io.git
git push -u origin main
```

Danach in GitHub unter **Settings → Pages** als Source „Deploy from a branch“,
Branch `main`, Ordner `/ (root)` wählen.

Prüfen:

```bash
curl -s https://pkfilin.github.io/.well-known/assetlinks.json
curl -s https://pkfilin.github.io/mealdeal/prospects.json
```

## App-Links testen

```bash
adb shell pm set-app-links --package io.github.pkfilin.mealdeal 2 all
adb shell pm get-app-links io.github.pkfilin.mealdeal
```

Der Status muss `verified` sein. Bei Änderungen am Schlüssel oder an der
`assetlinks.json` dauert die erneute Prüfung einige Stunden.
