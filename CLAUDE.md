# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Cos'è

Sito vetrina statico di Annalisa Bignami (corsi di Acrobatica Aerea). Niente build,
niente dipendenze, niente test: HTML, CSS e JS stanno dentro i file `.html`.

La disciplina si chiama **Acrobatica Aerea**, sempre con le maiuscole anche dentro
le frasi, in inglese **Aerial Acrobatics**: è il nome usato sui volantini e va tenuto
identico ovunque. Le zone sono Milano, la Valtellina e la Lombardia (non "Morbegno",
sostituito ovunque).

## Comandi

```
python -m http.server 8777
```

Poi `http://localhost:8777` (o `http://<ip-del-pc>:8777` dal telefono sulla stessa
rete). Per una modifica veloce basta aprire `index.html` nel browser.

La pubblicazione è GitHub Pages: `git push` su `main` mette online
`https://valebignami.github.io/AnnalisaBignami_Tessuti-aerei/`. Il file si chiama
`index.html` proprio perché Pages pubblica quello.

## Le due lingue

`index.html` (IT) e `en.html` (EN) sono **due copie complete e indipendenti** dello
stesso sito: stesso CSS, stesso JS, stessi `id` di sezione (`discipline`, `benefici`,
`chi-sono`, `dicono`, `domande`, `palestre`, `eventi`, `contatti`).

Ogni modifica va portata su entrambi i file, non solo su `index.html`. Chi ha
lavorato prima lo segnala nei messaggi di commit ("...IT+EN"). Cambiando struttura,
attenzione anche a:

- `<html lang>`, `og:locale`, i `<link rel="alternate" hreflang>` (che si citano a
  vicenda e vanno tenuti allineati);
- `.lang-toggle`: la voce della pagina corrente ha `class="on"` e `aria-current="page"`;
- `og:image` (`og-cover.png` / `og-cover-en.png`) e gli URL assoluti, che contengono
  il percorso GitHub Pages.

## Com'è fatto il file

Dentro `index.html` (le righe valgono anche, spostate, per `en.html`):

- righe ~33–1329: `<style>`, diviso da commenti `/* ---- nome ---- */`
  (tipografia, barra, bottoni, hero, sezioni, chi sono, domande, contatti,
  collage, dicono di me);
- righe ~1534–2296: il markup, `header.topbar` + `main` con una `<section>` per blocco;
- righe ~2298–2407: `<script>`, quattro blocchi senza librerie.

Le righe lunghissime (decine di migliaia di caratteri) sono foto in `data:` base64:
non leggerle, usa `awk 'length($0)<400'` o `grep -n` per muoverti nel file.

### Sistema di colore

Tutto parte dalle variabili in `:root`:

- colori dei teli: `--mandarino`, `--albicocca`, `--lampone`, `--acqua`;
- tinte di fondo: `--panna`, `--pesca`, `--sole`, `--menta`, `--rosa`, applicate alle
  sezioni con le classi `.tint-*`. Regola: **mai due tinte uguali di fila**. Ogni
  `.tint-*` è una sfumatura che scivola verso la tinta della sezione seguente e
  definisce `--tint-strong` (la fascia sottile in cima alla sezione) e `--bolla`;
- testo: `--fg`, `--fg-mid`, `--fg-soft`. I commenti accanto registrano il rapporto
  di contrasto: sono stati scuriti apposta per restare sopra 4.5:1 sui pastelli.
  Non schiarirli senza rifare il conto.

Il tema scuro è **volutamente neutralizzato**: sia `@media (prefers-color-scheme: dark)`
sia `:root[data-theme="dark"]` riportano una palette chiara e `color-scheme: light`.

Ordine degli strati, dichiarato nel commento "sezioni": sfondo → bolle (0) → teli (5)
→ `.wrap` contenuto (6) → intestazione (30) → barra di avanzamento (40).

### Le card dell'anteprima social

`immagini/og-cover.png` e `immagini/og-cover-en.png` sono le immagini che compaiono
quando il link viene condiviso in chat. Sono PNG da 1200×630 con **il testo stampato dentro**:
cambiare i meta non le tocca. Vanno rigenerate ogni volta che cambia il nome della
disciplina, l'elenco dei corsi o le zone.

Il sorgente è in `card-social/`: `og-cover.html` e `og-cover-en.html`, una per
lingua, autoportanti come il resto del sito. Si modificano, si aprono in un browser
con la finestra a 1200×630 e si salva lo screenshot sul PNG corrispondente in
`immagini/`. Le misure devono restare 1200×630, perché sono dichiarate in
`og:image:width` e `og:image:height`. Le due card si rifanno sempre insieme.

Attenzione: WhatsApp e Facebook tengono in cache le anteprime, quindi un link già
condiviso continua a mostrare la card vecchia per giorni.

### Immagini

Tutti i file immagine stanno in **`immagini/`**, mai nella cartella radice: le foto
del collage (`foto-*.jpg`) e le due card social. Se generi immagini di lavoro o
screenshot, tienili fuori dal repository — nella cartella temporanea di sessione.

Due meccanismi convivono: le foto dentro hero, schede e ritratto sono **inline in
base64** (il file resta autoportante); il collage della gallery e poche altre
puntano ai file in `immagini/`. Icone, logo e favicon sono SVG scritti a mano nel
markup.

### JavaScript

Quattro blocchi, tutti in italiano e tutti protetti da
`matchMedia('(prefers-reduced-motion: reduce)')` dove animano:

1. `.reveal` → comparsa a scorrimento via `IntersectionObserver`;
2. `#progress` → filo di avanzamento in cima;
3. voce di menu attiva + contatori `.fig b[data-to]`;
4. ricentraggio della striscia del menu sul telefono.

Nel blocco 4 c'è un avvertimento importante e va rispettato: **non usare
`scrollIntoView()`** sui link del menu. Agisce su tutti i contenitori scorrevoli
sopra l'elemento, documento compreso, e blocca lo scorrimento della pagina. Si usa
`scrollLeft` / `scrollTo` sull'elemento `.nav`.

## Convenzioni

- **Tutto in italiano**: commenti, messaggi di commit (imperativo: "Aggiunge...",
  "Corregge...", "Toglie..."), e anche i nomi delle variabili JS (`barraMenu`,
  `riportaInVista`, `animaCifra`, `voci`, `bersagli`).
- I commenti spiegano **perché**, non cosa, e spesso raccontano un tentativo fallito
  o una scelta di accessibilità. Sono memoria del progetto: aggiornali, non
  cancellarli quando tocchi il codice intorno.

## Cartelle

- `immagini/` — tutte le immagini usate dal sito: le foto del collage e le due card
  social. Le immagini nuove vanno qui, non nella cartella radice.
- `card-social/` — i due sorgenti HTML delle immagini di anteprima (vedi sopra).
- `versioni/` — istantanee del sito, una per cambiamento importante, nominate
  `sito-AAAAMMGG-HHMM-descrizione.html`. Servono solo a confrontare o tornare
  indietro; il sito vero è `index.html`. L'ultima è del 20/07/2026: la pratica non è
  proseguita nei commit successivi.
- `CV/`, `volantini/`, `foto-aeree/`, `qr/`, `Gif/`, `scegli-foto.html` — materiale di
  lavoro non pubblicato (volantini, curriculum, archivio foto con `CREDITI.txt`, e uno
  strumento locale per scegliere le foto). Non versionati.
- `.gitignore` esclude di proposito `*.pdf`, `*.docx`, `*.pptx`, `*.gif`, `*.mp4`: il
  repository è pubblico e quei file sarebbero scaricabili da chiunque. Non aggiungerli.
