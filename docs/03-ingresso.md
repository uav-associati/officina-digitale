# Ingresso di una persona

Da eseguire nell'ordine. G e' il primo giorno di lavoro della persona. Ogni passo dice chi lo esegue, entro quando e come si verifica che sia stato fatto davvero: non basta averlo fatto, deve risultare. Nelle ricerche del registro attivita', `utente` sta per il nome utente GitHub della persona.

1. **Comunicare l'ingresso**: data del primo giorno, ruolo e lavoro che fara', cosi' si sa in quali team entra.
   - Chi: l'Amministrazione (Roberto), alla Direzione (uomoaltovalore) e alla Responsabile tecnica (Marta).
   - Entro: G-5, cinque giorni lavorativi prima.
   - Verifica: esiste una segnalazione di ingresso con data, ruolo e team previsti. I passi successivi ci annotano il loro esito.

2. **Account GitHub con l'email di lavoro.** Se la persona ha gia' un account, usa quello e ci aggiunge l'email di lavoro.
   - Chi: la persona.
   - Entro: G-3.
   - Verifica: la persona scrive il suo nome utente nella segnalazione di ingresso e la Direzione ne apre il profilo.

3. **Verifica in due passaggi**, con i codici di recupero salvati in un posto che non sia il computer di lavoro.
   - Chi: la persona.
   - Entro: G-3.
   - Verifica: l'organizzazione la richiede, quindi senza non si puo' accettare l'invito. Dopo l'ingresso, nella pagina People compare il segno 2FA accanto al suo nome.

4. **Invito all'organizzazione, direttamente nel team.** Ruolo **Member**, mai Owner. Si aggiunge la persona al team del suo lavoro: GitHub la invita all'organizzazione e al team insieme. Nessun accesso diretto ai repository.
   - Chi: la Direzione (uomoaltovalore), che e' Owner.
   - Entro: G-2. L'invito scade dopo 7 giorni: se non viene accettato in tempo, va rifatto.
   - Verifica: nel registro `action:org.invite_member` riporta l'invito; in People, Invitations, la persona risulta con ruolo Member.

5. **Accettazione e team giusti.**
   - Chi: la persona accetta l'invito; la Responsabile tecnica (Marta) controlla i team.
   - Entro: G-1.
   - Verifica: in People la persona e' Member. Nella pagina dei collaboratori di ogni repository (Settings, Collaborators and teams) arriva solo tramite un team, mai con un accesso diretto. Nel registro `action:org.add_member action:team.add_member` riporta l'ingresso.

6. **Organigramma e responsabilita'.** Una riga nuova in `TEAM.md`, con ruolo e cosa serve davvero; se la persona risponde di una zona, anche `.github/CODEOWNERS`.
   - Chi: la Responsabile tecnica (Marta) scrive la proposta; la approva il Fondatore (Giovanni), che risponde dei documenti aziendali.
   - Entro: G.
   - Verifica: la proposta che aggiorna `TEAM.md`, ed eventualmente `CODEOWNERS`, e' unita.

7. **Letture**: `docs/01-come-lavoriamo.md`, questo documento e `docs/05-credenziale-esposta.md`.
   - Chi: la persona.
   - Entro: G.
   - Verifica: la prima proposta, al passo 8, segue il processo di `docs/01`: nasce da una segnalazione, sta in un branch, compila il modello della proposta.

8. **Prima attivita'**: aprire una segnalazione e chiuderla con una proposta di modifica, anche minima. Serve a verificare che gli accessi funzionino davvero, non che sembrino funzionare.
   - Chi: la persona; la proposta la approva un collega della stessa zona.
   - Entro: G+2.
   - Verifica: la proposta e' unita con l'approvazione di un collega; nel registro `actor:utente action:pull_request.create` la mostra.

## Cosa non si fa

- Non si da' accesso "per sicurezza, cosi' non ci blocchiamo"
- Non si concede Owner a chi deve solo lavorare
- Non si da' accesso diretto a un repository, nemmeno convertendo la persona in collaboratore esterno
- Non si aspetta il primo giorno per preparare gli accessi, e non si preparano settimane prima: l'invito scade dopo 7 giorni

## Il controllo finale

Verifica il risultato, non i passi. Entro G+2 la persona prova a scrivere direttamente su `main` e viene respinta: nel registro compare `actor:utente action:protected_branch.rejected_ref_update`. Poi apre una proposta che un collega approva e che viene unita. Se succedono tutte e due le cose, ha i permessi giusti: puo' lavorare, e non puo' saltare le regole.
