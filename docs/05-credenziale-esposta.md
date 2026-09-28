# Credenziale esposta

Da usare quando una chiave, un token o una password finisce dove non doveva: in un commit, in una chat, in un'email, in un documento condiviso, in uno screenshot, in un log.

Esposta vuol dire compromessa, anche se sembra che nessuno l'abbia vista. L'ordine conta: prima si toglie valore alla chiave, poi si sistema il codice. `SECURITY.md` spiega come evitare che succeda; questo documento dice cosa fare quando succede lo stesso.

Da eseguire nell'ordine. Ogni riga e' verificabile: o e' fatta o non lo e'.

1. **Revocare la chiave** nel servizio che l'ha emessa, dalla pagina indicata nella tabella in fondo. Si revoca subito, anche se il servizio resta fermo finche' non arriva la chiave nuova: qualche minuto di servizio fermo costa meno di una chiave in mano ad altri.
   - Fatto quando: la chiave non compare piu' fra quelle attive del servizio.
   - Chi: chi se ne accorge, se ha i permessi per farlo; altrimenti la Responsabile tecnica (Marta). Deploy key e segreti delle Actions li toglie chi amministra il repository: la Direzione (uomoaltovalore).
   - Tempo: entro 15 minuti dalla scoperta.

2. **Sostituirla** con una chiave nuova, con i permessi minimi necessari e, se il servizio lo permette, una scadenza. La chiave nuova va solo nelle variabili d'ambiente del server o nei segreti di GitHub: mai nel repository, mai in chat o per email. Su Stripe la rotazione fa i passi 1 e 2 insieme.
   - Fatto quando: il servizio torna a funzionare con la chiave nuova. Si verifica ripetendo l'operazione che la usa: l'invio di una fattura, di un'email, un pagamento di prova.
   - Chi: la Responsabile tecnica (Marta), con uno Sviluppatore (Luca o Sara) per aggiornare i punti in cui la chiave e' usata.
   - Tempo: entro un'ora dalla revoca.

3. **Verificare gli accessi avvenuti.** Si ricostruisce il periodo fra il momento dell'esposizione e la revoca, e si legge nei registri del servizio cosa ha fatto la chiave in quel periodo: ultimo utilizzo, indirizzi di provenienza, operazioni eseguite. Su GitHub e' il registro attivita' dell'organizzazione; su Stripe, i registri delle richieste della chiave; su Hostinger, l'attivita' dell'account.
   - Fatto quando: la segnalazione dell'incidente contiene una nota con il periodo, i registri letti e l'esito.
   - Chi: la Consulente sicurezza (Nina) legge i registri. Il registro dell'organizzazione GitHub lo apre la Direzione (uomoaltovalore), quelli degli altri servizi la Responsabile tecnica (Marta). Se la chiave dava accesso a dati dei clienti decide il Fondatore (Giovanni), anche sulla notifica al Garante per la protezione dei dati personali: va fatta entro 72 ore da quando ci si e' accorti della violazione, salvo che sia improbabile un rischio per le persone coinvolte.
   - Tempo: in giornata, una o due ore.

4. **Toglierla dal codice.** Al posto del valore si scrive il nome di una variabile d'ambiente, come in `src/config.esempio.js`, con una normale proposta di modifica. Se GitHub ha aperto un avviso (scheda **Security**, **Secret scanning**), lo si chiude con il motivo **Revoked**: GitHub non lo chiude da solo quando la chiave sparisce dal codice.
   - Fatto quando: la proposta e' unita, nel codice resta solo il nome della variabile, l'avviso e' chiuso.
   - Chi: uno Sviluppatore (Luca o Sara) scrive la proposta; la approva il responsabile della zona in cui si trova il file (`TEAM.md`, "Le zone del progetto"). L'avviso lo chiude la Responsabile tecnica (Marta).
   - Tempo: entro il giorno lavorativo successivo. Mezz'ora di lavoro, piu' il tempo della revisione.

5. **Decidere se riscrivere la storia.** Di norma non si riscrive: la chiave revocata non vale piu' niente, e le copie gia' fatte del repository non si possono richiamare. Anche il supporto di GitHub cancella le copie in cache solo quando il rischio non si elimina cambiando la credenziale. Si riscrive solo se il dato esposto non si puo' revocare, per esempio dati personali dei clienti. Riscrivere vuol dire sovrascrivere `main`, cosa che le protezioni vietano: serve un'eccezione temporanea, i commit cambiano identificativo, e chiunque abbia una copia deve rifarla, altrimenti rischia di rimettere il dato su GitHub con il primo push.
   - Fatto quando: la decisione e' scritta nella segnalazione, con il motivo. Se si riscrive, in piu': la storia nuova e' su GitHub, l'eccezione e' stata tolta, tutti hanno rifatto la copia, il supporto di GitHub ha ricevuto la richiesta di cancellare le copie in cache.
   - Chi: la Responsabile tecnica (Marta), insieme al Fondatore (Giovanni) se ci sono dati dei clienti. L'eccezione alle protezioni la concede la Direzione (uomoaltovalore), e la toglie subito dopo.
   - Tempo: la decisione, un quarto d'ora. Se si riscrive, mezza giornata in cui nessuno lavora sul repository.

6. **Capire cosa mancava nel processo.** Senza cercare colpevoli: da dove e' uscita la chiave, quale protezione mancava o non ha funzionato, cosa si cambia perche' non succeda di nuovo.
   - Fatto quando: una segnalazione contiene l'analisi e almeno un'azione correttiva, con un responsabile e una data.
   - Chi: la Responsabile tecnica (Marta), con la Consulente sicurezza (Nina). Se l'azione correttiva e' una regola nuova, la approva il Fondatore (Giovanni).
   - Tempo: entro una settimana. Un'ora di riunione; l'azione correttiva ha la sua scadenza.

## Dove si revoca una chiave

| Servizio | Cosa si revoca | Pagina |
|---|---|---|
| GitHub | Token personali fine-grained | https://github.com/settings/personal-access-tokens |
| GitHub | Token personali classici | https://github.com/settings/tokens |
| GitHub | App autorizzate ad agire per conto dell'account, per esempio la riga di comando `gh` | https://github.com/settings/applications |
| GitHub | Chiavi SSH dell'account | https://github.com/settings/keys |
| GitHub | Deploy key di questo repository | https://github.com/uav-associati/officina-digitale/settings/keys |
| GitHub | Segreti delle Actions di questo repository | https://github.com/uav-associati/officina-digitale/settings/secrets/actions |
| Posta: Hostinger Email | Password di una casella: **Mailboxes** accanto al dominio, poi il menu con i tre puntini della casella, **Change Password** | https://hpanel.hostinger.com/emails/ |
| Pagamenti: Stripe | Chiavi API: menu con i tre puntini della chiave, **Rotate key**, scadenza **Now**. La rotazione revoca la vecchia chiave e ne crea una nuova; con una scadenza diversa da Now la vecchia resta valida fino a quella data | https://dashboard.stripe.com/apikeys |
| Hosting: Hostinger | Token API: si eliminano dalla stessa pagina in cui si creano, e smettono subito di funzionare | https://hpanel.hostinger.com/api |
| Hosting: Hostinger | Password FTP: **Dashboard** del sito, **Files**, **FTP Accounts**, poi il cambio password dell'account | https://hpanel.hostinger.com/websites |
| Hosting: Hostinger | Chiavi SSH: **Dashboard** del sito, **SSH Access**, **Delete** sulla chiave | https://hpanel.hostinger.com/websites |
