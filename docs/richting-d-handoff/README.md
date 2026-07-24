# Handoff: VvE Beheer Collectief — website restyle (Richting D)

## Overview
Dit pakket bevat het **nieuwe visuele ontwerp** voor de website van VvE Beheer Collectief: een cleane, rustige, conversiegerichte restyle in de vastgelegde huisstijl "Richting D" (navy/leisteen + staalblauw, serif-koppen). Het doel van de developer-taak is om dit **nieuwe uiterlijk toe te passen op de bestaande, live website** — niet om een nieuwe site from scratch te bouwen.

> ⚠️ **Twee dingen uit de bestaande site MOETEN behouden blijven** (ze zitten NIET in deze design-mockup, want die is puur het uiterlijk):
> 1. **De link naar het klantenportaal** — de bestaande login/portaal-integratie (URL, auth, redirect) blijft exact zoals hij is. In dit ontwerp is "Klantenportaal" alleen als menu-/footeritem aangeduid; koppel dat aan de bestaande portaal-URL.
> 2. **De meertaligheid / vertalingen** — de bestaande i18n (taalwissel + vertaalde content naar andere talen) blijft volledig intact. Deze mockup is alleen in het Nederlands opgesteld; hang de nieuwe teksten in het bestaande vertaalsysteem (i18n-keys) en behoud de taalschakelaar. Voeg de taalschakelaar toe in de nieuwe header/footer in dezelfde stijl als de overige nav-items (Jost, uppercase, klein).

Kort samengevat: **overneem het uiterlijk, behoud de functionaliteit** (portaal + talen, plus wat er verder al werkt: formulier-backend, analytics, cookiebanner, enz.).

## About the Design Files
De bestanden in `reference/` zijn **design-referenties, gemaakt in HTML** — een prototype dat het beoogde uiterlijk en gedrag toont, **geen productiecode om letterlijk te kopiëren**. `VvE Beheer Collectief website.dc.html` is geëxporteerd uit een designtool en gebruikt een eigen template-syntax (`<x-dc>`, `<sc-for>`, `<sc-if>`, `{{ … }}`-holes en een `class Component`-logicablok onderaan). **Neem die syntax niet over.** Lees het bestand puur als bron van layout, styling, maatvoering, kleuren, teksten en interactiegedrag.

De opdracht is om deze mockup **na te bouwen in de bestaande codebase** met de daar al gebruikte patronen en libraries (bv. het huidige CMS/framework — WordPress, React, Vue, of wat de live site ook draait). Bestaat er nog geen omgeving, kies dan het meest passende framework en implementeer het ontwerp daarin.

De teksten (kopij, prijzen, FAQ, pakketinhoud) in de mockup zijn definitief en mogen 1-op-1 worden overgenomen als NL-content in het vertaalsysteem.

## Fidelity
**High-fidelity (hifi).** Dit is een pixel-nauwkeurige mockup met definitieve kleuren, typografie, spacing en interacties. Bouw de UI zo getrouw mogelijk na met de bestaande libraries en componenten van de codebase. Alle exacte waarden staan onder **Design Tokens**.

---

## Screens / Views
De site is één doorlopende marketingsite met client-side sectie-/paginanavigatie in de mockup. In de echte site mogen dit gewoon **aparte routes/pagina's** zijn (`/`, `/diensten`, `/pakketten`, `/werkwijze`, `/over-ons`, `/offerte`) — de mockup wisselt ze alleen via state omdat het één prototypebestand is.

