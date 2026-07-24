# Richting D-restyle — Implementatieplan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Het uiterlijk van de live VvE Beheer Collectief-website vervangen door de huisstijl "Richting D" (leisteen/papierwit + staalblauw, serif-koppen), met behoud van alle werkende functionaliteit (taalsysteem, Twinq-portaal, juridische pagina's, favicon) en met een offerteformulier dat aanvragen via `mailto:` verstuurt.

**Architecture:** Eén `index.html` = React 18 (UMD) + Babel-standalone in de browser, gemount op `#root`. **Geen bouwstap.** We herbouwen de presentatielaag (het `<style>`-blok, het `NL`-content-object en de componentboom in `<script type="text/babel">`) naar Richting D, en hergebruiken de bestaande "machinerie" (`useLanguage`/`t`, `LANGUAGES`, `I`, `Photo`). De volledige referentie-implementatie staat in `docs/richting-d-handoff/mockup.dc.html` (layout + inline-styles + data) en `docs/richting-d-handoff/README.md`.

**Tech Stack:** HTML + in-browser JSX (React 18.3.1, ReactDOM, @babel/standalone 7.29.0 via unpkg), Google Fonts (Source Serif 4 / Karla / Jost), Phosphor-iconen (bestaand). GitHub Pages hosting (auto-deploy bij push naar `main`).

---

## Verificatie-aanpak (lees dit eerst)

Dit project heeft **geen unit-test-harness** en bewust **geen bouwstap** (besluit gebruiker: één-bestands-site behouden). Klassieke "schrijf eerst een falende pytest"-TDD past hier niet. In plaats daarvan verifieert elke taak via de **browser-preview-tools**:

- `preview_start` met de config uit Taak 0 (statische server) → tab openen.
- Na een codewijziging: `navigate` naar de URL opnieuw (of `javascript_tool: window.location.reload()`).
- `read_console_messages` → **verwacht: geen errors** (een JSX/Babel-fout breekt de hele pagina en verschijnt hier).
- `read_page` / `computer{screenshot}` → bevestig dat de verwachte tekst/secties er staan.
- `computer{left_click}` / `form_input` → interacties testen; daarna `read_page` om het resultaat te bevestigen.

**Belangrijk:** het taalsysteem doet `fetch('lang/<code>.json')`; dat werkt alleen over http (niet `file://`). Draai dus altijd via de statische server uit Taak 0.

**Idiom-conversie mockup → productie (geldt voor élke pagina-taak; hier één keer beschreven — DRY):**

1. `style="a:b; c:d;"` (string) → `style={{ a:'b', c:'d' }}` (camelCase-keys: `background-color`→`backgroundColor`, `border-radius`→`borderRadius`, enz.).
2. `onClick="{{ fn }}"` → `onClick={fn}`.
3. Tekst-hole `{{ expr }}` → `{expr}`. **Hardgecodeerde NL-tekst gaat naar het `NL`-object** en wordt gelezen via `t('sectie').key` (zie Taak 1).
4. `<sc-for list="{{ arr }}" as="x"> … {{ x.y }} … </sc-for>` → `{arr.map((x, i) => ( <Elem key={i}> … {x.y} … </Elem> ))}`.
5. `<sc-if value="{{ cond }}"> … </sc-if>` → `{cond && ( <> … </> )}`.
6. `style-hover="…"` bestaat niet in React → vervang door een **hover-class** (gedefinieerd in het `<style>`-blok, Taak 1) via `className`. De inline base-`style` blijft; de class voegt alleen `:hover` toe.
7. `data-r="…"` en `data-reveal` **blijven ongewijzigd** als attributen (het `<style>`-blok target ze voor responsive + entree-animatie).
8. Iconen: de mockup gebruikt tekstglyphs (`✓ — → ↑`) en de chevron voor FAQ — houd die als letterlijke tekst; geen icon-font nodig. `I`/`Photo` blijven beschikbaar voor waar nodig.

---

## Bestandsstructuur

Alles blijft in **één** `index.html`, intern geordend in duidelijke secties binnen `<script type="text/babel">`:

- `LANGUAGES` (bestaand, ongewijzigd) — talen + `dir` (RTL voor `ar`).
- `NL` (**vervangen**) — het Nederlandse content-object; alle marketingteksten + behouden juridische teksten.
- `useLanguage()` / `t()` (bestaand, ongewijzigd) — laadt `lang/*.json`, valt terug op `NL`.
- Helpers `I`, `Photo` (bestaand) + nieuw `LanguageSwitcher`, `mailtoOfferte()` (Taak 1/7).
- Componenten (**herbouwd**): `App`, `Header`, `Footer` (incl. eind-CTA), `HomePage`, `DienstenPage`, `PakkettenPage`, `WerkwijzePage`, `OverOnsPage`, `OffertePage`, `LegalPage` (privacy/voorwaarden/cookies).
- Mount: `ReactDOM.createRoot(document.getElementById('root')).render(<App/>)` (bestaand).

Nieuwe/aan te raken bestanden buiten `index.html`:
- `.claude/launch.json` (**maken**) — preview-server.
- `assets/logo.png` (**toevoegen** uit handoff), `assets/skyline.png` (**verversen** uit handoff).

---

### Taak 0: Preview-harness + assets

**Files:**
- Create: `/Users/servicedesk/vve-website/.claude/launch.json`
- Add: `/Users/servicedesk/vve-website/assets/logo.png`
- Refresh: `/Users/servicedesk/vve-website/assets/skyline.png`

- [ ] **Stap 1: Maak de preview-serverconfig**

Create `.claude/launch.json`:

```json
{
  "version": "0.0.1",
  "configurations": [
    {
      "name": "vve-website",
      "runtimeExecutable": "python3",
      "runtimeArgs": ["-m", "http.server", "8000"],
      "port": 8000
    }
  ]
}
```

- [ ] **Stap 2: Kopieer de handoff-assets in de repo**

Run:

