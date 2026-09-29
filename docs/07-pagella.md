# Pagella della sicurezza dell'organizzazione

Una fotografia di come e' messa l'organizzazione `uav-associati` su dodici voci di sicurezza: per ognuna l'esito, il valore trovato e dove e' stato verificato. Serve a sapere da dove partire e, rifatta a ogni revisione degli accessi, a vedere se si migliora.

- Quando: 29/9/2026 alle 19:15 (GMT+8). Le tre voci che si potevano sistemare in pochi minuti, senza cambiare piano, sono state sistemate la sera stessa (vedi «Sistemato subito»).
- Come: solo letture, dalle impostazioni del sito di GitHub e con l'API (`gh api`). Durante il controllo nessuna impostazione e' stata cambiata.
- Cosa: i due repository dell'organizzazione, `officina-digitale` (pubblico) e `uav-oversell` (privato), piu' i repository degli account personali dei membri, per quanto si riescono a vedere.
- Criterio: una voce passa se rispetta la regola di `docs/02-policy-accessi.md`, quando la regola c'e'. Le voci che riguardano i repository passano solo se passano su tutti e due.

## Il punteggio: 6 su 12

Al controllo delle 19:15 era 3 su 12. Dopo le correzioni della sera passano la verifica in due passaggi, il permesso di base, gli accessi solo dai team (voci 5 e 6), i collaboratori esterni e l'approvazione dei token (voce 12). Restano sei voci. Due chiedono una seconda persona e un lavoro di trasferimento; quattro sono protezioni del repository privato che il piano gratuito non applica.

## Le dodici voci

| # | Voce | Esito | Valore trovato | Dove e' verificato |
|---|---|---|---|---|
| 1 | Nessun repository aziendale su account personali | Non passa | 8 repository privati di lavoro sull'account personale `uomoaltovalore`. Sull'account `emiliosalerno` nessuno, ma di un altro account si vedono solo i repository pubblici | `gh repo list uomoaltovalore`; API `users/emiliosalerno/repos` |
| 2 | Numero di Owner | Non passa | 1, `uomoaltovalore`. La policy ne chiede almeno due | People, filtro Owner; API `orgs/uav-associati/members?role=admin` |
| 3 | Permesso di base | Passa | Nessuno (`none`) | Settings, Member privileges; API `orgs/uav-associati`, campo `default_repository_permission`; in ogni repository «Base role: None» |
| 4 | Verifica in due passaggi obbligatoria | Passa | Obbligatoria per tutti, e solo con i metodi sicuri. Nessun membro ne e' senza | Settings, Authentication security; API `orgs/uav-associati`, campo `two_factor_requirement_enabled`; API `members?filter=2fa_disabled`, vuoto |
| 5 | Accessi da team e da persone, per repository | Passa, sistemata | Al controllo: `officina-digitale` 4 da team e 1 da una persona, `uav-oversell` 0 e 1. Dopo la correzione: `officina-digitale` 4 da team e nessuna persona; `uav-oversell` nessun accesso, solo gli Owner | Settings del repository, Collaborators and teams; API `repos/.../teams` e `repos/.../collaborators?affiliation=direct` |
| 6 | Accessi diretti a persone | Passa, sistemata | Al controllo: `uomoaltovalore`, con permesso write, su tutti e due i repository, senza motivazione scritta. Dopo la correzione: nessuno | Come la voce 5; API `user/repos?affiliation=collaborator` |
| 7 | Collaboratori esterni, con la data di ingresso | Passa | Nessuno, e nessun invito in sospeso | People, Outside collaborators: «No one outside of the organization has access to its repositories»; API `orgs/uav-associati/outside_collaborators` e `repos/.../invitations` |
| 8 | Proposta obbligatoria su `main`, per repository | Non passa | `officina-digitale`: si', con il ruleset `protezione-main`, applicato. `uav-oversell`: ruleset configurato ma non applicato, perche' il piano gratuito non lo applica ai repository privati | Settings del repository, Rulesets, con l'avviso «won't be enforced»; API `repos/.../branches/main`, campo `protected`: vero per `officina-digitale`, falso per `uav-oversell` |
| 9 | Approvazioni richieste, per repository | Non passa | `officina-digitale`: 1, di un responsabile della parte toccata, annullata da ogni nuovo commit. `uav-oversell`: 1 configurata, nessuna applicata | Come la voce 8; API `repos/.../rules/branches/main`, che per `uav-oversell` risponde 403 |
| 10 | Controlli obbligatori, per repository | Non passa | `officina-digitale`: 1, «Test del calcolo fatture». `uav-oversell`: nessuno, e nel repository non c'e' nessun test automatico | Come la voce 9; API `repos/.../actions/workflows` |
| 11 | Blocco delle credenziali nel push | Non passa | `officina-digitale`: attivo, insieme alla ricerca dei segreti. `uav-oversell`: non disponibile, perche' serve GitHub Secret Protection, che si compra solo con Team o Enterprise. Per i repository nuovi e' spento | API `repos/...`, campo `security_and_analysis`; Settings, Advanced Security di `uav-oversell`, dove l'opzione non compare |
| 12 | Token che accedono all'organizzazione solo con approvazione | Passa, sistemata | Al controllo: token fine-grained bloccati, app OAuth solo se approvate, ma token classic ammessi senza approvazione e senza scadenza obbligatoria. Dopo la correzione: bloccati anche i token classic | Settings, Personal access tokens, schede Fine-grained tokens e Tokens (classic); Settings, Third-party application access policy |

## Le voci non superate, dalla piu' rischiosa