### Globaal — Header (sticky)
- **Layout:** sticky bovenaan, `z-index:60`, `background:rgba(255,255,255,0.9)` met `backdrop-filter:blur(12px)`, onderrand `1px solid #EEF1F4`. Inhoud gecentreerd, `max-width:1160px`, `padding:16px 24px`, flex met `space-between`.
- **Links:** logo (`assets/logo.png`, hoogte 36px) + woordmerk "VvE Beheer / Collectief" in Jost (300 resp. 500, 10.5px, uppercase, `letter-spacing:0.3em`, kleur `#1C242E`). Klik → home.
- **Midden (desktop, ≥901px):** nav-items — Home · Diensten · Pakketten · Werkwijze · Over ons · Offerte. Jost 13px, `letter-spacing:0.06em`. Actief item kleur `#4E6885`, inactief `#59616B`, `transition:color .2s`.
- **Rechts (desktop):** telefoonnummer "(085)-800 0605" (Karla 600, 14px, `#1C242E`, `href="tel:+31858000605"`) + primaire knop **Offerte aanvragen**.
- **Hier hoort ook:** de **taalschakelaar** (bv. NL/EN) en, indien gewenst, een **Klantenportaal**-link — beide in dezelfde nav-stijl. (Zaten niet in de mockup; toevoegen vanuit de bestaande site.)
- **Mobiel (≤900px):** desktop-nav en -CTA's verbergen; toon hamburger (44×44px, drie balkjes 22×2px `#1C242E`). Klik opent een paneel met de nav-items (Jost 14px, elk met onderrand `1px solid #F2F4F6`, `padding:13px 4px`) + twee knoppen: "Offerte aanvragen" (gevuld) en "Bel ons" (outline). `aria-label` wisselt "Menu openen"/"Menu sluiten".

### Globaal — Footer + eind-CTA (donkere afsluiting)
- Elke pagina eindigt met een **donkere CTA-sectie** die naadloos overgaat in de footer (samen één donker blok). CTA-achtergrond `linear-gradient(180deg,#232E3A 0%,#1A212A 100%)`, footer `#1A212A`.
- **Eind-CTA:** gecentreerd, max-width 820px. Label (Jost 12px `#8AA1B8` uppercase) + H2 serif 42px `#F5F8FB` + intro (Karla 17px `#AEB8C3`) + primaire knop + rij geruststellingen (Karla 14px `#8891A0`).
- **Footer:** max-width 1160px, `padding:80px 24px 32px`.
  - Bovenblok (2 kolommen 1.5fr/1fr): links merkzin (serif 30px `#F5F8FB`) + omschrijving; rechts "Direct contact" (telefoon, e-mail `info@vvebeheercollectief.nl`, plaats) + outline-knop.
  - Linkkolommen (4×): **Diensten** (Administratief/Financieel/Technisch beheer, Compleetpakket) · **Bedrijf** (Over ons, Werkwijze, Klantverhalen, Veelgestelde vragen) · **Service** (**Klantenportaal**, Schade melden, Veelgestelde vragen, Documenten) · **Volg ons** (LinkedIn, Nieuwsbrief). → **"Klantenportaal" koppelen aan de bestaande portaal-URL.**
  - Onderbalk: logo (wit via `filter:brightness(0) invert(1)`, 26px) + "VvE Beheer Collectief · KvK 51721139"; rechts Privacy · Voorwaarden · Cookies · Offerte aanvragen. Scheidingslijnen `rgba(138,161,184,0.16)`.

