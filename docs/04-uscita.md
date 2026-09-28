# Uscita di una persona

G e' l'ultimo giorno di lavoro della persona. Se l'uscita e' improvvisa, G e' oggi: i passi "prima dell'uscita" si fanno subito, nell'ordine, senza aspettare. Nelle ricerche del registro attivita', `utente` sta per il nome utente GitHub della persona.

Togliere la persona dall'organizzazione non basta, e farlo per primo e' un errore: dopo la rimozione le sue approvazioni smettono di contare e le zone di cui rispondeva restano senza nessuno. Per questo prima si mette al sicuro il lavoro, poi si chiudono gli accessi, poi si controlla quello che GitHub non vede.

## Prima dell'uscita

1. **Comunicare l'uscita**: data e ora dell'ultimo giorno.
   - Chi: l'Amministrazione (Roberto), alla Direzione (uomoaltovalore) e alla Responsabile tecnica (Marta).
   - Entro: appena la data e' decisa; per un'uscita improvvisa, subito.
   - Verifica: esiste una segnalazione di uscita con data e ora. I passi successivi ci annotano il loro esito.

2. **Non deve essere l'unico Owner.**
   - Chi: la Direzione (uomoaltovalore).
   - Entro: G-3.
   - Verifica: in People, filtrando per ruolo Owner, restano almeno due Owner senza di lui, come chiede `docs/02-policy-accessi.md`.

3. **Sostituirlo dove risponde di qualcosa**: nelle zone di `.github/CODEOWNERS`, nei team di cui e' maintainer, fra le persone che possono approvare le proposte. Le regole su `main` chiedono l'approvazione di una persona diversa dall'autore, e chi resta deve bastare a darla.
   - Chi: la Responsabile tecnica (Marta); le sostituzioni le decide il Fondatore (Giovanni).
   - Entro: G-3.
   - Verifica: per ogni zona di `CODEOWNERS` c'e' almeno un'altra persona con permesso di scrittura, e aprendo il file su GitHub non compaiono errori. Una proposta di prova aperta da chi resta viene approvata da un altro che resta.

4. **Riassegnare il lavoro in corso**: le sue proposte aperte, le revisioni chieste a lui, le segnalazioni assegnate a lui.
   - Chi: la Responsabile tecnica (Marta).
   - Entro: G-1.
   - Verifica: nelle proposte e nelle segnalazioni, le ricerche `is:open author:utente`, `is:open review-requested:utente` e `is:open assignee:utente` non danno risultati, oppure ogni risultato ha un nuovo responsabile.

5. **Trasferire i repository aziendali che ha sul proprio account.** Solo il proprietario puo' trasferire un repository: dopo l'uscita non si fa piu'.
   - Chi: la persona, con la Direzione che accoglie il trasferimento nell'organizzazione.
   - Entro: G-1.
   - Verifica: il repository compare fra quelli dell'organizzazione; nel registro `action:repo.transfer` lo riporta.

## Il giorno dell'uscita

6. **Toglierlo dai team e dall'organizzazione.** Dal sito: People, il suo nome, Remove from organization. Mai Convert to outside collaborator, che gli lascia un accesso diretto ai repository.
   - Chi: la Direzione (uomoaltovalore), che e' Owner.
   - Entro: G, a fine lavoro.
   - Verifica: la rimozione richiede qualche minuto. Poi in People non compare piu', e nel registro `action:org.remove_member` la riporta con il suo nome.

7. **Nessun accesso diretto rimasto.** L'accesso diretto sopravvive alla rimozione dai team.
   - Chi: la Direzione (uomoaltovalore).
   - Entro: G.
   - Verifica: non compare in People, Outside collaborators, ne' in Settings, Collaborators and teams, di nessun repository.

8. **Token fine-grained.** L'organizzazione non puo' revocare i token di una persona, che stanno sul suo account: puo' togliere ai suoi token fine-grained l'accesso all'organizzazione. I token classici agiscono con i suoi permessi, e dopo l'uscita non ne ha piu'.
   - Chi: la Direzione (uomoaltovalore).
   - Entro: G.
   - Verifica: in Settings, Personal access tokens, Active tokens, non resta nessun token di cui e' proprietario; nel registro `action:personal_access_token.access_revoked` riporta ogni revoca.

9. **Deploy key dei repository.** Una deploy key appartiene al repository, non alla persona, e resta valida anche dopo l'uscita di chi l'ha installata.
   - Chi: la Responsabile tecnica (Marta).
   - Entro: G.
   - Verifica: nel registro `actor:utente action:public_key.create` elenca le chiavi che ha aggiunto; nessuna di quelle compare ancora in Settings, Deploy keys, del suo repository.

