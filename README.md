# Centro Digital Humanities — Università del Salento

Sito in italiano generato con **Jekyll**, con un layout personalizzato in
`_layouts/default.html`. I loghi originali in `public/logos/` sono usati
nell’intestazione e nel piè di pagina, senza modificarli.
Tipografia, palette avorio/giallo e layout responsive per tutte le pagine
sono definiti in `assets/main.scss`, senza font remoti o JavaScript.

## Pubblicazione su GitHub Pages

1. Integrare questi file nel branch `main`.
2. In **Settings → Pages → Build and deployment**, selezionare **Deploy from
   a branch**, branch **main**, cartella **/ (root)** e salvare. È necessario
   avere i permessi di amministrazione del repository.
3. Attendere il completamento del workflow **pages-build-deployment**.
   Il sito sarà disponibile su <https://centrodh-unisalento.github.io/>.

Non occorre un workflow personalizzato: GitHub Pages esegue Jekyll e installa
il tema indicato in `_config.yml`. Non aggiungere un file `.nojekyll`.
La configurazione effettiva di Pages va verificata nelle impostazioni del
repository; la sola presenza di un workflow non garantisce la pubblicazione.
Se Pages è configurato per **GitHub Actions**, passare alla pubblicazione da
branch descritta sopra per usare questa configurazione.

`url` e `baseurl` in `_config.yml` corrispondono al sito alla radice del dominio.
Aggiornarli se cambia il percorso di pubblicazione o viene impostato un dominio
personalizzato. I percorsi degli asset usano il filtro Jekyll `relative_url`.

## Pagine e contenuti

La navigazione comprende Il Centro (sezione della homepage), Persone e ricerca
(`/persone/`) ed Eventi (`/eventi/`). Il vecchio indirizzo `/ricerca/` reindirizza
alla pagina unificata tramite il plugin GitHub Pages `jekyll-redirect-from`. Homepage e menu
rimandano alle pagine interne, senza collegamenti a GitHub Projects.

`eventi.html` contiene uno stato vuoto esplicito: non sono stati forniti eventi
da pubblicare. Per aggiungerli, sostituire il blocco `.empty-state` con i dettagli
degli appuntamenti, includendo titolo, data, sede e informazioni di partecipazione.
`persone.html` presenta la direzione e i sei consiglieri in schede su due colonne,
con una preview incorniciata delle ultime cinque pubblicazioni e il collegamento
a IRIS sotto ogni nome. Su schermi piccoli le schede passano a una colonna.
Nella sezione «Eventi e news» della homepage, il titolo «Gli appuntamenti del
centro» affianca la scheda «Eventi news», che rimanda alla pagina degli eventi;
su mobile il titolo precede la scheda. La homepage non contiene la grafica
decorativa iniziale né la barra degli ambiti.

Sotto il testo introduttivo della homepage compare «Notizie dal Centro»,
seguita da un reel fotografico con `aiucd2022.jpeg` e poi `edefai.jpg`,
con didascalie e testi alternativi. Foto e didascalie rimandano ai rispettivi
siti: AIUCD 2022 e «Edefai - Logic for the AI spring 2026». Le foto
rimangono intere, senza ritagli o testo sovrapposto. Il reel usa lo scorrimento
orizzontale nativo (touch, trackpad o frecce della tastiera dopo averlo
selezionato con Tab), senza JavaScript né avanzamento automatico.

La homepage mantiene uno spazio autonomo per la descrizione del Centro
(`#centro`) e uno per news e articoli in stile blog (`#news`), distinto
dal rimando agli eventi. La sezione news mostra i tre articoli più recenti,
con data, anteprima e collegamento al testo completo; finché non ci sono
articoli, mostra un messaggio esplicito senza notizie dimostrative.

Per pubblicare una notizia, creare un file Markdown in
`_posts/AAAA-MM-GG-titolo.md` con questa intestazione, seguita dal testo:

```yaml
---
layout: post
title: "Titolo della notizia"
date: 2026-11-02
---
```

Il primo paragrafo diventa l'anteprima nella homepage. Il layout
`_layouts/post.html` mostra titolo, data e testo completo, con un collegamento
di ritorno alle news. Le notizie vengono pubblicate insieme al sito da `main`.

## Contatti e mailing list

Il piè di pagina condiviso (`_includes/footer.html`) presenta il contatto
`centrodh@unisalento.it` e il pulsante per richiedere l'iscrizione alla mailing
list Google Groups tramite `ml_centrodh+subscribe@unisalento.it`.
Il pulsante apre il programma di posta: l'utente deve inviare la richiesta
e seguire le istruzioni ricevute; non iscrive automaticamente al clic.