```bash
cp "/private/tmp/claude-502/-Users-servicedesk/0a63fd5a-7253-4f69-bec8-8b29e6030c81/scratchpad/brand/design_handoff_website_redesign/reference/assets/logo.png" /Users/servicedesk/vve-website/assets/logo.png
cp "/private/tmp/claude-502/-Users-servicedesk/0a63fd5a-7253-4f69-bec8-8b29e6030c81/scratchpad/brand/design_handoff_website_redesign/reference/assets/skyline.png" /Users/servicedesk/vve-website/assets/skyline.png
ls -la /Users/servicedesk/vve-website/assets/logo.png /Users/servicedesk/vve-website/assets/skyline.png
```

Expected: beide bestanden bestaan (logo ~37 KB, skyline ~1 MB).

> Als de scratchpad-map weg is, gebruik dan de originele download: pak `assets/` uit `~/Downloads/Brand kleuren en typografie.zip`.

- [ ] **Stap 3: Start de preview en bevestig dat de (nog oude) site laadt**

`preview_start` → `{name: "vve-website"}`. Navigeer naar `http://localhost:8000/`. `read_console_messages` → geen errors. `computer{screenshot}` → de huidige (teal) site rendert. Dit bevestigt dat de harness werkt vóór we verbouwen.

- [ ] **Stap 4: Commit**