### 1. Home (`/`)
Secties in volgorde:
1. **Hero** — achtergrond `linear-gradient(115deg,#E4EDF0 0%,#EFF4F6 40%,#FBFCFD 100%)`. Rechtsonder de **skyline van Den Haag** (`assets/skyline.png`), breedte 56% (max 840px), met horizontale fade-mask `linear-gradient(to right, transparent 0%, #000 36%)`, ingezet vanaf de bodemrand (géén floating/marge onder de afbeelding). Content links, max-width 600px, `min-height:580px`, verticaal gecentreerd: eyebrow met streepje ("Sinds 2008 · Zuid-Holland"), H1 serif 58px ("Welkom bij **VvE Beheer Collectief**" — laatste deel `#4E6885`), intro 18px, twee knoppen (primair "Bekijk onze diensten →", secundair "Onze werkwijze"). Op mobiel zakt de skyline achter de tekst (breedte 150%, opacity 0.4) en krijgt de hero extra `padding-bottom:220px`.
2. **Stats strip** — 4 KPI's (serif 34px + label): `18+` Jaar ervaring · `500+` VvE's onder beheer · `12.000+` Eigenaren bediend · `< 24u` Gemiddelde reactietijd. Onderrand `#EEF1F4`.
3. **Regio-strip** — `background:#FBFCFD`, gecentreerd: label "Actief in uw regio" (Jost `#4E6885`) · verticaal streepje · "Den Haag · Rijswijk · Delft · Wateringen · heel Zuid-Holland" (Karla 600).
4. **Drie pijlers** — sectielabel "01 — Onze dienstverlening", H2 "Drie pijlers, één vast aanspreekpunt", intro. Daaronder 3 kaarten (grid 3×, gap 24px): elk een genummerd blokje 38×38px (`#4E6885`, witte cijfers, radius 8px), H3 serif 22px, omschrijving, 4 check-items (`✓` in `#4E6885`), "Lees meer →". Kaart: wit, `border:1px solid #ECEFF2`, `border-radius:12px`, `padding:38px 34px`, shadow `0 18px 50px -34px rgba(28,36,46,0.4)`; hover tilt op (`translateY(-5px)` + diepere shadow).
5. **Waarom** — `background:#F5F7F9`, 2-koloms split (0.82fr/1.18fr). Links sticky (`top:110px`): label "02 — Wat ons onderscheidt", H2 38px, intro. Rechts een lijst van 4 rijen (genummerd 01–04 in serif `#9CACBE`), elke rij met bovenrand `1px solid #E1E6EB`; hover schuift in (`padding-left:16px`, lichte tint `rgba(78,104,133,0.03)`). Items: Eigen klantenportaal · Vast aanspreekpunt · Snelle reactietijd · Compleetpakket.
6. **Klantervaringen** (optioneel, toggle) — label "03 — Wat klanten zeggen", 3 testimonial-kaarten (serif quote 18px + naam/rol). Onderschrift dat het voorbeeldcitaten zijn tot echte reviews beschikbaar zijn.
7. **FAQ** — `background:#F5F7F9`, max-width 820px. Label "04 — Veelgestelde vragen". Accordeon: elke vraag een witte kaart (radius 10px), klikbare rij met H3 serif 18.5px + chevron "⌄" die 180° draait bij openen. Eén item open tegelijk (eerste standaard open). 5 vragen (zie mockup voor exacte tekst).
8. **Nieuwsbrief** — gecentreerd, e-mailveld + knop "Inschrijven"; na submit een bevestigingsblok.
9. **Eind-CTA + footer** (zie Globaal).

### 2. Diensten (`/diensten`)
Gecentreerde header op `#F5F7F9`. Daaronder een **sticky sub-nav** (`top:69px`) met ankers Administratief · Financieel · Technisch die naar de pijlers scrollen (`scroll-margin-top:140px`). Drie pijler-blokken (split 1.05fr/1fr): links label/H2/intro + pakket-badge; rechts witte kaart met "Wat wij voor u regelen" (check-items) en "Het resultaat" (serif). Daarna een donker **Compleet-pakket-blok** (`linear-gradient(150deg,#26313E,#181F28)`, radius 14px, logo-watermerk rechtsonder op opacity 0.05) met badge "Meest gekozen", en een 2-koloms lijst met 8 punten. Afsluiten met offerte-CTA op `#F5F7F9`.

