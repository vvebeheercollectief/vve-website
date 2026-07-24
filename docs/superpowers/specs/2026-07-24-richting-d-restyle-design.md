# Ontwerp-spec: "Richting D"-restyle van de VvE Beheer Collectief website

**Datum:** 2026-07-24
**Repo:** `vve-website` (single-file React-app op GitHub Pages)
**Bron van waarheid:** `docs/richting-d-handoff/README.md` + `docs/richting-d-handoff/mockup.dc.html` (definitieve NL-teksten, prijzen, FAQ, pakket- en vergelijkingsinhoud) en het merkboek `Design.pdf` (Richting D: kleur + typografie).

---

## 1. Doel & scope

Het volledige **uiterlijk** van de bestaande, live website vervangen door de vastgelegde huisstijl **"Richting D"** (leisteen/papierwit + staalblauw accent, serif-koppen), en de **werkende functionaliteit behouden**. Geen nieuwe site vanaf nul; geen nieuwe hosting/workflow.

**In scope**
- Alle zes weergaven (Home, Diensten, Pakketten, Werkwijze, Over ons, Offerte) + globale header/footer opnieuw opgebouwd in Richting D.
- Nieuwe Nederlandse teksten (uit de mockup) als het nieuwe `NL`-content-object.
- Offerteformulier gaat aanvragen daadwerkelijk versturen via **`mailto:`** naar `info@vvebeheercollectief.nl`.
- Toegankelijkheid (focus-states, `prefers-reduced-motion`, toetsenbord, aria), responsive ≤900px.

**Buiten scope (bewust, apart traject)**
- De 17 taalbestanden bijwerken → **vervolgronde** (zie §7). Nu geldt nette NL-terugval.
- Echte fotografie ("Over ons") → verzorgde placeholder nu.
- Echte klantreviews → testimonial-sectie blijft uit tot ze er zijn.
- Nieuwsbrief koppelen aan een echte mailinglijst → blijft client-side bevestiging (ongewijzigd t.o.v. nu).

## 2. Vastgelegde besluiten

| Onderwerp | Besluit |
|---|---|
| Vertalingen | Nederlands nu live; taalknop blijft; nette **NL-terugval**; 17 talen in vervolgronde. |
| "Over ons"-foto | Verzorgde **placeholder** (subtiel verloop) nu; echte foto later. |
| Bouwaanpak | **Herbouw binnen het bestaande `index.html`** (één-bestands React-app). |
| Offerteformulier | **Versturen via `mailto:`** (kant-en-klare e-mail naar `info@`). |
| Uitrol | Bouwen op branch `richting-d-restyle`, preview, pas na akkoord merge naar `main`. |

## 3. Architectuur (bestaand — blijft)

- Eén `index.html`, geserveerd door **GitHub Pages** (auto-deploy bij push naar `main`).
- In-browser **React 18.3.1 (UMD) + ReactDOM + Babel-standalone 7.29.0** via unpkg; `<script type="text/babel">`; `ReactDOM.createRoot(document.getElementById('root')).render(<App />)`. **Geen bouwstap.**
- Client-side "routing" via React-state (`page`), niet via echte URL's.
- Herbruikbare helpers blijven: `useLanguage()`/`t()` (taalsysteem), `I` (Phosphor-icoon-wrapper), `Photo` (achtergrond-image met `role="img"`).

