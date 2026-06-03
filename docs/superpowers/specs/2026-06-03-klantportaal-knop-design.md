# Klantportaal-knop — Design

**Datum:** 2026-06-03
**Project:** VvE Beheer Collectief — marketing website (index.html)

## Doel

Bestaande klanten (die al bij ons in beheer zijn) snel naar de TwinQ-inlogpagina laten gaan vanaf de website, zonder dat ze deze hoeven te zoeken. Nieuwe bezoekers blijven gericht op de "Offerte aanvragen"-CTA.

## Doelgroep & verschil met bestaande CTA

- **Klantportaal-knop** → bestaande klanten, externe link naar TwinQ.
- **Offerte aanvragen-knop** → nieuwe klanten, interne pagina.

Daarom krijgt de portaal-knop een rustiger ("secondary") uiterlijk, zodat de oranje "Offerte aanvragen" de primaire CTA blijft.

## Link

```
https://vvebeheercollectief.twinq.nl/apex/f?p=TPL:101:::NO::TPL_APP:EPL
```

Opent in een **nieuw tabblad** (`target="_blank"` + `rel="noopener noreferrer"`). Externe link — gebruikt een gewone `<a href>`, niet `setPage()`.

## Plaatsing

**Desktop header** (`SiteHeader`, `.site-header-cta`):
Volgorde wordt: telefoon → taalkeuze → **Klantportaal** → Offerte aanvragen.
Uiterlijk: secondary knop met inlog-/sleutel-icoon (Phosphor `sign-in`), passend op de donkere navy-header (lichte rand, witte tekst).

**Mobiel** (`.mobile-menu` → `.mobile-cta`):
Klantportaal-knop toegevoegd bij de bestaande knoppen, onder "Offerte aanvragen".

## Tekst & vertaling

Tekst is vertaalbaar via een nieuwe key `nav.portaal_btn`, conform het bestaande `t()`-patroon. Toevoegen aan de NL-basis (inline in index.html) én de 8 taalbestanden in `lang/`:

| Taal | Tekst |
|------|-------|
| NL | Klantportaal |
| EN | Client portal |
| DE | Kundenportal |
| PL | Portal klienta |
| TR | Müşteriportaal → **Müşteri portalı** |
| AR | بوابة العملاء |
| ES | Portal de clientes |
| RU | Портал клиента |
| UK | Портал клієнта |

## Out of scope

- Geen wijzigingen aan TwinQ zelf.
- Geen aparte landingspagina; directe externe link volstaat.
- Geen single sign-on / deeplink per VvE.

## Verificatie

- Knop zichtbaar rechtsboven op desktop, opent TwinQ in nieuw tabblad.
- Knop zichtbaar in mobiel menu, zelfde gedrag.
- Knoptekst verandert mee met taalkeuze.
- "Offerte aanvragen" blijft visueel de primaire CTA.
