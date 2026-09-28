# Sicurezza

## Come segnalare un problema di sicurezza

Non aprire una segnalazione pubblica. Le segnalazioni pubbliche sono visibili a chiunque, compreso chi potrebbe approfittare del problema prima che venga corretto.

Usa la scheda **Security**, poi **Report a vulnerability**. La segnalazione resta privata fra chi la scrive e chi la riceve, e diventa pubblica solo quando la correzione e' pubblicata.

## Cosa non deve mai entrare in questo repository

- Chiavi di servizi esterni, in qualunque forma
- Password, anche di ambienti di prova
- Stringhe di connessione a database
- Chiavi private, certificati, file `.pem`
- File `.env`

Un segreto scritto in un commit resta nella storia del repository anche dopo che lo cancelli dal file. Cancellarlo non basta: va considerato compromesso e sostituito.

I passi da seguire, nell'ordine e con chi li esegue, sono in `docs/05-credenziale-esposta.md`.

## Cosa facciamo per prevenirlo

- **Push protection** rifiuta la scrittura quando riconosce la forma di una chiave nota
- **Secret scanning** analizza tutta la storia e avvisa se trova qualcosa
- **Dependabot** avvisa quando una libreria che usiamo ha una vulnerabilita' nota
- Il file `.gitignore` tiene fuori i file di configurazione con i valori veri