**Aanpak:** de presentatielaag (de `App`-componentboom, de `<style>`-inhoud en het `NL`-content-object) wordt vervangen door Richting D. De "machinerie" (taalsysteem, portaal-link, juridische pagina's, `mailto:`-links, favicon, `LANGUAGES`-lijst met RTL-vlag voor `ar`) blijft ongewijzigd of wordt hergebruikt.

## 4. Design tokens (Richting D)

### Kleur
| Rol | Waarde |
|---|---|
| Leisteen (hoofd, donkere vlakken/koppen) | `#222B36` · `#1C242E` |
| Tekst hoofd (bijna-zwart navy) | `#1C242E` |
| Tekst body/secundair | `#59616B` · `#68717C` |
| Tekst tertiair/muted | `#7A828C` · `#96A0AB` · `#9BA3AC` |
| Tekst op donker (licht) | `#F5F8FB` · `#C3CBD4` · `#AEB8C3` · `#8891A0`; chips `#3B4653` |
| Accent staalblauw (primair) | `#4E6885` |
| Accent hover donkerder | `#3F566F`; op donker: `#5A7796` |
| Accent licht/mist | `#8AA1B8` · `#9CACBE` |
| Donkere gradient (CTA/footer) | `#232E3A → #1A212A` (180°); blokken `#26313E → #181F28` (150°) |
| Papier/off-white | `#FBFCFD`; sectie-licht `#F5F7F9` |
| Wit | `#FFFFFF` |
| Randen/hairlines | `#EEF1F4` · `#ECEFF2` · `#E1E6EB` · `#CDD5DD` · `#E4E9ED` |
| Chip/markering-vlak | `#EEF3F7` · `#EAF0F4` · `#F6F9FB` |
| Link | `#4E6885`, hover `#3B5069` |
| Focus-ring | `rgba(78,104,133,0.15)` (3px) + border `#4E6885` |

Verhouding in gebruik (merkboek): leisteen ~55% · papier ~28% · staal ~11% · neutraal ~6%. Staalblauw is **spaarzaam** accent.

### Typografie (Google Fonts)
- **Source Serif 4** (400/500/600) — koppen, titels, prijzen, statements. H1 `letter-spacing:-0.015em`, H2 `-0.01em`.
- **Karla** (400/500/600/700) — lopende tekst, labels, knop-alternatief. Regelafstand ~1.6–1.75.
- **Jost** (300/400/500) — kleine kapitalen/labels, nav, knoppen, eyebrows. UPPERCASE, `letter-spacing` 0.06em (nav) → 0.28em (eyebrows).
- **Schaal:** H1 hero 58 / pagina 40–46 · H2 38–42 · H3 20–23 · body 15–18 · label/eyebrow 11–12 · knop 11.5–12.5 (px).

> Let op: dit vervangt de huidige **DM Sans**-lettertypeset. De Google-Fonts-`<link>` in `<head>` wordt bijgewerkt naar Source Serif 4 + Karla + Jost.

### Spacing, radius, schaduw, animatie
- **Content-breedtes:** 1160 (breed) · 1080 (offerte) · 1000 (diensten/pakketten/werkwijze/over ons) · 820 (tekst/FAQ) · 760 (tijdlijn) · 640 (gecentreerde koppen).
- **Sectie-padding:** 104 (ruime home-secties) · 88–96 (paginaheaders) · 64 (mobiel); horizontaal overal 24. Grid-gap 24 / 32–34 / 52–64. **Breakpoint 900px.**
- **Radius:** knoppen 4–5 · chips/pills 40 · kaarten 10–14 · kleine blokjes 3–8.
- **Schaduw:** kaart `0 18px 50px -34px rgba(28,36,46,.4)`; hover `0 34px 70px -34px rgba(28,36,46,.5)`; prijskaart `0 24px 60px -46px rgba(28,36,46,.4)`; uitgelicht `0 46px 92px -42px rgba(28,36,46,.6)` + ring `0 0 0 2px #4E6885`.
- **Animatie:** `omFadeUp` (opacity 0→1, translateY 20→0), `omFade`, optioneel `omFloat`. Timing `.6–.7s cubic-bezier(.2,.6,.2,1)`; hovers `.2–.25s`. Bij `prefers-reduced-motion: reduce` alles uit / direct zichtbaar.

## 5. Globale componenten

**Header (sticky).** `z-index:60`, `background:rgba(255,255,255,0.9)` + `backdrop-filter:blur(12px)`, onderrand `1px solid #EEF1F4`, `max-width:1160px`, `padding:16px 24px`. Links: logo (hoogte 36) + woordmerk "VvE Beheer / Collectief" (Jost 300/500, 10.5px, uppercase, `letter-spacing:0.3em`, `#1C242E`). Midden (≥901px): Home · Diensten · Pakketten · Werkwijze · Over ons · Offerte (Jost 13px). Rechts: telefoon "(085)-800 0605" (`tel:+31858000605`) + primaire knop "Offerte aanvragen". **Plus behouden/toevoegen in header-stijl:** de **taalknop** (bestaand) en de **Klantenportaal**-link. Mobiel (≤900px): hamburger → paneel met nav-items + knoppen "Offerte aanvragen" (gevuld) en "Bel ons" (outline); `aria-label` wisselt "Menu openen/sluiten".

**Eind-CTA + footer (donker).** Elke pagina eindigt met donkere CTA-sectie (`linear-gradient(180deg,#232E3A,#1A212A)`) die naadloos overgaat in de footer (`#1A212A`). Footer: bovenblok (merkzin serif 30px + "Direct contact": telefoon, `mailto:info@vvebeheercollectief.nl`, plaats) + 4 linkkolommen — **Diensten**, **Bedrijf**, **Service** (met **Klantenportaal** → Twinq-URL), **Volg ons**. Onderbalk: wit logo (`filter:brightness(0) invert(1)`) + "VvE Beheer Collectief · KvK 51721139"; rechts Privacy · Voorwaarden · Cookies · Offerte aanvragen.

## 6. Pagina's

Exacte NL-teksten, prijzen, FAQ-vragen, pakket-features en vergelijkingsrijen staan in `docs/richting-d-handoff/` en zijn **leidend**. Samengevat:

1. **Home** — Hero (gradient + Den Haag-skyline met fade-mask, eyebrow "Sinds 2008 · Zuid-Holland", H1 serif 58, 2 knoppen) → Stats-strip (18+ / 500+ / 12.000+ / <24u) → Regio-strip → 3 pijlers (kaarten) → "Waarom" (sticky split, 4 rijen) → *Testimonials (uit)* → FAQ-accordeon (1 open tegelijk, eerste open) → Nieuwsbrief → eind-CTA/footer.
2. **Diensten** — Gecentreerde header op `#F5F7F9`, sticky sub-nav (Administratief/Financieel/Technisch, smooth-scroll met ~130–140px offset), 3 pijlerblokken (split), donker Compleetpakket-blok ("Meest gekozen", 8 punten), offerte-CTA.
3. **Pakketten** — 3 prijskaarten: **Start** (vanaf **€ 92,95** p/appartementsrecht p/jaar excl. btw), **Start aanvullend** (vanaf **€ 147,95**, uitgelicht: staalblauwe rand + ring, badge "Meest gekozen"), **Compleet** (Op aanvraag). 8 feature-regels per kaart (`✓`/`—`). Daaronder vergelijkingstabel (4 kolommen, gegroepeerd; mobiel horizontaal scrollbaar) + keuzehulp.
4. **Werkwijze** — Verticale tijdlijn (max 760px, `border-left:2px solid #E4E9ED`), 6 genummerde stappen (Dag 1–2 → Doorlopend), 3 geruststellings-kaarten, donkere CTA.
5. **Over ons** — Split-header met **fotoplek = placeholder** (`linear-gradient(160deg,#EEF2F5,#D6DEE6)`, aspect 1/1, radius 14), KPI-strip, missie-blok (serif statement), donkere CTA-balk.
6. **Offerte** — Split; links sticky intro (label/H1, chip "~2 minuten · geen verplichtingen", 4 voordelen, telefoon); rechts witte formulierkaart met **4-stappen-wizard** (voortgangsbalkjes; Stap 1 VvE-gegevens, Stap 2 diensten-keuzekaarten, Stap 3 situatie + vrije tekst, Stap 4 contact/functie). Navigatie: Terug / Volgende / **Aanvraag versturen** → zie §8. Na versturen: bevestigingsblok.

**Juridische routes (behouden):** Privacy, Voorwaarden, Cookies blijven als aparte content-routes (inhoud in het `NL`-object; footerlinks). Er is geen actieve cookie-consent-banner en die wordt niet toegevoegd (ongewijzigd).

## 7. Meertaligheid

Het bestaande taalsysteem blijft **volledig hergebruikt**:
- `useLanguage()` laadt `lang/<code>.json` en `t(section, key)` valt bij een ontbrekende sectie/sleutel automatisch terug op het Nederlandse `NL`-object. → Nieuwe teksten zonder vertaling tonen vanzelf **Nederlands**, geen lege plekken.
- Taalknop (desktop + mobiel) blijft zichtbaar; `localStorage('lang')`; `document.documentElement.dir` blijft RTL voor `ar`.
- **Aanpak content:** het `NL`-object krijgt de nieuwe Richting D-structuur/teksten. De bestaande `lang/*.json` bevatten sleutels van het óúde ontwerp; die matchen straks grotendeels niet meer en vallen dus terug op NL. Ze worden **niet verwijderd** (doen geen kwaad) en in de **vervolgronde** opnieuw gevuld op basis van de nieuwe NL-sleutels.
- Nieuwe/gewijzigde teksten krijgen bij voorkeur **nieuwe sleutelnamen**, zodat geen enkele óúde (nu onjuiste) vertaling per ongeluk blijft plakken op nieuwe tekst.

## 8. Formulieren

**Offerte-wizard → `mailto:`.** De inputs worden *controlled* (React-state verzamelt de waarden). Bij "Aanvraag versturen" bouwt de app een `mailto:info@vvebeheercollectief.nl?subject=Offerteaanvraag%20…&body=…` met de ingevulde velden (VvE-naam, aantal appartementen, bouwjaar, adres, gekozen diensten, situatie/toelichting, naam, functie, e-mail, telefoon), netjes geformatteerd en ge-`encodeURIComponent`-eerd. Daarna toont de app het bestaande bevestigingsscherm. **Aandachtspunt:** `mailto:` opent het mailprogramma van de bezoeker; de bezoeker klikt zelf "verzenden". Geen server-backend nodig (past bij GitHub Pages). Robuustere variant (formulierdienst als Formspree/Web3Forms) is bewust *niet* nu gekozen.

**Nieuwsbrief.** Blijft client-side bevestiging (`setDone(true)`), ongewijzigd. Koppelen aan een echte mailinglijst is een latere stap.

## 9. Beeld & assets

- **Skyline:** hergebruik `assets/skyline.png` (bestaat al; transparante lucht) met horizontale fade-mask in de hero. (De handoff-skyline is gelijkwaardig; niet nodig te dupliceren.)
- **Logo:** `assets/logo-donker.png` / `assets/icoon-*.png` bestaan; wit-op-donker via `filter:brightness(0) invert(1)` of `assets/logo-wit.png`.
- **"Over ons"-foto:** placeholder-verloop (geen bestand nodig) tot een echte foto wordt aangeleverd.
- **Favicon:** `assets/favicon.ico` behouden.
- **Fonts:** Source Serif 4 + Karla + Jost via Google Fonts (`<link>` in `<head>`), conform huidige conventie (nu ook Google Fonts).

## 10. Toegankelijkheid & responsive

- Zichtbare focus-ring (`#4E6885` + 3px `rgba(78,104,133,0.15)`) op links/knoppen/velden.
- `@media (prefers-reduced-motion: reduce)` → animaties uit, `data-reveal` direct zichtbaar.
- Breakpoint **900px**: grids → 1-koloms (g4 → 2), splits stapelen, sticky wordt statisch, H1 → 38 / H2 → 30, vergelijkingstabel horizontaal scrollbaar.
- Toetsenbord + `aria`: hamburger (`aria-label`, `aria-expanded`), FAQ-accordeon (button + `aria-expanded`), taalknop, formulier-labels gekoppeld aan inputs.

## 11. Uitrol

1. Bouwen op branch **`richting-d-restyle`** (main blijft = live, onaangeroerd).
2. Lokaal/preview verifiëren (browser-preview: render, console/network schoon, klikken door de 6 pagina's, taalknop, mobiel ≤900px, offerte-`mailto:`).
3. **Gebruiker bekijkt de preview** en geeft akkoord.
4. Merge/fast-forward naar **`main`** → GitHub Pages deployt automatisch live. `CNAME`/Pages-config ongewijzigd.

## 12. Acceptatiecriteria

- [ ] Alle 6 pagina's + header/footer ogen als Richting D (leisteen/papier/staalblauw, Source Serif 4/Karla/Jost).
- [ ] **Klantenportaal**-link (Twinq) werkt in header, footer en Service-kolom.
- [ ] Taalknop zichtbaar; niet-vertaalde nieuwe teksten vallen netjes terug op Nederlands; RTL voor `ar` intact.
- [ ] Privacy / Voorwaarden / Cookies bereikbaar en gevuld; KvK 51721139 in footer.
- [ ] Offerteformulier stelt een correcte `mailto:` samen met alle ingevulde velden en toont daarna het bevestigingsscherm.
- [ ] Responsive ≤900px correct; focus-states en `prefers-reduced-motion` gerespecteerd.
- [ ] Geen JS-fouten in de console; skyline/logo/favicon laden.

## 13. Risico's & aandachtspunten

- **`mailto:`** hangt af van een geconfigureerd mailprogramma bij de bezoeker; op mobiel opent doorgaans de mail-app. Acceptabel als no-backend-oplossing; formulierdienst is de upgrade-optie.
- **In-browser Babel** (geen bouwstap) is bewust behouden — groter runtime-parse, maar simpelste hosting. Grotere teksten/onderdelen laten `index.html` groeien (nu ~146 KB).
- **CDN-afhankelijkheid** (React/Babel/Google Fonts via unpkg/Google) blijft zoals nu.
- Oude `lang/*.json`-sleutels worden dood-gewicht tot de vertaalronde; functioneel onschadelijk door NL-terugval.