10. **Segreti delle Actions.** Chi ha permesso di scrittura su un repository puo' leggerne i segreti con un workflow: vanno cambiati tutti, in ogni repository dove aveva scrittura.
    - Chi: la Responsabile tecnica (Marta), con chi gestisce ciascun servizio. La chiave nuova si crea come in `docs/05-credenziale-esposta.md`, passo 2.
    - Entro: G+1.
    - Verifica: in Settings, Secrets and variables, Actions, la data di aggiornamento di ogni segreto e' successiva all'uscita; nel registro `action:repo.update_actions_secret` riporta ogni cambio.

11. **App e webhook che ha aggiunto.** Restano all'organizzazione anche dopo la sua uscita.
    - Chi: la Direzione (uomoaltovalore).
    - Entro: G+1.
    - Verifica: nel registro `actor:utente action:integration_installation.create action:hook.create` elenca cosa ha aggiunto; per ogni risultato la segnalazione di uscita dice se resta e chi ne risponde d'ora in poi.

12. **Registro degli ultimi trenta giorni.**
    - Chi: la Consulente sicurezza (Nina), con la Direzione che apre il registro.
    - Entro: G+1.
    - Verifica: la segnalazione di uscita riporta l'esito della ricerca `actor:utente`, limitata agli ultimi trenta giorni con `created:`, e una spiegazione per ogni evento insolito.

## Quello che GitHub non puo' verificare

GitHub vede solo quello che succede su GitHub. Questi accessi restano aperti anche dopo i passi precedenti, e nessun registro di GitHub li mostra.

13. **Chiavi SSH installate sui server.** Una chiave copiata sui server aziendali, per esempio l'hosting, continua ad aprirli qualunque cosa succeda su GitHub.
    - Chi: chi amministra ciascun server, con la Responsabile tecnica (Marta), che tiene l'elenco delle chiavi di ogni persona.
    - Entro: G.
    - Verifica: l'elenco delle chiavi autorizzate di ogni server (il file `authorized_keys`, o su Hostinger la pagina SSH Access) non contiene piu' la sua.

14. **Segreti condivisi**: password di servizi e chiavi in uso comune che conosceva, come pannelli dei fornitori, pagamenti, posta, il gestore di password dell'azienda.
    - Chi: chi gestisce ciascun servizio; l'elenco dei segreti lo tiene l'Amministrazione (Roberto).
    - Entro: G+1.
    - Verifica: per ogni segreto dell'elenco, la data di modifica nel servizio o nel gestore di password e' successiva all'uscita.

15. **Servizi esterni collegati**: i suoi account aziendali fuori da GitHub, come la casella di posta e i pannelli di hosting e pagamenti; gli accessi fatti con "Accedi con GitHub" o "Accedi con Google"; le integrazioni collegate al suo account personale.
    - Chi: l'Amministrazione (Roberto), servizio per servizio.
    - Entro: G.
    - Verifica: nell'elenco utenti di ogni servizio il suo account e' disattivato o tolto, e ogni integrazione che passava da lui e' collegata a un account aziendale e funziona.

16. **Copie gia' fatte.** Cloni e file scaricati non si ritirano: per questo i segreti si cambiano, ai passi 10 e 14. I repository privati che ha sul proprio account li vede solo lui.
    - Chi: la persona, che lo dichiara; la Direzione (uomoaltovalore) raccoglie la dichiarazione.
    - Entro: G.
    - Verifica: la segnalazione di uscita contiene la sua dichiarazione scritta che nessun codice dell'azienda resta nei suoi repository privati.

## Se rientra

Se la stessa persona viene invitata di nuovo entro tre mesi, GitHub propone di ripristinare i team e gli accessi che aveva. Per un'uscita vera si riparte da zero, e si segue `docs/03-ingresso.md`.

## Il controllo finale

Verifica il risultato, non i passi, il giorno dopo l'uscita:

- nel registro, `actor:utente` non mostra nessun evento successivo all'uscita. Le date della ricerca `created:` sono in ora UTC: un evento subito dopo mezzanotte, in Italia, risulta il giorno prima;
- il suo nome non compare in People, nei team, fra i collaboratori esterni ne' fra i collaboratori di nessun repository;
- una proposta aperta da una delle persone che restano viene approvata da un'altra che resta, e si puo' unire.

Se una delle tre non torna, un accesso e' rimasto aperto, oppure il lavoro dipende ancora da chi e' uscito.
