# Outstream Simple - Standalone VAST Player

**outstream-simple.html** je kompletný standalone HTML súbor pre outstream VAST reklamy s plnou funkcionalitou.

## Prehľad

Tento súbor obsahuje kompletnú implementáciu outstream VAST playera s nasledujúcimi funkciami:

- ✅ **Standalone** - Všetky CSS a JavaScript sú vložené priamo do HTML, žiadne externé závislosti (okrem IMA SDK)
- ✅ **CSS izolácia** - Všetky CSS pravidlá sú izolované do `.player-container`, neovplyvňujú zvyšok stránky
- ✅ **Klikateľný freeze frame** - Po skončení reklamy je posledný frame klikateľný (ak existuje click-through URL)
- ✅ **Automatické získanie click-through URL** - Získava click-through URL z IMA SDK a automaticky nastaví klikateľnosť
- ✅ **Mobile podpora** - Plne funkčné na mobilných zariadeniach
- ✅ **VAST URL s description_url** - Podporuje `[url]` placeholder v VAST URL pre automatické nastavenie description_url
- ✅ **Automatické získanie URL** - Automaticky získava URL stránky (aj z iframe) a vloží ju do VAST URL

## Rýchly štart

1. Otvorte `outstream-simple.html` v prehliadači alebo vložte do vašej stránky
2. Upravte `VAST_URL` v JavaScripte (riadok ~324) s vašim VAST endpointom
3. Ak používate `description_url`, použite `[url]` placeholder - automaticky sa nahradí aktuálnou URL stránky

## HTML štruktúra

```html
<div class="player-container" id="player-container-2">
    <div class="ad-label">REKLAMA</div>
    <div id="ad-bg-2" class="ad-bg"></div>
    <div id="freeze-overlay-2" class="freeze-overlay"></div>
    <video id="outstream-ad-2" class="op-player__media" controls playsinline muted>
    </video>
    <div id="ad-countdown-2" class="ad-countdown"></div>
    <span id="elapsed-time-2" class="elapsed-time">0:00</span>
</div>
```

## Konfigurácia VAST URL

### Základný VAST URL

```javascript
const VAST_URL = 'https://your-vast-server.com/vast.xml';
```

### VAST URL s description_url (automatické nahradenie [url])

```javascript
const VAST_URL = 'https://pubads.g.doubleclick.net/gampad/ads?iu=/your-ad-unit&description_url=[url]&output=vast';
```

**Poznámka:** `[url]` placeholder sa automaticky nahradí aktuálnou URL stránky. Ak sme v iframe, použije sa URL rodičovského okna (ak je dostupná, inak URL iframe).

## Hlavné funkcie

### 1. CSS izolácia

Všetky CSS pravidlá sú izolované do `.player-container`, takže neovplyvňujú zvyšok stránky:

```css
/* Všetky naše CSS pravidlá začínajú s .player-container */
.player-container .ad-label { ... }
.player-container .ad-bg { ... }
.player-container .freeze-overlay { ... }
.player-container .ad-countdown { ... }
.player-container .elapsed-time { ... }
```

**Výhody:**
- Žiadne konflikty so štýlmi stránky
- Bezpečné vloženie do akejkoľvek stránky
- Žiadne globálne CSS pravidlá

### 2. Klikateľný freeze frame

Po skončení reklamy sa automaticky:

1. **Získa click-through URL** z IMA SDK cez `currentAd.getClickThroughUrl()`
2. **Nastaví freeze frame video ako klikateľné** (ak existuje click-through URL)
3. **Pridá click handler**, ktorý otvorí URL v novom okne s `noopener` a `noreferrer`

**Funkcionalita:**
- Automatické získanie click-through URL v `adsstart` evente
- Nastavenie `pointer-events: auto` a `cursor: pointer` pre freeze frame
- Správne čistenie event handlerov pri novom načítaní reklamy
- Bezpečné otvorenie URL v novom okne

