# Sito di Annalisa Bignami

Sito vetrina per i corsi di Acrobatica Aerea (tessuti, cerchio, amaca) e
preparazione fisica a Milano, in Valtellina e in Lombardia.

## Com'è fatto

Un unico file, [`index.html`](index.html): HTML, CSS e JavaScript stanno tutti
dentro, insieme alle icone e alle illustrazioni disegnate in SVG. Nessuna
libreria esterna, nessun font da scaricare, niente da installare o compilare.

Per lavorarci basta aprire `index.html` in un browser. Per vederlo dal telefono
sulla stessa rete di casa, da questa cartella:

```
python -m http.server 8777
```

e poi aprire `http://<indirizzo-del-pc>:8777` dal telefono.

## L'immagine dell'anteprima

Quando il link viene condiviso in chat, WhatsApp e i social mostrano un riquadro con
un'immagine: sono `og-cover.png` (italiano) e `og-cover-en.png` (inglese). Il testo è
stampato dentro l'immagine, quindi quando cambiano i corsi o le zone va rifatta anche
quella. I sorgenti stanno in [`card-social/`](card-social/): si aprono in un browser
con la finestra a 1200×630 e si salva lo screenshot sul PNG corrispondente.

Le chat tengono in cache le anteprime: un link già mandato continuerà a mostrare
l'immagine vecchia per qualche giorno.

## Le versioni precedenti

La cartella [`versioni/`](versioni/) tiene le tappe del lavoro, una per
cambiamento importante, con data e ora nel nome. Servono per confrontare o
tornare indietro: il sito vero è sempre e solo `index.html`.

## Perché index.html

GitHub Pages pubblica automaticamente il file chiamato `index.html`. È il motivo
per cui il file si chiama così e non `sito-annalisa.html` come in origine.