```bash
cd /Users/servicedesk/vve-website
git add .claude/launch.json assets/logo.png assets/skyline.png
git commit -m "chore: preview-server config + Richting D-assets (logo, skyline)

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Taak 1: Fundament — fonts, `<style>`, `NL`-content-object, app-shell + chrome + pagina-stubs

Dit levert een **werkende skelet-site**: nieuwe header/footer/eind-CTA, taalknop, mobiel menu, back-to-top, en navigatie tussen (voorlopig gestubde) pagina's. Alle content-data zit al in `NL`. Groot maar samenhangend; hierna wordt elke pagina afzonderlijk ingevuld.

**Files:**
- Modify: `/Users/servicedesk/vve-website/index.html` (`<head>` fonts-`<link>`; het hele `<style>`-blok; het `NL`-object; `App`+`Header`+`Footer`+chrome+stubs in `<script type="text/babel">`)

- [ ] **Stap 1: Vervang de fonts-`<link>` in `<head>`**

Verwijder de DM Sans-`<link>` en zet Richting D-fonts (Phosphor-links laten staan — `I` gebruikt ze):

```html
<link href="https://fonts.googleapis.com/css2?family=Source+Serif+4:opsz,wght@8..60,400;8..60,500;8..60,600&family=Karla:wght@400;500;600;700&family=Jost:wght@300;400;500&display=swap" rel="stylesheet" />
```

- [ ] **Stap 2: Vervang de inhoud van het `<style>`-blok**

Zet het basis-`<style>` (overgenomen uit mockup, plus hover-utilities en de herstijlde taalknop). Dit is het gedeelde ontwerp-systeem; per-element-styling gebeurt inline in de componenten.

```css
:root{
  --leisteen:#222B36; --navy:#1C242E; --staal:#4E6885; --staal-hover:#3F566F;
  --papier:#FBFCFD; --sectie:#F5F7F9; --muted:#59616B; --muted2:#68717C;
  --line:#EEF1F4;
}
*{ box-sizing:border-box; }
html{ scroll-behavior:smooth; }
body{ margin:0; background:#FFFFFF; color:#1C242E; font-family:'Karla',system-ui,Arial,sans-serif; -webkit-font-smoothing:antialiased; }
img{ max-width:100%; display:block; }
a{ color:#4E6885; text-decoration:none; }
a:hover{ color:#3b5069; }
button{ font-family:inherit; cursor:pointer; }
input,textarea,select{ font-family:'Karla',sans-serif; }
::placeholder{ color:#AAB2BB; }
input:focus,textarea:focus,select:focus{ outline:none; border-color:#4E6885 !important; box-shadow:0 0 0 3px rgba(78,104,133,0.15); }
:focus-visible{ outline:2px solid #4E6885; outline-offset:2px; }

@keyframes omFadeUp{ from{opacity:0; transform:translateY(20px);} to{opacity:1; transform:translateY(0);} }
@keyframes omFade{ from{opacity:0;} to{opacity:1;} }
@keyframes omFloat{ 0%,100%{transform:translateY(0);} 50%{transform:translateY(-12px);} }
[data-reveal]{ animation:omFadeUp .7s cubic-bezier(.2,.6,.2,1) both; }

/* hover-utilities (vervangen mockup's style-hover) */
.h-btnP:hover{ background:#3F566F !important; transform:translateY(-1px); }
.h-btnP2:hover{ background:#3F566F !important; transform:translateY(-2px); }
.h-btnO:hover{ border-color:#4E6885 !important; transform:translateY(-2px); }
.h-card:hover{ transform:translateY(-5px); box-shadow:0 34px 70px -34px rgba(28,36,46,.5) !important; }
.h-card4:hover{ transform:translateY(-4px); box-shadow:0 30px 60px -40px rgba(28,36,46,.45) !important; }
.h-row:hover{ padding-left:16px !important; background:rgba(78,104,133,.03) !important; }
.h-nav:hover{ color:#4E6885 !important; }
.h-link:hover{ color:#3F566F !important; }

/* taalknop (herstijld naar Richting D) */
.lang-switcher{ position:relative; }
.lang-toggle{ display:flex; align-items:center; gap:7px; background:none; border:1px solid #CDD5DD; border-radius:4px; padding:9px 12px; font-family:'Jost'; font-size:12px; letter-spacing:0.06em; text-transform:uppercase; color:#28323E; cursor:pointer; }
.lang-toggle:hover{ border-color:#4E6885; color:#1C242E; }
.lang-dropdown{ position:absolute; top:calc(100% + 8px); right:0; background:#fff; border:1px solid #E1E6EB; border-radius:8px; box-shadow:0 18px 50px -30px rgba(28,36,46,.4); padding:6px; min-width:180px; opacity:0; pointer-events:none; transform:translateY(-6px); transition:.18s; z-index:80; max-height:320px; overflow:auto; }
.lang-dropdown.open{ opacity:1; pointer-events:all; transform:translateY(0); }
.lang-option{ display:flex; align-items:center; gap:8px; width:100%; text-align:left; background:none; border:none; padding:9px 10px; border-radius:6px; font-family:'Karla'; font-size:14px; color:#28323E; cursor:pointer; }
.lang-option:hover{ background:#F5F7F9; }
.lang-option.active{ background:#EEF3F7; color:#1C242E; font-weight:600; }
.lang-option-code{ margin-left:auto; font-size:11px; color:#96A0AB; font-family:'Jost'; text-transform:uppercase; }

@media (max-width:900px){
  [data-r="dnav"], [data-r="dcta"], [data-r="dlang"]{ display:none !important; }
  [data-r="burger"]{ display:flex !important; }
  [data-r="mbar"]{ display:flex !important; }
  [data-r="split"]{ grid-template-columns:1fr !important; gap:36px !important; }
  [data-r="g3"], [data-r="g2"]{ grid-template-columns:1fr !important; }
  [data-r="g4"]{ grid-template-columns:1fr 1fr !important; }
  [data-r="row"]{ flex-direction:column !important; align-items:flex-start !important; }
  [data-r="hero"]{ min-height:0 !important; padding-top:60px !important; padding-bottom:220px !important; }
  [data-r="sky"]{ width:150% !important; right:-26% !important; opacity:0.4 !important; }
  [data-r="h1"]{ font-size:38px !important; }
  [data-r="h2"]{ font-size:30px !important; }
  [data-r="scrollx"]{ overflow-x:auto; -webkit-overflow-scrolling:touch; }
  [data-r="tablemin"]{ min-width:680px !important; }
  [data-r="offsticky"]{ position:static !important; }
  [data-r="secpad"]{ padding-top:64px !important; padding-bottom:64px !important; }
  p, li{ line-height:1.75; }
}
@media (prefers-reduced-motion: reduce){
  [data-reveal]{ opacity:1 !important; transform:none !important; animation:none !important; }
  *{ animation-duration:.001s !important; animation-iteration-count:1 !important; scroll-behavior:auto !important; }
}
```

- [ ] **Stap 3: Vervang het `NL`-content-object**

Bouw `NL` op door de data-arrays uit de mockup-Component (`docs/richting-d-handoff/mockup.dc.html`, regels 735–893) over te nemen, plus de losse koppen/eyebrows die inline in de mockup-JSX staan. **Verwijder alle inline-styles uit de data** (die `style:`/`cardStyle:`/`btnStyle:`-strings horen in de componenten, niet in content). Behoud de bestaande juridische secties.

Doel-structuur (waarden = letterlijke NL-tekst uit mockup/README; iconen/kleuren weglaten):

```js
const NL = {
  nav: { home:'Home', diensten:'Diensten', pakketten:'Pakketten', werkwijze:'Werkwijze', overons:'Over ons', offerte:'Offerte',
         offerte_btn:'Offerte aanvragen', bel:'Bel ons', portaal:'Klantenportaal', menu_open:'Menu openen', menu_close:'Menu sluiten' },
  hero: { eyebrow:'Sinds 2008 · Zuid-Holland', title_1:'Welkom bij', title_2:'VvE Beheer Collectief',
          subtitle:'Wij zorgen dat u onbezorgd kunt wonen. Alles transparant en één vast aanspreekpunt.',
          cta_diensten:'Bekijk onze diensten →', cta_werkwijze:'Onze werkwijze' },
  kpis: [ {v:'18+', l:'Jaar ervaring in VvE-beheer'}, {v:'500+', l:"VvE's onder ons beheer"},
          {v:'12.000+', l:'Eigenaren bediend'}, {v:'< 24u', l:'Gemiddelde reactietijd'} ],
  regio: { label:'Actief in uw regio', plaatsen:'Den Haag · Rijswijk · Delft · Wateringen · heel Zuid-Holland' },
  pijlers: { eyebrow:'01 — Onze dienstverlening', title:'Drie pijlers, één vast aanspreekpunt',
             intro:'Administratief, financieel en technisch beheer onder één dak. U krijgt geen wisselende contactpersonen, maar één beheerder die uw gebouw kent.',
             lees_meer:'Lees meer →', onder:'Niet zeker welk beheer past bij uw VvE?', onder_link:'Bekijk alle diensten →',
             items:[ /* pijlersHome uit regels 797–801: {n,title,desc,items[]} */ ] },
  waarom: { eyebrow:'02 — Wat ons onderscheidt', title:'Waarom eigenaren voor ons kiezen',
            intro:'Het beheer van een VvE staat of valt met transparantie en bereikbaarheid. Daar bouwden wij onze hele werkwijze omheen.',
            rows:[ /* waarom uit regels 802–807: {title,text} (nummer n = index) */ ] },
  faq: { eyebrow:'04 — Veelgestelde vragen', items:[ /* faqRaw uit regels 808–814: {q,a} */ ] },
  testimonials: { eyebrow:'03 — Wat klanten zeggen', title:'Vertrouwd door bestuurders en eigenaren',
                  items:[ /* testimonials 817–821: {quote,name,role} */ ],
                  note:'Voorbeeldcitaten tot echte reviews beschikbaar zijn.' },
  nieuwsbrief: { title:'Blijf op de hoogte', intro:'…', placeholder:'Uw e-mailadres', btn:'Inschrijven', done:'Bedankt voor uw inschrijving!' },
  diensten_page: { /* header + dienstIndex(827–831) + dienstenPijlers(822–826) + compleet-blok + compleetPunten(832) + CTA */ },
  pakketten_page: { /* header + packages(787–794 zonder styles) + compare(745–782) + keuzehulp(833–838) + prijsnoot + chips */ },
  werkwijze_page: { /* header + werkwijze(839–846) + geruststelling(847–851) */ },
  overons_page: { /* header + tekst + regiobullets + missie-statement (zie README §5) */ },
  offerte_page: { eyebrow:'Vrijblijvende offerte', title:'…', intro:'…',
                  benefits:[ /* offerteBenefits 870 */ ], stap_van:'Stap',
                  step_titles:['Over uw VvE','Welke diensten','Huidige situatie','Contactgegevens'],
                  step1:{ vve_naam:'Naam VvE', vve_naam_ph:'…', aantal:'Aantal appartementsrechten', aantal_ph:'…', bouwjaar:'Bouwjaar', bouwjaar_ph:'…', adres:'Adres / locatie', adres_ph:'…' },
                  step2:{ intro:'…', opts:[ /* step2opts 864–869: {t,s} */ ] },
                  step3:{ situatie_label:'Huidige situatie', chips:['Nieuwe VvE','Overstap van beheerder','Zelfbeheer nu','Anders'], toelichting_label:'Toelichting', toelichting_ph:'…' },
                  step4:{ voornaam:'Voornaam', achternaam:'Achternaam', functie_label:'Functie binnen de VvE', functie_opts:[ /* functieOpts 871 */ ], email:'E-mailadres', telefoon:'Telefoonnummer' },
                  terug:'← Terug', volgende:'Volgende stap →', versturen:'Aanvraag versturen',
                  success_title:'Bedankt voor uw aanvraag!', success_desc:'…' },
  cta: { eyebrow:'Vrijblijvende offerte', title:'Klaar voor onbezorgd VvE-beheer?', intro:'…', btn:'Offerte aanvragen', reassure:'Vrijblijvend · Binnen 2 werkdagen reactie · Geen risico' },
  footer: { merkzin:'Onbezorgd wonen, samen geregeld.', omschrijving:'Persoonlijk, transparant en betrouwbaar VvE-beheer voor Den Haag en heel Zuid-Holland. 18 jaar ervaring, 500+ VvE\'s in beheer.',
            contact_label:'Direct contact', plaats:'Wateringen, Zuid-Holland', contact_btn:'Neem direct contact op',
            cols:[ /* footerCols 858–863: {h,links[]} */ ], kvk:'VvE Beheer Collectief · KvK 51721139',
            privacy:'Privacy', voorwaarden:'Voorwaarden', cookies:'Cookies', offerte_link:'Offerte aanvragen →' },
  // BEHOUDEN uit de huidige site (niet herschrijven): kopieer de bestaande secties 1-op-1.
  privacy_page: { /* bestaande NL.privacy_page ongewijzigd overnemen */ },
  voorwaarden_page: { /* bestaande NL.voorwaarden_page ongewijzigd overnemen */ },
  cookies_page: { /* bestaande NL.cookies_page ongewijzigd overnemen */ },
  titles: { /* bestaande page-titles overnemen/aanvullen */ },
};
```

Vul elke `/* … */` met de exacte NL-waarden uit de aangegeven mockup-regels (verbatim, dat is de definitieve copy). Voor `overons_page`, `nieuwsbrief`, `offerte_page.intro` e.d. die niet in de Component-data staan: neem de tekst uit de mockup-JSX-body (secties in `docs/richting-d-handoff/mockup.dc.html`) of README §5/§6.

- [ ] **Stap 4: Reuse-check van de machinerie**

Laat `LANGUAGES`, `useLanguage`, `t`, `I`, `Photo` **ongewijzigd** staan. Bevestig dat `t(section, key)` de 2-arg-vorm heeft met NL-terugval (regels rond 1437–1443 in de huidige `index.html`). Nieuwe componenten lezen content via bv. `const h = t('hero'); … {h.title_1}`.

- [ ] **Stap 5: Herbouw `App` + `Header` + `Footer`/eind-CTA + chrome + pagina-stubs**

Vervang de bestaande `App`/componentboom. Gebruik dit skelet (port Header/Footer/back-to-top/mobiele balk uit de mockup-JSX volgens de conversieregels; pagina's voorlopig als stubs):

```jsx
function LanguageSwitcher({ lang, setLang }) {
  const [open, setOpen] = React.useState(false);
  const cur = LANGUAGES.find(l => l.code === lang) || LANGUAGES[0];
  return (
    <div className="lang-switcher" onMouseLeave={() => setOpen(false)}>
      <button className="lang-toggle" aria-haspopup="listbox" aria-expanded={open} onClick={() => setOpen(o => !o)}>
        <span>{cur.code.toUpperCase()}</span><span aria-hidden="true">⌄</span>
      </button>
      <div className={`lang-dropdown ${open ? 'open' : ''}`} role="listbox">
        {LANGUAGES.map(l => (
          <button key={l.code} className={`lang-option ${l.code === lang ? 'active' : ''}`} role="option"
            aria-selected={l.code === lang} onClick={() => { setLang(l.code); setOpen(false); }}>
            {l.label}<span className="lang-option-code">{l.code}</span>
          </button>
        ))}
      </div>
    </div>
  );
}

const PORTAAL_URL = 'https://vvebeheercollectief.twinq.nl/apex/f?p=TPL:101:::NO::TPL_APP:EPL';

function App() {
  const { lang, setLang, t, isRtl } = useLanguage();
  const [page, setPage] = React.useState('home');
  const [menuOpen, setMenuOpen] = React.useState(false);
  const [showTop, setShowTop] = React.useState(false);

  const go = (k) => { setPage(k); setMenuOpen(false); try { window.scrollTo(0, 0); } catch (e) {} };
  React.useEffect(() => {
    const onScroll = () => setShowTop((window.pageYOffset || 0) > 620);
    window.addEventListener('scroll', onScroll, { passive: true });
    return () => window.removeEventListener('scroll', onScroll);
  }, []);
  const scrollTop = () => { try { window.scrollTo({ top: 0, behavior: 'smooth' }); } catch (e) { window.scrollTo(0, 0); } };
  const nav = t('nav');
  const goers = { goHome:()=>go('home'), goDiensten:()=>go('diensten'), goPakketten:()=>go('pakketten'),
                  goWerkwijze:()=>go('werkwijze'), goOverons:()=>go('overons'), goOfferte:()=>go('offerte') };

  const pages = {
    home:      <HomePage t={t} {...goers} />,
    diensten:  <DienstenPage t={t} {...goers} />,
    pakketten: <PakkettenPage t={t} {...goers} />,
    werkwijze: <WerkwijzePage t={t} {...goers} />,
    overons:   <OverOnsPage t={t} {...goers} />,
    offerte:   <OffertePage t={t} />,
    privacy:   <LegalPage t={t} section="privacy_page" />,
    voorwaarden: <LegalPage t={t} section="voorwaarden_page" />,
    cookies:   <LegalPage t={t} section="cookies_page" />,
  };

  return (
    <div style={{ fontFamily: "'Karla',sans-serif", color: '#1C242E', background: '#FFFFFF' }}>
      <Header t={t} page={page} go={go} menuOpen={menuOpen} setMenuOpen={setMenuOpen}
              lang={lang} setLang={setLang} {...goers} />
      {pages[page] || pages.home}
      <Footer t={t} go={go} {...goers} />
      {showTop && (
        <div onClick={scrollTop} aria-label="Terug naar boven" role="button" tabIndex={0}
             style={{ position:'fixed', right:22, bottom:96, zIndex:70, width:46, height:46, borderRadius:'50%',
                      background:'#fff', border:'1px solid #DCE3E8', boxShadow:'0 14px 34px -14px rgba(28,36,46,.4)',
                      display:'flex', alignItems:'center', justifyContent:'center', cursor:'pointer', color:'#4E6885', fontSize:18 }}>↑</div>
      )}
      <MobileBar t={t} goOfferte={goers.goOfferte} />
    </div>
  );
}

// Stubs — worden in Taak 2–8 vervangen door de volledige pagina's:
function HomePage({ t }) { return <main className="container" style={{padding:'80px 24px'}}><h1>Home (stub)</h1></main>; }
function DienstenPage({ t }) { return <main className="container" style={{padding:'80px 24px'}}><h1>Diensten (stub)</h1></main>; }
function PakkettenPage({ t }) { return <main className="container" style={{padding:'80px 24px'}}><h1>Pakketten (stub)</h1></main>; }
function WerkwijzePage({ t }) { return <main className="container" style={{padding:'80px 24px'}}><h1>Werkwijze (stub)</h1></main>; }
function OverOnsPage({ t }) { return <main className="container" style={{padding:'80px 24px'}}><h1>Over ons (stub)</h1></main>; }
function OffertePage({ t }) { return <main className="container" style={{padding:'80px 24px'}}><h1>Offerte (stub)</h1></main>; }
function LegalPage({ t, section }) { const p = t(section); return <main className="container" style={{maxWidth:820, padding:'80px 24px'}}><h1 style={{fontFamily:"'Source Serif 4',serif"}}>{p.title}</h1></main>; }
```

Bouw `Header`, `Footer` (incl. het donkere eind-CTA-blok bóven de footer) en `MobileBar` volledig uit de mockup-JSX (regels 55–93 header, 658–714 footer/back-to-top/mbar) via de conversieregels. **Belangrijke bindingen:**
- Header: logo `assets/logo.png`; nav-items uit `t('nav')` met actief-kleur `#4E6885` / inactief `#59616B`; telefoon `tel:+31858000605`; "Offerte aanvragen"-knop (`className="h-btnP"`); **taalknop** `<LanguageSwitcher>` (desktop `data-r="dlang"`, en in het mobiele menu); hamburger met `aria-label`/`aria-expanded` op `menuOpen`.
- Footer "Service"-kolom: het item **"Klantenportaal"** wordt een `<a href={PORTAAL_URL} target="_blank" rel="noopener">`. Onderbalk-links "Privacy/Voorwaarden/Cookies" → `onClick={()=>go('privacy'|'voorwaarden'|'cookies')}`. E-mail → `mailto:info@vvebeheercollectief.nl`.

- [ ] **Stap 6: Verifieer het skelet in de browser**

Reload `http://localhost:8000/`. `read_console_messages` → **geen errors**. Controleer:
- Header + donkere footer in Richting D-stijl; `getComputedStyle(document.body).fontFamily` bevat `Karla`.
- Klikken op nav-items wisselt de stub-titels (Home → Diensten → …).
- Taalknop opent; kies "Deutsch" → chrome-labels wisselen waar vertaald, en vallen anders terug op NL (geen lege plekken); `document.documentElement.lang` wordt `de`.
- `resize_window {preset:'mobile'}` → hamburger + mobiele actiebalk verschijnen; menu opent/sluit.
- Footer "Klantenportaal" heeft `href` naar de Twinq-URL; Privacy/Voorwaarden/Cookies openen de (stub) legal-pagina.

`computer{screenshot}` van desktop + mobiel als bewijs.

- [ ] **Stap 7: Commit**

```bash
cd /Users/servicedesk/vve-website
git add index.html
git commit -m "feat(restyle): Richting D-fundament — fonts, stijl, content-object, shell + chrome

Nieuwe header/footer/eind-CTA, taalknop, mobiel menu, back-to-top; NL-content
uit de handoff; pagina's als stubs. Twinq-portaal + legal-routes bedraad.

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Taak 2: Home-pagina

**Files:** Modify `index.html` — vervang `HomePage`-stub door de volledige pagina.

- [ ] **Stap 1: Bouw `HomePage`** — port de home-secties uit de mockup (regels 95–186: hero, stats-strip, regio-strip, drie pijlers, waarom) + FAQ + nieuwsbrief (verderop in de mockup-body). Volg de conversieregels; lees teksten via `t()`:
  - Hero: gradient-section + `<img data-r="sky" src="assets/skyline.png" …>` met mask; eyebrow (streepje + `t('hero').eyebrow`); `<h1 data-r="h1">{h.title_1} <span style={{color:'#4E6885'}}>{h.title_2}</span></h1>`; subtitle; 2 knoppen (`h-btnP2` / `h-btnO`) → `goDiensten`/`goWerkwijze`.
  - Stats: `t('kpis').map(...)` in `data-reveal data-r="g4"`.
  - Regio: `t('regio')`.
  - Pijlers: intro uit `t('pijlers')`; kaarten uit `t('pijlers').items.map(...)` met `className="h-card"`; check-items met `✓`.
  - Waarom: sticky intro `t('waarom')`; rijen `t('waarom').rows.map((w,i)=> … nummer = String(i+1).padStart(2,'0'))` met `className="h-row"`.
  - Testimonials: `{ t('testimonials') && TESTIMONIALS_AAN && ( … )}` — **standaard uit** (zet een const `const SHOW_TESTIMONIALS = false;` bovenaan; toon de sectie niet tot er echte reviews zijn).
  - FAQ: accordeon met lokale state `const [openFaq,setOpenFaq]=React.useState(0)`; `t('faq').items.map((f,i)=> … open={openFaq===i} onClick={()=>setOpenFaq(openFaq===i?-1:i)})`; chevron `⌄` met `transform:rotate(${open?180:0}deg)`; één tegelijk open, eerste standaard open; gebruik `<button>` voor de klikbare rij met `aria-expanded`.
  - Nieuwsbrief: lokale state `const [nlDone,setNlDone]=React.useState(false)`; `<form onSubmit={e=>{e.preventDefault(); setNlDone(true);}}>` (gedrag ongewijzigd t.o.v. nu; geen mailinglijst-koppeling).
- [ ] **Stap 2: Verifieer** — reload, `page='home'`. Console schoon. `read_page`/screenshot: hero met skyline + serif H1; 4 KPI's; 3 pijlerkaarten; waarom-rijen; FAQ opent/sluit één item; nieuwsbrief toont bevestiging na submit. Testimonials-sectie is **afwezig**. Mobiel (`resize_window mobile`): hero-skyline zakt achter tekst, grids 1-koloms. Taalknop → Duits: onvertaalde teksten tonen NL.
- [ ] **Stap 3: Commit**

```bash
git add index.html && git commit -m "feat(restyle): home-pagina in Richting D

Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"
```

---

### Taak 3: Diensten-pagina

**Files:** Modify `index.html` — vervang `DienstenPage`-stub.

- [ ] **Stap 1: Bouw `DienstenPage`** — port de diensten-sectie uit de mockup (gecentreerde header op `#F5F7F9`; sticky sub-nav met `t('diensten_page')` `dienstIndex`-labels → `scrollToEl('dienst-1'|-2|-3)`; 3 pijlerblokken uit `dienstenPijlers` met "Wat wij voor u regelen" (`items`) en "Het resultaat" (`resultaat`), elk met `id`; donker Compleetpakket-blok (`linear-gradient(150deg,#26313E,#181F28)`, badge "Meest gekozen", 2-koloms `compleetPunten`); afsluitende offerte-CTA). Definieer `scrollToEl` lokaal:

```jsx
const scrollToEl = (id) => { const el = document.getElementById(id); if (el) { const y = el.getBoundingClientRect().top + (window.pageYOffset||0) - 130; window.scrollTo({ top:y, behavior:'smooth' }); } };
```

- [ ] **Stap 2: Verifieer** — `page='diensten'`. Console schoon. Sub-nav-klik scrolt naar de juiste pijler (~130px offset). 3 blokken tonen check-items + resultaat. Donker Compleet-blok met 8 punten. Mobiel: split stapelt. Screenshot.
- [ ] **Stap 3: Commit** — `git add index.html && git commit -m "feat(restyle): diensten-pagina in Richting D\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

### Taak 4: Pakketten-pagina (prijskaarten + vergelijkingstabel)

**Files:** Modify `index.html` — vervang `PakkettenPage`-stub.

- [ ] **Stap 1: Bouw `PakkettenPage`** — port uit de mockup:
  - Header op `#F5F7F9` (`t('pakketten_page')`).
  - 3 prijskaarten uit `packages` (regels 787–795): naam (serif 23), omschrijving (min-height 62), prijs (serif 36) + sub, CTA-knop (uitgelicht = gevuld `#4E6885` `h-btnP`, overige = outline), 8 feature-regels met `✓`/`—` (opgenomen `#4E6885`/`#3B4653`, niet-opgenomen grijs). **Uitgelicht** ("Start aanvullend"): staalblauwe rand + ring `0 0 0 2px #4E6885` + badge "Meest gekozen"; niet-ingezakt terwijl de andere `marginTop:16`.
  - Onder de kaarten: prijsnoot ("Tarieven excl. 21% btw…") + 3 chips (Vrijblijvend & gratis · Geen risico · Overstap in 6–8 weken).
  - Vergelijkingstabel op `#F5F7F9` (max 1000): witte kaart, 4-koloms grid (Functie / Start / Start aanv. / Compleet), kolomkop "Start aanv." licht gemarkeerd `#EEF3F7`. Rijen gegroepeerd per categorie uit `compare` (regels 745–782): render per `cat` een groepskop, dan `rows` met `func` + 3 cellen (`✓`/`—`/tekst zoals "1× p/j"). Wikkel de tabel in `data-r="scrollx"` met binnenblok `data-r="tablemin"` voor mobiel scrollen.
  - Afsluiten met keuzehulp (`keuzehulp`, regels 833–838) + offerte-CTA.
- [ ] **Stap 2: Verifieer** — `page='pakketten'`. Console schoon. 3 kaarten, middelste uitgelicht met ring + badge en niet ingezakt. Tabel toont categorieën + ✓/—; op mobiel horizontaal scrollbaar (min-width 680). Screenshot desktop + mobiel.
- [ ] **Stap 3: Commit** — `git add index.html && git commit -m "feat(restyle): pakketten-pagina + vergelijkingstabel in Richting D\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

### Taak 5: Werkwijze-pagina (tijdlijn)

**Files:** Modify `index.html` — vervang `WerkwijzePage`-stub.

- [ ] **Stap 1: Bouw `WerkwijzePage`** — header op `#F5F7F9`; verticale tijdlijn (max 760, `borderLeft:'2px solid #E4E9ED'`) uit `werkwijze` (regels 839–846): per stap genummerde cirkel 24×24 (`#4E6885`, witte cijfers `idx`, witte rand 4px), fase-label (`when`, Jost), H3 serif 23 (`title`), tekst (`text`). Daarna 3 geruststelling-kaarten uit `geruststelling` (847–851) op `#F5F7F9`. Afsluiten met donkere CTA (herbruik het eind-CTA-patroon).
- [ ] **Stap 2: Verifieer** — `page='werkwijze'`. Console schoon. 6 genummerde stappen met verbindingslijn; 3 kaarten. Mobiel oké. Screenshot.
- [ ] **Stap 3: Commit** — `git add index.html && git commit -m "feat(restyle): werkwijze-pagina (tijdlijn) in Richting D\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

### Taak 6: Over ons-pagina (met foto-placeholder)

**Files:** Modify `index.html` — vervang `OverOnsPage`-stub.

- [ ] **Stap 1: Bouw `OverOnsPage`** — split-header (1.05fr/0.95fr): links label/H1/tekst + 3 regio-bulletjes (`t('overons_page')`); rechts **foto-placeholder** = `<div>` met `background:'linear-gradient(160deg,#EEF2F5,#D6DEE6)'`, `aspectRatio:'1/1'`, `borderRadius:14`, en een subtiel label "Fotografie —" (geoorloofd; wordt vervangen door echte foto later). Daarna KPI-strip (`t('kpis')`) op `#F5F7F9`, missie-blok (label + serif statement 34 + tekst), donkere CTA-balk.
- [ ] **Stap 2: Verifieer** — `page='overons'`. Console schoon. Header met placeholder-vlak (geen gebroken `<img>`), KPI-strip, missie. Mobiel: split stapelt. Screenshot.
- [ ] **Stap 3: Commit** — `git add index.html && git commit -m "feat(restyle): over ons-pagina in Richting D (foto-placeholder)\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

### Taak 7: Offerte-pagina — wizard + `mailto:`-verzending

**Files:** Modify `index.html` — vervang `OffertePage`-stub. Dit is de enige **functionele toevoeging** bovenop de restyle.

- [ ] **Stap 1: Voeg de mailto-helper toe** (bovenaan het script, bij de helpers):

```jsx
function mailtoOfferte(f) {
  const L = [
    `Naam VvE: ${f.vveNaam || '-'}`,
    `Aantal appartementsrechten: ${f.aantal || '-'}`,
    `Bouwjaar: ${f.bouwjaar || '-'}`,
    `Adres / locatie: ${f.adres || '-'}`,
    `Gewenste diensten: ${(f.diensten && f.diensten.length ? f.diensten.join(', ') : '-')}`,
    `Huidige situatie: ${f.situatie || '-'}`,
    `Toelichting: ${f.toelichting || '-'}`,
    ``,
    `Naam: ${((f.voornaam || '') + ' ' + (f.achternaam || '')).trim() || '-'}`,
    `Functie: ${f.functie || '-'}`,
    `E-mail: ${f.email || '-'}`,
    `Telefoon: ${f.telefoon || '-'}`,
  ];
  const subject = `Offerteaanvraag${f.vveNaam ? ' — VvE ' + f.vveNaam : ''}`;
  return `mailto:info@vvebeheercollectief.nl?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(L.join('\n'))}`;
}
```

- [ ] **Stap 2: Bouw `OffertePage`** met **controlled inputs** en 4-staps-state:

```jsx
function OffertePage({ t }) {
  const op = t('offerte_page');
  const [step, setStep] = React.useState(1);
  const [done, setDone] = React.useState(false);
  const [f, setF] = React.useState({ vveNaam:'', aantal:'', bouwjaar:'', adres:'', diensten:[], situatie:'', toelichting:'', voornaam:'', achternaam:'', functie: (op.step4.functie_opts||[''])[0], email:'', telefoon:'' });
  const set = (k, v) => setF(p => ({ ...p, [k]: v }));
  const toggleDienst = (d) => setF(p => ({ ...p, diensten: p.diensten.includes(d) ? p.diensten.filter(x=>x!==d) : [...p.diensten, d] }));
  const submit = () => { window.location.href = mailtoOfferte(f); setDone(true); };
  // … render: links sticky intro (op.eyebrow/title/intro/benefits + telefoon);
  //   rechts witte kaart met voortgangsbalkjes (4), "op.stap_van X van 4", stap-titel;
  //   step 1: inputs vveNaam/aantal(number)/bouwjaar(number)/adres → value + onChange={e=>set(...)};
  //   step 2: op.step2.opts.map → keuzekaart met checkbox checked={f.diensten.includes(o.t)} onChange={()=>toggleDienst(o.t)};
  //   step 3: chips (situatie, single-select) + textarea (toelichting);
  //   step 4: voornaam/achternaam/functie(select van functie_opts)/email/telefoon;
  //   nav: "← Terug" (step>1), "Volgende stap →" (step<4), "Aanvraag versturen" (step===4, donker #1C242E) → submit().
  //   done → bevestigingsblok (op.success_title/desc).
}
```

Port de exacte markup/labels uit de mockup (offerte-sectie, regels ~600–654) + `NL.offerte_page`; behoud de donkere "Aanvraag versturen"-knop (`#1C242E`).

- [ ] **Stap 3: Verifieer** — `page='offerte'`. Console schoon. Loop door de 4 stappen (Volgende/Terug werken; voortgangsbalkjes vullen). Vul velden. Op stap 4, controleer de mailto **zonder echt te mailen**: lees in de console de opgebouwde link:

```js
// via javascript_tool, met testwaarden in de state, of inspecteer na klik window.location (die verandert naar mailto:)
```

Bevestig dat `mailtoOfferte({vveNaam:'Test', aantal:'24', email:'a@b.nl'})` een `mailto:info@vvebeheercollectief.nl?subject=…&body=…` met de velden oplevert (correct ge-encodeerd, `\n` als `%0A`). Na "Aanvraag versturen" verschijnt het bevestigingsblok. Screenshot van wizard + bevestiging.

- [ ] **Stap 4: Commit** — `git add index.html && git commit -m "feat(restyle): offerte-wizard met mailto-verzending\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

### Taak 8: Juridische pagina's (privacy/voorwaarden/cookies)

**Files:** Modify `index.html` — vervang `LegalPage`-stub door de volledige weergave.

- [ ] **Stap 1: Bouw `LegalPage`** — render `t(section)` (privacy_page/voorwaarden_page/cookies_page) in Richting D: gecentreerde kolom max 820, H1 serif, secties met H2 (serif) + tekst (Karla). Ondersteun beide vormen uit de bestaande data (losse velden zoals `intro`/`h2_*`/`*_text` bij cookies/privacy én `artikelen:[{h2,p}]` bij voorwaarden) — detecteer arrays en map ze. De inhoud komt 1-op-1 uit de behouden NL-secties (Taak 1, Stap 3); niets herschrijven.
- [ ] **Stap 2: Verifieer** — via footer-onderbalk klik Privacy → toont de privacyverklaring; idem Voorwaarden en Cookies. Console schoon. KvK 51721139 zichtbaar in footer. Screenshot van één legal-pagina.
- [ ] **Stap 3: Commit** — `git add index.html && git commit -m "feat(restyle): juridische pagina's (privacy/voorwaarden/cookies) in Richting D\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

### Taak 9: Integrale QA + politoer

**Files:** Modify `index.html` (alleen fixes die uit QA komen).

- [ ] **Stap 1: Doorloop alle acceptatiecriteria** (spec §12) in de browser:
  - Alle 6 pagina's + legal ogen als Richting D (leisteen/papier/staalblauw, Source Serif 4/Karla/Jost). Screenshot-set van alle pagina's.
  - **Twinq-portaal** werkt in header (indien aanwezig), footer Service-kolom → opent de Twinq-URL in nieuw tabblad.
  - **Taalknop**: wissel naar `de`, `en`, `ar`. Onvertaalde nieuwe teksten vallen terug op NL (geen lege plekken/`undefined`). Bij `ar` staat `document.documentElement.dir='rtl'`. `localStorage.lang` onthoudt de keuze na reload.
  - Privacy/Voorwaarden/Cookies bereikbaar en gevuld; KvK in footer.
  - Offerte: mailto correct samengesteld; bevestigingsscherm.
  - **Responsive ≤900px**: hamburger + mobiele actiebalk; grids stapelen; H1/H2 verkleinen; vergelijkingstabel scrollbaar. Test `resize_window {preset:'mobile'}` en `{preset:'tablet'}`.
  - **Toegankelijkheid**: `prefers-reduced-motion` → animaties uit (test via `resize_window {colorScheme}` n.v.t.; gebruik devtools-emulatie of `javascript_tool` om `matchMedia` te bevestigen dat `[data-reveal]` geen transform meer heeft); focus-ring zichtbaar bij tabben; hamburger/FAQ/taalknop hebben `aria-*`.
  - `read_console_messages` op elke pagina → **geen errors/warnings** die op gebroken code wijzen.
- [ ] **Stap 2: Fix bevindingen** — pas `index.html` aan waar QA iets vindt; herverifieer het betreffende punt.
- [ ] **Stap 3: Controleer dat er geen dode "oude" resten zijn** — geen verwijzingen meer naar `--teal-*`, DM Sans, of oude componentnamen die niet meer bestaan; `git diff main -- index.html` globaal doorlezen.
- [ ] **Stap 4: Commit** — `git add index.html && git commit -m "test(restyle): integrale QA-fixes (a11y, responsive, taal-terugval)\n\nCo-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>"`

---

## Uitrol (na afronding van alle taken — apart, met akkoord van de gebruiker)

1. Push de branch: `git push -u origin richting-d-restyle`.
2. Gebruiker bekijkt de preview (lokaal via de server, of een Pages-preview van de branch) en geeft **akkoord**.
3. Pas na akkoord: merge/fast-forward `richting-d-restyle` → `main` en push. GitHub Pages deployt automatisch live. `CNAME`/Pages-config blijft ongewijzigd.
4. Verifieer de live URL (github.io Pages-URL) na deploy.

> **Niet** kaal naar `main` pushen vóór akkoord — `main` = productie (auto-deploy).

---

## Self-Review (uitgevoerd bij het schrijven)

- **Spec-dekking:** elke spec-sectie heeft een taak — tokens/fonts (T1), header/footer/portaal/legal-wiring (T1/T8), 6 pagina's (T2–T7), meertaligheid + NL-terugval (T1 §Stap 4 + T9), offerte-mailto (T7), beeld/placeholder (T0/T6), a11y+responsive (T1-CSS + T9), uitrol (slot). ✓
- **Placeholders:** de `/* … */` in het `NL`-object verwijzen naar **exacte mockup-regels** met verbatim NL-copy — bewuste DRY-verwijzing naar de in-repo referentie, geen open TODO's. Alle nieuwe/tricky logica (mailto, LanguageSwitcher, App-router, offerte-state, scrollToEl, hover-CSS) staat volledig uitgeschreven.
- **Type/naam-consistentie:** `go`, `goHome…goOfferte`, `t(section).key`, `PORTAAL_URL`, `mailtoOfferte(f)`, `f.diensten`, `showTop`, `openFaq`, `page`-sleutels (`home/diensten/pakketten/werkwijze/overons/offerte/privacy/voorwaarden/cookies`) zijn consistent tussen `App` en de pagina-componenten. ✓