1. **Voce 2: un solo Owner.**
   - Il rischio: se l'account `uomoaltovalore` si perde, per esempio con il telefono della verifica in due passaggi e senza i codici di recupero, nessuno puo' piu' governare l'organizzazione. Se lo prende qualcun altro, la governa lui. Dallo stesso account dipendono anche i repository della voce 1.
   - Come si sistema: si nomina un secondo Owner. Deve essere un'altra persona, perche' un secondo account della stessa persona non riduce il rischio.
   - Tempo: un quarto d'ora, fra invito, verifica in due passaggi, nomina e controllo. Prima pero' serve la persona.
   - Piano: nessun cambio.
2. **Voce 1: otto repository di lavoro su un account personale.**
   - Il rischio: fuori dall'organizzazione non valgono le sue regole: team, permesso di base, regole sui token, registro attivita'. E se l'account personale ha un problema, se li porta dietro.
   - Come si sistema: prima si decide quali sono davvero aziendali, poi si trasferiscono nell'organizzazione (Settings del repository, Danger Zone, Transfer), uno alla volta. Ogni trasferimento va preparato: le app collegate all'account personale vanno ricollegate all'organizzazione, e vanno aggiornati i collegamenti che usano il vecchio indirizzo.
   - Tempo: mezz'ora per repository, mezza giornata in tutto.
   - Piano: nessun cambio su GitHub. Per il repository pubblicato con Vercel, prima va verificato che il piano di Vercel accetti un repository di un'organizzazione.
3. **Voce 11: nessun blocco delle credenziali su `uav-oversell`.**
   - Il rischio: una chiave scritta per sbaglio nel codice della piattaforma di vendita non viene fermata ne' segnalata, e resta nella storia. Il codice lo scrive un agente AI, e la piattaforma usa chiavi di servizi esterni.
   - Come si sistema: GitHub Team (4 $ a persona al mese) piu' GitHub Secret Protection (19 $ al mese per ogni persona che carica codice). Senza cambiare piano si puo' aggiungere uno scanner di segreti come controllo automatico su ogni proposta: segnala la chiave, ma non blocca il push.
   - Tempo: 10 minuti per attivarla dopo l'abbonamento; 1-2 ore per lo scanner gratuito.
   - Piano: si', per il blocco vero.
4. **Voce 8: su `uav-oversell` la proposta obbligatoria non e' applicata.**
   - Il rischio: chi ha la scrittura, compresa l'app dell'agente AI, puo' scrivere direttamente su `main` o riscriverne la storia. Oggi lo impedisce solo una regola di lavoro.
   - Come si sistema: si passa a GitHub Team, e il ruleset `protezione-main`, gia' pronto, si applica da solo. Prima pero' va creato l'account GitHub dell'agente AI. Le sue proposte oggi risultano aperte da `uomoaltovalore`, e chi apre una proposta non puo' approvarla: con l'approvazione obbligatoria non si potrebbero unire.
   - Tempo: un quarto d'ora per l'account; il ruleset non richiede lavoro.
   - Piano: si', GitHub Team.
5. **Voce 9: su `uav-oversell` l'approvazione non e' applicata.**
   - Stessa causa e stessa soluzione della voce 8: l'approvazione (1) e' gia' configurata, manca il piano che la applica.
   - Tempo e piano: come la voce 8.
6. **Voce 10: nessun controllo obbligatorio su `uav-oversell`.**
   - Il rischio: nel repository non c'e' un test automatico, quindi si puo' unire codice che non si compila.
   - Come si sistema: prima un workflow che esegue i controlli che il progetto ha gia' (formato e tipi, descritti nel suo README), poi il nome di quel controllo nel ruleset.
   - Tempo: un'ora.
   - Piano: il controllo gira anche sul piano gratuito (2.000 minuti al mese per i repository privati); diventa obbligatorio solo con Team.

## Sistemato subito

La sera del 29/9/2026, dopo il controllo, con la verifica del risultato:

- **Voce 12, token classic.** Settings, Personal access tokens, Tokens (classic): «Restrict access via personal access tokens (classic)». Adesso nessun token personale, ne' classic ne' fine-grained, entra nell'organizzazione. Le integrazioni che conosciamo non usano token classic. Verifica: ricaricata la pagina, l'opzione risulta salvata.
- **Voci 5 e 6, accessi diretti.** Tolto l'accesso diretto di `uomoaltovalore` da tutti e due i repository. E' Owner, quindi i suoi permessi non cambiano: e' cambiato solo da dove arrivano. Verifica: l'API non elenca piu' accessi diretti, e il suo permesso effettivo resta admin. Sul sito `officina-digitale` mostra solo 4 team, `uav-oversell` «Only Owners can contribute to this repository».
- **Fuori dalle dodici voci, Dependabot su `uav-oversell`.** Erano spenti il grafo delle dipendenze e gli avvisi, perche' il repository e' arrivato per trasferimento e non ha preso l'impostazione dei repository nuovi. Ora sono attivi. Verifica: l'API risponde 204, e sul sito i due pulsanti sono diventati «Disable».

## Da sapere per il controllo mensile

Dal 29/9/2026 nell'organizzazione e' installata un'app GitHub, quella dell'agente AI che lavora sulla piattaforma di vendita, con accesso al solo `uav-oversell`. Il prossimo controllo mensile la trovera' nella ricerca 5 di `docs/06-controllo-mensile.md`: e' voluta. Nella ricerca 2 comparira' la creazione del ruleset `protezione-main` su `uav-oversell`.

## Quando si rifa'

Due volte l'anno, insieme alla revisione degli accessi di marzo e settembre (`docs/02-policy-accessi.md`). Prossima: lunedi' 1 marzo 2027.
