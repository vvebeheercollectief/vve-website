# Spec — v3 "Helder" landingspagina + footer herontwerp

**Datum:** 2026-06-04
**Bron:** `design_handoff_vve_landing/Richting5-Helder v3.html` + README (handoff "Collectief website new-8.zip")
**Doel:** De home/landingspagina (desktop + mobiel) en de footer van `index.html` herontwerpen naar het v3 "Helder"-ontwerp. De nieuwe hero alleen voor desktop (mobiele hero "Optie E" blijft ongewijzigd).

## Scope-besluiten (door gebruiker bevestigd)
- **Talen:** Alleen Nederlands nu. Nieuwe strings in het `NL`-object; andere talen vallen terug op NL (geen JSON-edits).
- **Vervangen:** Volledig volgens v3. Fotoblok (differentiators) → 4 reden-kaarten. Donkere reviews-balk + keurmerk-strip → lichte review-kaarten (keurmerk-strip vervalt; bedrijf is geen lid).
- **Reviews:** Prototype-namen overnemen (Marian Koster, Robert de Jong, Sandra Elzinga).
- **Echte gegevens:** KvK 51721139, (085)-800 0605, info@vvebeheercollectief.nl, Wateringen — niet de prototype-placeholders.
- **Nieuwsbrief:** Alleen visueel (geen mailprovider-koppeling).

## Architectuur
- Eén `index.html`, React 18 (CDN) + Babel, bestaande `t()`-vertaalfunctie, `.dz-`-prefix voor geïsoleerde nieuwe CSS.
- Knoppen/links blijven via `setPage(...)` naar de juiste route (offerte/diensten/werkwijze) i.p.v. de prototype-router.
- Buttons hergebruiken bestaande `.btn`-klassen.

## Onderdelen (home, boven → onder)
1. **Hero (desktop)** — `.dz-pill` locatie-pill vervangen door `.dz-since` "Sinds 2008"-marker (fijne lijntjes links/rechts). Rest van de hero (skyline, h1, lead, CTA's, trust-chips) ongewijzigd. Mobiele hero ongemoeid.
2. **Wat doen wij** (`band`) — 3 pijler-kaarten: teal-50 icoon-chip, h3, beschrijving, 3-punts checklist, "Lees meer →" met bovenrand. + afsluitbalk `.dz-svc-note` (handshake + tekst links, "Bekijk alle diensten →" rechts).
3. **Waarom kiezen** (`band-tint`) — 4 reden-kaarten (`.dz-reasons`/`.dz-rcard`): Eigen klantenportaal / Vast aanspreekpunt / Snelle reactietijd / Compleet-pakket. Geen statistieken.
4. **Donkere CTA** (`band-dark`, full-bleed) — eyebrow + h2 + lead + 2 knoppen (Offerte aanvragen → offerte; Bel ons direct → tel:) + 4 geruststellings-regels (`.dz-cta-list` met check-circle).
5. **FAQ** (`band`) — gecentreerde kop + accordion (6 items, witte kaarten, "+" → "×" rotatie, meerdere tegelijk open). Footerregel "Staat uw vraag er niet bij?".
6. **Reviews** (`band-tint`) — gecentreerde kop + 3 lichte review-kaarten (5 sterren, quote, initialen-avatar + naam/rol).
7. **Nieuwsbrief** (`band`) — zacht teal-paneel, e-mailveld + "Inschrijven"-knop + slot-regel. Alleen visueel.

## Footer (nieuw, editorial, licht)
- `.dz-foot-hero` (2-koloms): links grote merkkop "Onbezorgd wonen, **samen geregeld.**" + lead; rechts witte contactkaart (DIRECT CONTACT, telefoon/e-mail/locatie, full-width knop → offerte).
- `.dz-foot-in` (4 kolommen): Diensten / Bedrijf / Service / Volg ons.
- `.dz-foot-bottom`: © 2026 VvE Beheer Collectief · KvK 51721139 + juridische links (Privacy/Voorwaarden/Cookies → bestaande pagina's).
- Responsief: 2-koloms < 860px, 1-koloms < 520px.

## Section-bands (ritme)
- `.dz-band` `#FBFCFD`; `.dz-band-tint` licht-teal gradient; `.dz-band-dark` navy full-bleed met teal-gloed.

## Nieuwe NL-strings (toe te voegen aan `NL`)
- `hero.since` ("Sinds 2008")
- `pillars_section.note` + `note_link` (afsluitbalk)
- `reasons_section` (eyebrow/title/subtitle) + `reasons[4]{icon,title,desc}`
- `dark_cta` (eyebrow/title/subtitle/btn_primary/btn_secondary/list[4])
- `faq_section` aanpassen naar gecentreerd (title/eyebrow) + `faq_foot`
- `reviews` (prototype-namen) — hergebruik/vervang `testimonials`
- `newsletter` (eyebrow/title_1/title_2/subtitle/placeholder/btn/note)
- `footer` uitbreiden: `bigname_1/bigname_2`, `lead`, `contact_label`, link-kolommen (Service/Volg ons), behoud bestaande links

## Verificatie
- Preview-server starten, home renderen, console-errors checken, screenshots desktop + mobiel (resize), accordion + hover testen.
- Robuustheid: `.dz-reveal` content nooit onzichtbaar (reduced-motion/print forceren zichtbaar) — bestaande `Reveal`-component hergebruiken.