Il gruppo deve consentire le richieste di adesione e, per gli indirizzi
esterni all'Ateneo, i membri esterni secondo le politiche Google Workspace.
L'approvazione dipende dalle impostazioni dei gestori. Verificare la procedura
da un indirizzo non iscritto: il sito non configura né verifica il gruppo.

## Pubblicazioni IRIS

Il componente riutilizzabile `_includes/pubblicazioni-iris.html` riceve
`person=person` e genera l’anteprima e il collegamento sotto il nome in `persone.html`, per
direttore, vicedirettore e sei componenti del Consiglio Direttivo.
`_data/direttivo.yml` contiene nomi, ruoli, collegamenti istituzionali e
identificativi autore IRIS forniti dal Centro, conservati senza modifiche.

Ogni collegamento apre la ricerca autore IRIS con
`sort_by=2&order=DESC`, 20 risultati per pagina e la paginazione del catalogo.
`_data/pubblicazioni.json` contiene una fotografia delle prime cinque voci
pubbliche di ciascun autore, recuperate dalle pagine IRIS ordinate per data di
pubblicazione (`sort_by=2&order=DESC&rpp=5&offset=0`). Si conservano l’ordine
del catalogo, i titoli, l’anno o la dicitura «In corso di stampa» e i collegamenti
permanenti `/handle/…`. Non si deducono titoli o date dalle colonne ISI/Scopus
del CSV, che possono essere incomplete. Ogni titolo apre la propria scheda
IRIS in una nuova scheda; il collegamento all’elenco completo resta visibile.

Le anteprime sono generate da Jekyll, senza JavaScript, iframe o richieste
dal browser a IRIS. Restano leggibili anche se il catalogo è temporaneamente
irraggiungibile. Non sono aggiornate automaticamente: la data della fotografia
è mostrata nella pagina Persone e ricerca.

Per aggiornare le anteprime, consultare per ciascun `iris_authority` le prime
cinque voci della ricerca pubblica ordinata per data e aggiornare la relativa
lista in `authors` di `_data/pubblicazioni.json`. Ogni voce deve contenere
`title`, `date_label` e `url` (scheda permanente HTTPS su IRIS). Conservare
l’ordine di IRIS, incluse le opere in corso di stampa; aggiornare `updated_at`
nel formato `AAAA-MM-GG` solo dopo aver verificato tutti gli autori. Pubblicare
i dati insieme alle pagine sul branch `main`. Per persone senza dati viene
mostrato un avviso esplicito con il collegamento al catalogo.

I metadati della fotografia iniziale sono stati recuperati da un runner GitHub
senza credenziali IRIS. Non vengono copiati abstract, testi integrali o allegati.
La disponibilità pubblica dei metadati non attesta una licenza di riuso dei
documenti collegati, che rimangono su IRIS.

Per aggiornare la composizione del direttivo, modificare `_data/direttivo.yml`;
ogni nuova persona deve avere il proprio identificativo `iris_authority`,
ricavato dal parametro `authority` della ricerca IRIS, non dal nome.

### Prova manuale dell'esportazione

Il workflow `.github/workflows/test-iris-export.yml` verifica l'accesso pubblico
al CSV da un runner GitHub, senza credenziali IRIS. Non ha una pianificazione,
non scrive nel repository e non pubblica o modifica le pagine del sito.

In **Actions → Test IRIS export → Run workflow**, selezionare l'identificativo
autore (predefinito: `rp22433`, Paolo Baldi) e avviare la prova. Gli altri codici
sono in `_data/direttivo.yml`. L'esecuzione controlla la struttura del CSV e la
presenza di record, segnala se esistono colonne per titolo e data di pubblicazione
e, solo dopo la validazione, allega il file e un rapporto come artifact
`iris-export-<identificativo>`, conservato per sette giorni.

La richiesta usa `rpp=20`: la prova non garantisce che siano esportati tutti
i risultati, che il BibTeX sia disponibile o che sia consentito ripubblicare
i metadati. Errori di rete, risposte HTTP di errore, pagine HTML al posto del CSV
ed esportazioni vuote fanno fallire il workflow; i dettagli sono nei log.

## Sviluppo locale

Con Ruby 3.x e Bundler installati (le dipendenze `github-pages` non supportano
Ruby 4):

```sh
bundle install
bundle exec jekyll serve
```

Aprire <http://localhost:4000/>. Per verificare la generazione senza avviare
il server:

```sh
bundle exec jekyll build --strict_front_matter
```

Il `Gemfile` usa l'insieme di dipendenze `github-pages` per riprodurre il servizio
di pubblicazione. `_site/` e le cache locali sono esclusi da Git.
