# Centro Digital Humanities — Università del Salento

Homepage in italiano basata su **Minima**, tema Jekyll supportato da GitHub
Pages. I loghi originali del repository sono in `public/logos/`; gli accenti
gialli e gli adattamenti responsive sono in `assets/main.scss`.

## Pubblicazione su GitHub Pages

1. Integrare questi file nel branch `main`.
2. In **Settings → Pages → Build and deployment**, selezionare **Deploy from
   a branch**, branch **main**, cartella **/ (root)** e salvare. È necessario
   avere i permessi di amministrazione del repository.
3. Attendere il completamento del workflow **pages-build-deployment**.
   Il sito sarà disponibile su <https://centrodh-unisalento.github.io/home/>.

Non occorre un workflow personalizzato: GitHub Pages esegue Jekyll e installa
il tema indicato in `_config.yml`. Non aggiungere un file `.nojekyll`.
La configurazione effettiva di Pages va verificata nelle impostazioni del
repository; la sola presenza di un workflow non garantisce la pubblicazione.
Se Pages è configurato per **GitHub Actions**, passare alla pubblicazione da
branch descritta sopra per usare questa configurazione.

`url` e `baseurl` in `_config.yml` corrispondono al sito di progetto `/home`.
Aggiornarli se cambia il percorso di pubblicazione o viene impostato un dominio
personalizzato. I percorsi degli asset usano il filtro Jekyll `relative_url`.
Il collegamento ai progetti è configurato in `projects_url` e punta alla pagina
GitHub Projects richiesta; l'accesso ai contenuti dipende dai permessi su GitHub.

## Sviluppo locale

Con Ruby e Bundler installati:

```sh
bundle install
bundle exec jekyll serve
```

Aprire <http://localhost:4000/home/>. Per verificare la generazione senza avviare
il server:

```sh
bundle exec jekyll build --strict_front_matter
```

Il `Gemfile` usa l'insieme di dipendenze `github-pages` per riprodurre il servizio
di pubblicazione. `_site/` e le cache locali sono esclusi da Git.
