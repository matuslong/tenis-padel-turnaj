# Tenis + Padel turnaj

Jednoducha web appka pre jednodnovy turnaj 8 hracov v tenise a padeli.

## Spustenie

Otvor subor `index.html` priamo v prehliadaci. Appka nema build step, server, npm balicky ani externe JS kniznice.

Lokalne sa da otvorit priamo:

```text
file:///C:/dev/tenis-padel-turnaj/index.html
```

Verejny odkaz na pravidla, ktory sa da poslat vopred:

```text
https://matuslong.github.io/tenis-padel-turnaj/?tab=rules
```

## Ukladanie dat

Appka vzdy uklada data do `localStorage` v danom prehliadaci, takze refresh alebo zatvorenie stranky na tom istom zariadeni nestrati progres.

Rovnaky stav turnaja sa uklada aj online cez Supabase a je dostupny kazdemu, kto ma URL stranky a pozna kod turnaja.

Kazdy turnaj ma vlastny kod. Appka podporuje priame odkazy:

```text
https://matuslong.github.io/tenis-padel-turnaj/?turnaj=tenis-padel-2026
```

Pre novy turnaj otvor `Domov` priamo v appke a zadaj novy kod. Kod moze obsahovat male pismena, cisla a pomlcky. Domov zobrazuje lokalny zoznam turnajov, ktore boli v danom prehliadaci vytvorene alebo otvorene.

V `index.html` vypln:

```js
const SUPABASE_URL = "https://PROJECT.supabase.co";
const SUPABASE_ANON_KEY = "ANON_PUBLIC_KEY";
```

Poznamka: appka aktualne pouziva lokalny storage prefix `tenis-padel-turnaj-v2`. Stara testovacia historia z `v1` sa pri nacitani vymaze.

## Funkcie turnaja

- Domov: vytvorenie alebo otvorenie turnaja, zoznam lokalne znamych turnajov a kopirovanie odkazu na pravidla.
- Tenis: skupiny, playoff, live tabulky a bodovanie za umiestnenie `12, 10, 8, 7, 5, 4, 2, 1`.
- Padel Americano: automaticke generovanie aktualneho kola, prelosovanie aktualnej stvorice a vyrovnavanie poctu zapasov; hra sa pevny sucet `24` hier.
- Padel body: surove padel hry urcia padel poradie, do celkovej tabulky ide rovnake umiestnovacie bodovanie ako v tenise.
- Vysledky: zapas sa da zadat mimo poradia, odlozit bez bodov, dopisat neskor alebo uzavriet ako remizu.
- Losovanie: start turnaja, padel kolo a prelosovanie maju kratku shuffle animaciu.

## Supabase setup

V Supabase SQL editore spusti:

```sql
create table if not exists public.tournaments (
  id text primary key,
  state jsonb not null,
  updated_at timestamptz not null default now()
);

alter table public.tournaments enable row level security;

create policy "public tournament read"
on public.tournaments
for select
to anon
using (true);

create policy "public tournament insert"
on public.tournaments
for insert
to anon
with check (true);

create policy "public tournament update"
on public.tournaments
for update
to anon
using (true)
with check (true);
```

Toto je zamerne verejne editovatelne nastavenie: kazdy, kto ma URL stranky a pozna kod turnaja, moze citat a upravovat vysledky.

## Nudzove pravidla

- Chybny vysledok oprav tapnutim na dokonceny zapas.
- Zapas sa da odlozit bez bodov a dopisat neskor.
- Ak sa k odlozenemu zapasu uz nevratite, uzavri ho ako remizu. Tenisova skupina sa ulozi ako `8:8`, padel ako `12:12`.
- Kontumacia je povolena vo vsetkych zapasoch.
- Kontumacia sa ulozi ako `15:0` v tenisovej skupine, `24:0` v padeli alebo `4:0` pri playoff sete.
- Remiza / nedohrane je povolena iba v tenisovych skupinach a v padeli.
- Remiza / nedohrane neprida vyhru ani prehru.
- Playoff musi mat vitaza.
- V appke je dostupne `Spat poslednu zmenu`, export, stiahnutie zalohy a import zalohy.

## GitHub

Projekt je pripraveny ako lokalny git projekt. Private GitHub remote sa da doplnit:

```powershell
git remote add origin <private-github-url>
git push -u origin main
```

Pre GitHub Pages staci publikovat obsah repozitara. `index.html` funguje ako staticka stranka.
