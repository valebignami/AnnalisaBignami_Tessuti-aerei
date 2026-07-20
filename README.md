# Sito di Annalisa Bignami

Sito vetrina per i corsi di discipline aeree (tessuti, cerchio, amaca) e
preparazione fisica a Milano, Morbegno e in Lombardia.

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

## Le versioni precedenti

La cartella [`versioni/`](versioni/) tiene le tappe del lavoro, una per
cambiamento importante, con data e ora nel nome. Servono per confrontare o
tornare indietro: il sito vero è sempre e solo `index.html`.

## Perché index.html

GitHub Pages pubblica automaticamente il file chiamato `index.html`. È il motivo
per cui il file si chiama così e non `sito-annalisa.html` come in origine.
