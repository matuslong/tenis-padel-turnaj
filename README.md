# Tenis + Padel turnaj

Jednoducha offline web appka pre jednodnovy turnaj 8 hracov v tenise a padeli.

## Spustenie

Otvor subor `index.html` priamo v prehliadaci. Appka nema build step, server, npm balicky ani externe API.

## Data

Vsetky data sa ukladaju do `localStorage` v danom prehliadaci:

- mena hracov,
- tenisove skupiny a vysledky,
- playoff pavuk,
- padel kola,
- live leaderboard.

Reset turnaja je dostupny priamo v appke cez sekciu `Sprava`.

## GitHub

Projekt je pripraveny ako lokalny git projekt. Private GitHub remote sa da doplnit neskor:

```powershell
git remote add origin <private-github-url>
git push -u origin main
```
