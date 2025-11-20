# OpenPlayer Examples

Tento adresár obsahuje príklady použitia OpenPlayer.

## Outstream VAST Player

Príklady pre použitie OpenPlayer ako outstream player pre VAST reklamy.

### Súbory

- **outstream-simple.html** - Standalone outstream player s vloženým CSS a JS
- **outstream-player.html** - Kompletný príklad s dokumentáciou
- **outstream-player.js** - JavaScript implementácia
- **outstream-wrapper.js** - Wrapper trieda pre jednoduchšie použitie
- **outstream-wrapper-example.html** - Príklad použitia wrapper triedy
- **README-outstream.md** - Detailná dokumentácia
- **test-outstream.js** - Automatizované testy
- **outstream-simple.test.html** - Interaktívne testy v prehliadači
- **TEST-README.md** - Dokumentácia testov

## outstream-simple.html

### Prehľad

**outstream-simple.html** je kompletný standalone HTML súbor pre outstream VAST reklamy s nasledujúcimi funkciami:

- ✅ **Standalone** - Všetky CSS a JavaScript sú vložené priamo do HTML
- ✅ **CSS izolácia** - Všetky CSS pravidlá sú izolované do `.player-container`, neovplyvňujú zvyšok stránky
- ✅ **Klikateľný freeze frame** - Po skončení reklamy je posledný frame klikateľný (ak existuje click-through URL)
- ✅ **Automatické získanie click-through URL** - Získava click-through URL z IMA SDK a automaticky nastaví klikateľnosť
- ✅ **Mobile podpora** - Plne funkčné na mobilných zariadeniach
- ✅ **VAST URL s description_url** - Podporuje `[url]` placeholder v VAST URL pre automatické nastavenie description_url

### Rýchly štart

1. Otvorte `outstream-simple.html` v prehliadači alebo vložte do vašej stránky
2. Upravte `VAST_URL` v JavaScripte (riadok ~324) s vašim VAST endpointom
3. Ak používate `description_url`, použite `[url]` placeholder - automaticky sa nahradí aktuálnou URL stránky

### HTML štruktúra

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

### Konfigurácia VAST URL

```javascript
// Základný VAST URL
const VAST_URL = 'https://your-vast-server.com/vast.xml';

// VAST URL s description_url (automatické nahradenie [url])
const VAST_URL = 'https://pubads.g.doubleclick.net/gampad/ads?iu=/your-ad-unit&description_url=[url]&output=vast';
```

**Poznámka:** `[url]` placeholder sa automaticky nahradí aktuálnou URL stránky (ak sme v iframe, použije sa URL rodičovského okna).

### Hlavné funkcie

#### 1. CSS izolácia

Všetky CSS pravidlá sú izolované do `.player-container`, takže neovplyvňujú zvyšok stránky:

```css
/* Všetky naše CSS pravidlá začínajú s .player-container */
.player-container .ad-label { ... }
.player-container .ad-bg { ... }
.player-container .freeze-overlay { ... }
```

#### 2. Klikateľný freeze frame

Po skončení reklamy sa automaticky:
- Získa click-through URL z IMA SDK
- Nastaví freeze frame video ako klikateľné (ak existuje click-through URL)
- Pridá click handler, ktorý otvorí URL v novom okne

#### 3. Automatické získanie URL pre description_url

Ak používate `[url]` placeholder v VAST URL:
- Automaticky sa získa URL aktuálnej stránky
- Ak sme v iframe, použije sa URL rodičovského okna
- URL sa správne encode-uje a vloží do VAST URL

### Použitie na stránke

#### Možnosť 1: Priamy vloženie

```html
<!-- Vložte celý obsah outstream-simple.html do vašej stránky -->
```

#### Možnosť 2: Iframe

```html
<iframe src="outstream-simple.html" width="580" height="400" frameborder="0"></iframe>
```

### Testovanie

```bash
# Automatizované testy
node examples/test-outstream.js

# Interaktívne testy
# Otvorte examples/outstream-simple.test.html v prehliadači
```

### Ďalšie príklady

- **outstream-player.html** - Kompletný príklad s dokumentáciou
- **outstream-wrapper.js** - Wrapper trieda pre jednoduchšie použitie

## Ďalšie príklady

Pre ďalšie príklady a dokumentáciu navštívte:
- [OpenPlayer dokumentácia](../docs/)
- [OpenPlayer web](https://www.openplayerjs.com)

