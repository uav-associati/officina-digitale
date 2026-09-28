# Controllo mensile

Una volta al mese si cercano nel registro attivita' dell'organizzazione cinque tipi di evento: quelli che cambiano chi comanda, cosa e' protetto, cosa vede ogni membro, cosa e' pubblico e chi entra dall'esterno. GitHub li registra ma non li segnala a nessuno: se nessuno li cerca, passano inosservati.

- Dove: https://github.com/organizations/uav-associati/settings/audit-log (Settings, Archive, Logs, Audit log). Il registro lo vedono solo gli Owner.
- Quando: il primo giorno lavorativo di ogni mese, sul mese appena chiuso.
- Chi: la Direzione (uomoaltovalore), che e' Owner, esegue le ricerche; la Consulente sicurezza (Nina) rilegge i risultati.
- Tempo: mezz'ora.

## Come si cerca

Si incolla l'espressione nella casella di ricerca del registro. Tre regole, provate sul registro vero il 29/9/2026:

- piu' `action:` nella stessa ricerca si sommano: basta che l'evento sia uno di quelli;
- `-action:` esclude un evento;
- `created:2026-09-01..2026-09-30` limita la ricerca a un mese. Nel controllo mensile si aggiunge a ogni espressione, con le date del mese appena chiuso.

La ricerca non filtra sui valori scritti dentro l'evento, come il ruolo nuovo o la visibilita' nuova: quelli si leggono aprendo ogni risultato. E zero risultati non dimostra che la ricerca sia giusta: un nome di evento sbagliato da' zero risultati senza nessun errore. I nomi qui sotto vengono dall'elenco ufficiale degli eventi di GitHub.

## Le cinque ricerche

1. **Qualcuno e' diventato Owner.**
   - Espressione: `action:org.update_member action:org.add_member`
   - Cosa significa: ogni cambio di ruolo nell'organizzazione, da membro a Owner o viceversa, e ogni persona entrata. E' diventato Owner chi nell'evento ha il permesso `admin`: "with admin permission" per chi entra, il passaggio ad `admin` per chi cambia ruolo.
   - Se non lo riconosci: chiedi subito a chi compare come autore dell'evento e alla persona interessata se era voluto. Se non lo era, riporta la persona a Member e controlla che gli Owner siano solo quelli di `docs/02-policy-accessi.md`. Poi cerca cosa ha fatto da Owner, con `actor:` seguito dal suo nome utente, dalla data dell'evento in poi. Se nemmeno l'autore riconosce l'azione, il suo account e' compromesso: lo si toglie dall'organizzazione finche' non ha cambiato password e revocato sessioni e token, e si apre una bozza di security advisory come in `docs/05-credenziale-esposta.md`.

2. **Un ruleset o una protezione di branch e' stato creato, modificato o cancellato.**
   - Espressione: `action:repository_ruleset action:protected_branch -action:protected_branch.rejected_ref_update -action:protected_branch.policy_override`
   - Cosa significa: creazione, modifica e cancellazione dei ruleset e delle protezioni dei branch, su tutti i repository. Restano fuori i push respinti (`rejected_ref_update`, cioe' la protezione che ha funzionato) e gli scavalcamenti (`policy_override`), che non sono modifiche alle regole. Gli scavalcamenti vanno guardati a parte, con `action:protected_branch.policy_override`: ogni risultato e' qualcuno che ha scritto su un branch protetto passando sopra alle regole.
   - Se non lo riconosci: apri l'evento, che dice quale regola e cosa e' cambiato, e rimetti la regola com'era (Settings del repository, Rules, Rulesets). Poi controlla cosa e' entrato nel branch mentre la regola era diversa: i commit e le proposte unite in quel periodo. Chiedi all'autore perche'; se non riconosce l'azione, vale quanto detto al punto 1 per un account compromesso.

3. **Il permesso di base dell'organizzazione e' cambiato.**
   - Espressione: `action:org.update_default_repository_permission`
   - Cosa significa: ogni cambio del permesso che ogni membro riceve su ogni repository per il solo fatto di essere membro. L'evento dice da quale valore a quale, per esempio "from read to write".
   - Se non lo riconosci: rimettilo al valore scritto in `docs/02-policy-accessi.md` (Settings, Member privileges, Base permissions). Se era salito, per esempio a write, controlla i commit e le proposte di tutti i repository nel periodo in cui e' rimasto alto: il registro non elenca i singoli push.

4. **Un repository e' passato da privato a pubblico.**
   - Espressione: `action:repo.access`
   - Cosa significa: ogni cambio di visibilita' di un repository, in tutte e due le direzioni. Da privato a pubblico e' l'evento con `previous_visibility` private e `visibility` public. Non vede i repository creati gia' pubblici: quelli si trovano con `action:repo.create`.
   - Se non lo riconosci: rimettilo privato subito (Settings del repository, Danger Zone, Change visibility). Poi consideralo esposto per tutto il tempo in cui e' stato pubblico: chiunque puo' averlo copiato, e i fork pubblici fatti in quel periodo restano pubblici anche dopo. Controllane tutta la storia come prima di una pubblicazione (segreti, file di configurazione, email, messaggi dei commit) e per ogni segreto trovato segui `docs/05-credenziale-esposta.md`.

5. **Un'applicazione e' stata installata o le sono stati concessi permessi.**
   - Espressione: `action:integration_installation.create action:integration_installation.version_updated action:integration_installation.repositories_added action:org.oauth_app_access_approved`
   - Cosa significa: un'app GitHub installata nell'organizzazione, un'app che ha ricevuto permessi nuovi, un'app a cui sono stati aggiunti repository, un'app OAuth autorizzata ad accedere all'organizzazione. I token personali non sono app: per quelli c'e' `action:personal_access_token.access_granted`.
   - Se non lo riconosci: togli l'accesso all'app (Settings, GitHub Apps per le app GitHub; Settings, OAuth app policy per le app OAuth). Poi guarda nell'evento a quali repository arrivava e con quali permessi: tutto quello che poteva leggere va considerato letto, e i segreti che poteva vedere si trattano come esposti (`docs/05-credenziale-esposta.md`). Chiedi a chi l'ha installata perche'.

Il controllo e' fatto quando: le cinque ricerche sono state eseguite sul mese appena chiuso, ogni risultato ha una spiegazione, e una segnalazione del mese riporta il numero di risultati di ciascuna. Nella segnalazione vanno solo numeri e spiegazioni: un risultato non riconosciuto si scrive in una bozza di security advisory, non in una segnalazione pubblica.

## Il punto di partenza, 29/9/2026

Risultati delle cinque ricerche su tutto il registro, dalla nascita dell'organizzazione il 24/9. Il primo controllo mensile si confronta con questa tabella: ogni riga nuova si deve saper spiegare.

| Ricerca | Risultati | Cosa sono |
|---|---|---|
| 1. Owner | 3 | uomoaltovalore entra come Owner alla creazione dell'organizzazione; emiliosalerno entra due volte come membro, con permesso read |
| 2. Ruleset e protezioni | 1 | creazione del ruleset `protezione-main` su officina-digitale |
| 3. Permesso di base | 2 | 24/9 alle 21:23 (GMT+8) da read a write, alle 21:36 da write a none, entrambi da uomoaltovalore |
| 4. Da privato a pubblico | 0 | la visibilita' di un repository non e' mai cambiata |
| 5. App installate o con permessi nuovi | 0 | nell'organizzazione non e' installata nessuna app |