### 3. Pakketten (`/pakketten`)
Header op `#F5F7F9`. **Drie prijskaarten** (grid 3×, `align-items:start`): Start (vanaf **€ 92,95** p/appartementsrecht p/jaar excl. btw), **Start aanvullend** (vanaf **€ 147,95**, uitgelicht: staalblauwe rand + ring `0 0 0 2px #4E6885`, badge "Meest gekozen", niet ingezakt terwijl de andere `margin-top:16px` hebben), Compleet (**Op aanvraag**). Elke kaart: naam (serif 23px), korte omschrijving (min-height 62px), prijs (serif 36px) + sub, CTA-knop (uitgelicht = gevuld `#4E6885`, overige = outline), en 8 feature-regels met `✓`/`—` (opgenomen `#4E6885`/`#3B4653`, niet-opgenomen grijs `#9BA3AC`+`#C9D0D8`). Onder de kaarten: "Tarieven excl. 21% btw…" + 3 geruststellings-chips (Vrijblijvend & gratis · Geen risico · Overstap in 6–8 weken).
Daarna **vergelijkingstabel** op `#F5F7F9` (max-width 1000px): witte kaart met 4-koloms grid (Functie / Start / Start aanv. / Compleet), kolomkop "Start aanv." licht gemarkeerd `#EEF3F7`. Rijen gegroepeerd per categorie (Bestuurlijk, Secretarieel, Financieel, Technisch — dagelijks/planmatig). Op mobiel horizontaal scrollbaar (`min-width:680px` + `overflow-x:auto`). Afsluiten met "Welk pakket past bij onze VvE?" (split met keuzehulp-rijen).

### 4. Werkwijze (`/werkwijze`)
Header op `#F5F7F9`. **Verticale tijdlijn** (max-width 760px): lijn `border-left:2px solid #E4E9ED`, per stap een genummerde cirkel 24×24px (`#4E6885`, witte cijfers, witte rand 4px), fase-label (Jost), H3 serif 23px, tekst. Zes stappen (Dag 1–2 → Doorlopend). Daarna 3 "geruststelling"-kaarten op `#F5F7F9`. Afsluiten met donkere CTA.

### 5. Over ons (`/over-ons`)
Split-header (1.05fr/0.95fr): links label/H1/tekst + 3 regio-bulletjes; rechts een fotoplek (nu placeholder `linear-gradient(160deg,#EEF2F5,#D6DEE6)`, aspect 1/1, radius 14px — **vervangen door echte foto**). Daarna KPI-strip (4×) op `#F5F7F9`, dan missie-blok (label + serif statement 34px + tekst), dan donkere CTA-balk.

### 6. Offerte (`/offerte`)
Achtergrond `#F5F7F9`. Split (0.85fr/1.15fr): links **sticky** (`top:100px`) intro met label/H1, chip "Duurt ~2 minuten · geen verplichtingen", 4 voordelen, en telefoon. Rechts een **witte formulierkaart** (radius 14px, `padding:44px 46px`, shadow) met een **4-stappen wizard**:
- Voortgang: label "Stap X van 4" + 4 balkjes (`#4E6885` gevuld / `#E4E9ED` leeg).
- **Stap 1** Over uw VvE: naam VvE, aantal appartementen, bouwjaar, adres/locatie.
- **Stap 2** Welke diensten: 4 keuzekaarten (Compleetpakket, Administratief, Financieel, Technisch).
- **Stap 3** Huidige situatie: 3 chip-keuzes + vrije tekst.
- **Stap 4** Contactgegevens: voor-/achternaam, functie (select: Voorzitter/Penningmeester/Secretaris/Bestuurslid/Eigenaar-lid), e-mail, telefoon.
- Navigatie: "← Terug" (vanaf stap 2), "Volgende stap →" (stap 1–3), "Aanvraag versturen" (stap 4, donkere knop `#1C242E`). Na versturen: bevestigingsblok met logo. **Koppel de submit aan de bestaande formulier-backend/e-mail.**

---

