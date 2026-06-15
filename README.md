# Tenis + Padel turnaj

Jednoducha web appka pre jednodnovy turnaj 8 hracov v tenise a padeli.

## Spustenie

Otvor subor `index.html` priamo v prehliadaci. Appka nema build step, server, npm balicky ani externe JS kniznice.

## Ukladanie dat

Appka vzdy uklada data do `localStorage` v danom prehliadaci, takze refresh alebo zatvorenie stranky na tom istom zariadeni nestrati progres.

Ak doplnis Supabase konfiguraciu v `index.html`, rovnaky stav turnaja sa bude ukladat aj online a bude dostupny kazdemu s GitHub URL.

V `index.html` vypln:

```js
const SUPABASE_URL = "https://PROJECT.supabase.co";
const SUPABASE_ANON_KEY = "ANON_PUBLIC_KEY";
const TOURNAMENT_ID = "tenis-padel-2026";
```

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

Toto je zamerne verejne editovatelne nastavenie: kazdy, kto ma URL stranky, moze citat a upravovat vysledky.

## GitHub

Projekt je pripraveny ako lokalny git projekt. Private GitHub remote sa da doplnit:

```powershell
git remote add origin <private-github-url>
git push -u origin main
```

Pre GitHub Pages staci publikovat obsah repozitara. `index.html` funguje ako staticka stranka.