### 3. Automatické získanie URL pre description_url

Ak používate `[url]` placeholder v VAST URL:

1. **Získa URL aktuálnej stránky** - ak sme v iframe, skúsi získať URL rodičovského okna
2. **Fallback na URL iframe** - ak nie je možné získať URL rodičovského okna (cross-origin), použije URL iframe
3. **URL encode** - URL sa správne encode-uje a vloží do VAST URL

**Implementácia:**
```javascript
function getPageUrl() {
    try {
        // Skúsime získať URL rodičovského okna (ak sme v iframe)
        if (window.parent && window.parent.location.href !== window.location.href) {
            return window.parent.location.href;
        }
    } catch (e) {
        // Cross-origin iframe - použijeme URL iframe
    }
    return window.location.href;
}
```

### 4. Mobile podpora

- Responzívny dizajn
- Správne zobrazenie na mobilných zariadeniach
- Klikateľný freeze frame funguje aj na mobile

## Použitie na stránke

### Možnosť 1: Priamy vloženie

Vložte celý obsah `outstream-simple.html` do vašej stránky alebo použite ako externý súbor:

```html
<!-- Vložte celý obsah outstream-simple.html -->
```

### Možnosť 2: Iframe

```html
<iframe 
    src="outstream-simple.html" 
    width="580" 
    height="400" 
    frameborder="0"
    allowfullscreen>
</iframe>
```

**Poznámka:** Pri použití v iframe sa automaticky použije URL rodičovského okna pre `description_url`.

## Testovanie

### Automatizované testy

```bash
node examples/test-outstream.js
```

Testuje:
- CSS izoláciu
- JavaScript selektory
- Klikateľnosť freeze frame
- Bezpečnosť
- HTML štruktúru

### Interaktívne testy

Otvorte `examples/outstream-simple.test.html` v prehliadači a použite tlačidlá na spustenie testov.

## Technické detaily

### CSS izolácia

- Všetky CSS pravidlá začínajú s `.player-container`
- Žiadne globálne `body` alebo `html` styly
- Všetky selektory sú špecifické pre naše elementy

### JavaScript selektory

- Všetky `querySelector` a `getElementById` používajú špecifické ID
- ID obsahujú čísla (napr. `player-container-2`, `outstream-ad-2`)
- Žiadne globálne selektory, ktoré by mohli ovplyvniť iné elementy

### Event handlery

- Správne čistenie event handlerov pri novom načítaní reklamy
- Ukladanie handlerov do state pre možné odstránenie
- Resetovanie click-through URL pri novom načítaní

### Bezpečnosť

- `window.open` s `noopener` a `noreferrer`
- `preventDefault()` a `stopPropagation()` v click handleroch
- Správne error handling pre cross-origin iframe

## Štruktúra kódu

```
outstream-simple.html
├── <head>
│   ├── <style> - OpenPlayer CSS (vložený)
│   └── <style> - Naše CSS (izolované)
├── <body>
│   └── <div class="player-container">
│       ├── <div class="ad-label">
│       ├── <div class="ad-bg">
│       ├── <div class="freeze-overlay">
│       ├── <video>
│       ├── <div class="ad-countdown">
│       └── <span class="elapsed-time">
└── <script>
    ├── OpenPlayer JS (vložený)
    └── Naša implementácia
        ├── loadIMASDK()
        ├── initializePlayer()
        ├── getPageUrl() - získanie URL pre description_url
        └── Event handlery
```

## Poznámky

- IMA SDK sa načítava automaticky z Google CDN
- Všetky funkcie sú v jednom súbore - žiadne externé závislosti
- Kód je pripravený na produkčné použitie
- Testované a overené automatizovanými testami

## Podpora

Pre otázky a problémy navštívte:
- [OpenPlayer dokumentácia](../docs/)
- [OpenPlayer web](https://www.openplayerjs.com)
