# Policy degli accessi

Questo documento esiste per una ragione sola: decidere le regole una volta, invece che ogni volta che qualcuno chiede un permesso.

Qualche parola, prima di cominciare:

- l'**organizzazione** e' lo spazio dell'azienda su GitHub, dove stanno tutti i progetti;
- un **repository** e' un progetto, con tutta la storia delle sue modifiche;
- `main` e' la versione buona di un progetto, quella che conta;
- un **team** e' un gruppo di persone che ricevono insieme lo stesso accesso;
- un **Owner** e' chi puo' fare tutto nell'organizzazione, anche cancellarla.

## Dove sta il codice aziendale

Tutto il codice dell'azienda vive nell'organizzazione `uav-associati`. Nessun progetto aziendale sta sull'account personale di qualcuno. Se ne trovi uno, va trasferito nell'organizzazione, non copiato.

## Chi e' Owner

Oggi l'Owner dell'organizzazione e' una persona sola: uomoaltovalore (Direzione). Owner e' un potere totale: chi lo ha puo' cancellare l'organizzazione, e solo un altro Owner puo' toglierglielo.

La regola e' averne almeno due, per non restare chiusi fuori se una persona non e' raggiungibile, e mai piu' di quanti ne servono. Il secondo Owner oggi manca: si nomina appena in azienda c'e' una seconda persona che puo' assumersi questa responsabilita'. Finche' manca, se uomoaltovalore non e' raggiungibile nessuno puo' cambiare le regole dell'organizzazione.

## Verifica in due passaggi

Nell'organizzazione entra solo chi ha attivato la verifica in due passaggi: oltre alla password serve un codice dal telefono o una chiave di sicurezza. GitHub lo fa rispettare da solo: chi non ce l'ha non puo' accettare l'invito, e chi la disattiva viene tolto dall'organizzazione.

## Chi crea i progetti

Solo gli Owner creano repository nell'organizzazione. Chi ne ha bisogno lo chiede alla Direzione, che lo crea e da' l'accesso al team giusto. Cosi' non nascono progetti che nessuno sa di avere, e nessun progetto diventa pubblico per sbaglio.

## Il permesso di base

Il permesso di base e' quello che ogni membro riceve su ogni repository per il solo fatto di far parte dell'organizzazione. Da noi e' **nessuno**: ogni accesso arriva da un team.

Con "lettura", un commerciale o un nuovo assunto vedrebbe il codice di tutti i progetti dal primo giorno. Con "scrittura", potrebbe anche modificarli.

## Come si concedono gli accessi

- L'accesso si da' ai **team**, non alle persone. Una persona entra in un team, e il team ha un permesso su un repository
- L'accesso diretto di una persona a un repository e' l'eccezione, e va motivato per iscritto
- Il permesso di partenza e' il piu' basso che permette di lavorare. Si sale su richiesta, non per precauzione

## Chi approva cosa

Ogni parte di un progetto ha i suoi responsabili, scritti nel file dei responsabili di zona (`.github/CODEOWNERS`). Quando qualcuno propone una modifica, GitHub chiama da solo i responsabili della parte toccata. Ogni zona ha almeno due persone che possono approvare, perche' nessuno puo' approvare la propria proposta.

Cambiare il file dei responsabili, cioe' decidere chi approva cosa, richiede l'approvazione di un membro del team **revisori** diverso da chi propone la modifica.

## Le protezioni minime su ogni repository

Sulla versione buona di ogni progetto (`main`):

- nessuno scrive direttamente: ogni modifica passa da una proposta
- ogni proposta ha bisogno dell'approvazione di almeno **1** persona diversa dall'autore, e deve essere un responsabile della parte toccata
- i controlli automatici devono essere tutti verdi
- la versione buona non si puo' cancellare
- la sua storia non si puo' riscrivere: la cosiddetta scrittura forzata e' vietata

## Fornitori esterni

I fornitori esterni non diventano membri dell'organizzazione. Ricevono accesso a un solo repository, per **3 mesi**, rinnovabili per iscritto. GitHub non fa scadere l'accesso da solo: le scadenze si controllano il **primo giorno lavorativo di ogni mese**, insieme al controllo mensile (`docs/06-controllo-mensile.md`), e un accesso scaduto si toglie quel giorno.

## Revisione periodica

Due volte l'anno, a **marzo** e a **settembre**, si rileggono tutti gli accessi e si toglie tutto cio' che non e' piu' giustificato. Responsabile: **uomoaltovalore** (Direzione); la Consulente sicurezza (Nina) rilegge il risultato.

---

Prossima revisione: **lunedi' 1 marzo 2027**. Ne risponde **uomoaltovalore** (Direzione).
