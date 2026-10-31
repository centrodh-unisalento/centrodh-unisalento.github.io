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

La navigazione comprende Il Centro (sezione della homepage), Persone
(`/persone/`), Eventi (`/eventi/`) e Ricerca (`/ricerca/`). Homepage e menu
rimandano alle pagine interne, senza collegamenti a GitHub Projects.

`eventi.html` contiene uno stato vuoto esplicito: non sono stati forniti eventi
da pubblicare. Per aggiungerli, sostituire il blocco `.empty-state` con i dettagli
degli appuntamenti, includendo titolo, data, sede e informazioni di partecipazione.
`ricerca.html` presenta le prospettive di ricerca già descritte dal Centro e le
pubblicazioni del direttivo su IRIS. La homepage non contiene la
grafica decorativa iniziale né la barra degli ambiti.

## Pubblicazioni IRIS

Il componente riutilizzabile `_includes/pubblicazioni-iris.html`, inserito in
`ricerca.html`, mostra otto schede espandibili: direttore, vicedirettore e sei
componenti del Consiglio Direttivo. `_data/direttivo.yml` contiene nomi, ruoli,
collegamenti istituzionali e identificativi autore IRIS forniti dal Centro;
anche `persone.html` usa questo elenco per mantenere le due pagine coerenti.

Ogni scheda incorpora la ricerca autore IRIS in un iframe con
`sort_by=2&order=DESC`, 20 risultati per pagina e la paginazione del catalogo.
I dati vengono caricati direttamente da IRIS a ogni apertura della pagina:
non sono copiati nel repository e non servono JavaScript, plugin Jekyll
personalizzati o workflow programmati. Una pagina lasciata aperta non viene
ricaricata periodicamente; per aggiornare i risultati ricaricare la pagina.
Le finestre usano il caricamento differito e non inviano il referrer.

La visualizzazione incorporata dipende dalle politiche HTTP di IRIS
(`X-Frame-Options` e CSP `frame-ancestors`): se l'archivio la vieta, il browser
blocca la finestra. Il collegamento “Apri su IRIS” resta sempre disponibile,
ma non sostituisce la visualizzazione delle pubblicazioni nella pagina.
L'accesso a IRIS non è stato possibile dall'ambiente di sviluppo: verificare
le finestre dal sito pubblicato prima di considerare l'integrazione operativa.
Se IRIS blocca gli iframe, occorre un'integrazione autorizzata per recuperare
e pubblicare i dati, anziché tentare di aggirare le sue politiche.

Per aggiornare la composizione del direttivo, modificare `_data/direttivo.yml`;
ogni nuova persona deve avere il proprio identificativo `iris_authority`,
ricavato dal parametro `authority` della ricerca IRIS, non dal nome.

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