## Interactions & Behavior
- **Navigatie:** menu-items en CTA's wisselen van pagina/route; bij paginawissel scrollt de pagina naar boven. In de echte site: gewone routing.
- **Hover:** knoppen tillen `translateY(-1…-2px)` en verdiepen van kleur (primair `#4E6885` → `#3F566F`, op donker → `#5A7796`); kaarten tillen `translateY(-4…-6px)` met diepere shadow; "waarom"-rijen schuiven in met lichte tint; nav-items verkleuren.
- **FAQ-accordeon:** klik toggelt één open item; chevron roteert 180° (`transition:transform .25s`).
- **Diensten sub-nav:** klik scrollt smooth naar de pijler met 130–140px offset (voor de sticky headers).
- **Offerte-wizard:** stap-state 1–4, vorige/volgende, submit → "bedankt"-scherm.
- **Nieuwsbrief:** submit → bevestigingsblok (in de echte site aan de mailinglijst koppelen).
- **Terug-naar-boven:** ronde knop rechtsonder verschijnt na `scrollY > 620px`, smooth scroll naar boven.
- **Mobiele actiebalk:** vaste balk onderaan (`bottom:0`, `z-index:65`) met "Offerte" + "Bel ons", alleen ≤900px.
- **Entree-animatie:** secties met `data-reveal` faden op met `omFadeUp` (translateY 20→0, opacity 0→1, `.7s cubic-bezier(.2,.6,.2,1)`). Hero-elementen komen gestaggerd binnen (delays .06/.13/.2s). Skyline faadt in met `omFade 1.1s`.
- **Responsive breakpoint:** `900px`. Grids (`g2/g3/g4`) worden 1-koloms (g4 → 2), splits stapelen, sticky-elementen worden statisch, H1 → 38px / H2 → 30px, tabel wordt scrollbaar.
- **Reduced motion:** bij `prefers-reduced-motion: reduce` alle animaties uit (`data-reveal` direct zichtbaar, animatieduur ~0).

## State Management
- `page` — actieve route/sectie (`home|diensten|pakketten|werkwijze|overons|offerte`). In de echte site: de router.
- `menuOpen` — mobiel menu open/dicht.
- `openFaq` — index van open FAQ-item (−1 = alle dicht; standaard 0).
- `offStep` — 1–4, actieve stap van de offerte-wizard.
- `formDone` — offerteformulier verstuurd (toon bevestiging).
- `nlDone` — nieuwsbrief ingeschreven (toon bevestiging).
- `showTop` — terug-naar-boven-knop zichtbaar (afgeleid van scrollpositie > 620px).
- Twee content-toggles uit de mockup: `showTestimonials` (verberg testimonials tot echte reviews er zijn) en `showPhotoLabels` (toon "Fotografie —"-labels op placeholders). In productie vervallen deze zodra echte reviews/foto's aanwezig zijn.

## Design Tokens

### Kleuren
| Rol | Hex |
|-----|-----|
| Tekst hoofd (bijna-zwart navy) | `#1C242E` |
| Tekst body / secundair | `#59616B` · `#68717C` |
| Tekst tertiair / muted | `#7A828C` · `#96A0AB` · `#9BA3AC` |
| Tekst op donker (licht) | `#3B4653` (chips), `#C3CBD4`, `#AEB8C3`, `#8891A0` |
| Leisteen / donker vlak | `#222B36` · `#1C242E` |
| Donkere gradient (CTA/footer) | `#232E3A` → `#1A212A` (180°); blokken `#26313E` → `#181F28` (150°) |
| Accent staalblauw (primair) | `#4E6885` |
| Accent hover (donkerder) | `#3F566F` |
| Accent op donker (hover) | `#5A7796` |
| Accent licht / mist | `#8AA1B8` · `#9CACBE` |
| Papier / off-white | `#FBFCFD` |
| Sectie-achtergrond licht | `#F5F7F9` |
| Wit | `#FFFFFF` · tekst-op-donker `#F5F8FB` / `#F7F9FB` |
| Randen / hairlines | `#EEF1F4` · `#ECEFF2` · `#E1E6EB` · `#CDD5DD` · `#E4E9ED` |
| Chip/markering-vlak | `#EEF3F7` · `#EAF0F4` · `#F6F9FB` |
| Link | `#4E6885`, hover `#3b5069` |
| Focus-ring | `rgba(78,104,133,0.15)` (3px) + border `#4E6885` |

