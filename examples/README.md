# OpenPlayer Examples

Tento adresár obsahuje príklady použitia OpenPlayer.

## Outstream VAST Player

Príklady pre použitie OpenPlayer ako outstream player pre VAST reklamy.

### Súbory

- **outstream-simple.html** - Najjednoduchší príklad outstream player
- **outstream-player.html** - Kompletný príklad s dokumentáciou
- **outstream-player.js** - JavaScript implementácia
- **outstream-wrapper.js** - Wrapper trieda pre jednoduchšie použitie
- **outstream-wrapper-example.html** - Príklad použitia wrapper triedy
- **README-outstream.md** - Detailná dokumentácia

### Rýchly štart

1. Otvorte `outstream-simple.html` v prehliadači
2. Nahraďte VAST_URL v JavaScripte vašim vlastným VAST endpointom
3. Kliknite na Play pre spustenie reklamy

### Základné použitie

```html
<video id="outstream-ad" class="op-player__media" controls playsinline></video>

<script src="path/to/openplayer.min.js"></script>
<script>
const player = new OpenPlayerJS('outstream-ad', {
    ads: {
        src: 'https://your-vast-url.com/vast.xml',
        autoPlayAdBreaks: false,
    },
});
player.init();
</script>
```

### Použitie wrapper triedy

```html
<div id="ad-container"></div>

<script src="path/to/openplayer.min.js"></script>
<script src="outstream-wrapper.js"></script>
<script>
const outstream = new OutstreamPlayer({
    container: '#ad-container',
    vastUrl: 'https://your-vast-url.com/vast.xml',
    autoplay: false
});
outstream.init();
</script>
```

## Ďalšie príklady

Pre ďalšie príklady a dokumentáciu navštívte:
- [OpenPlayer dokumentácia](../docs/)
- [OpenPlayer web](https://www.openplayerjs.com)