### Typografie (Google Fonts)
- **Source Serif 4** (400/500/600) — koppen, titels, prijzen, statements. H1 `letter-spacing:-0.015em`, H2 `-0.01em`.
- **Karla** (400/500/600/700) — lopende tekst, labels in kaarten, knoptekst-alternatief. Regelafstand ~1.6–1.75.
- **Jost** (300/400/500) — kleine kapitalen/labels, nav, knoppen, eyebrows. UPPERCASE, `letter-spacing` 0.06em (nav) tot 0.28em (eyebrows).
- **Schaal:** H1 hero 58 / pagina 40–46px · H2 sectie 38–42px · H3 20–23px · body 15–18px · label/eyebrow 11–12px · knop 11.5–12.5px.

### Spacing & maatvoering
- Content-breedtes: `1160px` (breed), `1080px` (offerte), `1000px` (diensten/pakketten/werkwijze/over ons), `820px` (tekstsecties, FAQ), `760px` (tijdlijn), `640px` (gecentreerde koppen).
- Sectie-padding verticaal: `104px` (ruime home-secties), `88–96px` (paginaheaders), `64px` (mobiel via `secpad`). Horizontaal overal `24px`.
- Grid-gap: 24px (kaarten), 32–34px (KPI/footer), 52–64px (splits).
- Breakpoint: `900px`.

### Radius & shadow
- Radius: knoppen `4–5px`, chips/pills `40px`, kaarten `10–14px`, kleine blokjes `3–8px`.
- Shadows (kaarten): `0 18px 50px -34px rgba(28,36,46,0.4)`; hover `0 34px 70px -34px rgba(28,36,46,0.5)`; prijskaarten `0 24px 60px -46px rgba(28,36,46,0.4)`, uitgelicht `0 46px 92px -42px rgba(28,36,46,0.6)` + ring `0 0 0 2px #4E6885`.

### Animatie-keyframes
- `omFadeUp`: `opacity 0→1`, `translateY(20px)→0`.
- `omFade`: `opacity 0→1`.
- `omFloat`: subtiel op/neer (`translateY 0 → -12px → 0`) — optioneel, niet noodzakelijk.
- Timing: `.6–.7s cubic-bezier(.2,.6,.2,1)`; hovers `.2–.25s ease`.

## Assets
- **`reference/assets/logo.png`** — logo (huis-/handen-icoon, leisteen op transparant). Wit maken op donker met `filter:brightness(0) invert(1)`. Vervang door de officiële SVG als die in de codebase beschikbaar is.
- **`reference/assets/skyline.png`** — silhouet van de Haagse skyline, gebruikt in de hero met een fade-mask. Transparante lucht. Wordt met een mask/gradient in de achtergrond verwerkt (niet als los zwevend plaatje).
- **Fotografie:** de fotoplekken (hero-visual, "Over ons") zijn nu placeholders (subtiel verloop + label). Vervangen door echte, ingetogen fotografie van panden/entrees/regio conform de merkrichtlijnen.
- **Fonts:** Source Serif 4, Karla, Jost — via Google Fonts (of self-hosted volgens codebase-conventie).

## Files
- `reference/VvE Beheer Collectief website.dc.html` — de volledige hifi-mockup (alle 6 pagina's, header/footer, interacties). **Lees als referentie; niet de template-syntax overnemen.** Alle exacte teksten (NL), prijzen, FAQ, pakket- en vergelijkingsinhoud staan hierin.
- `reference/assets/logo.png`, `reference/assets/skyline.png` — beeldassets.

## Bewaar-checklist voor de developer
- [ ] Nieuw uiterlijk (deze tokens + layouts) toegepast op alle bestaande pagina's.
- [ ] **Klantenportaal-link** behouden en gekoppeld (header/footer/Service-kolom) aan de bestaande portaal-URL/auth.
- [ ] **Meertaligheid** behouden: NL-teksten uit de mockup in het i18n-systeem, taalschakelaar zichtbaar in de nieuwe header/footer, overige talen intact.
- [ ] Bestaande formulier-backend, analytics, cookie-/privacypagina's en KvK-gegevens behouden.
- [ ] Placeholders vervangen door echte fotografie; voorbeeldtestimonials pas tonen als ze echt zijn.
- [ ] Responsive gedrag (≤900px), toetsenbord/focus-states en `prefers-reduced-motion` gerespecteerd.
