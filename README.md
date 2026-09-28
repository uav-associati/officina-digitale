# Officina Digitale

Repository di partenza del percorso **GitHub da zero a cento**.

Officina Digitale è un'azienda di dieci persone che vende un gestionale di fatturazione ai suoi clienti. L'azienda non esiste, il codice sì: è volutamente piccolo e leggibile, perché serve a esercitarsi, non a essere studiato.

Questo repository contiene tutto ciò che serve al percorso: il prodotto, i documenti aziendali, l'organigramma, i modelli di segnalazione e un controllo automatico che gira su ogni proposta di modifica.

## Come si usa

Non si clona questo repository: se ne crea una copia propria con il pulsante **Use this template**, in alto a destra. La copia è tua, ci puoi rompere tutto, e nessuna delle tue modifiche tocca l'originale.

Il percorso ti guida passo per passo. La lezione 0.4 è quella che ti fa fare la copia.

## Cosa c'è dentro

| Percorso | Contenuto |
|---|---|
| `src/` | Il prodotto: calcolo delle fatture e anagrafica clienti |
| `test/` | I controlli automatici che verificano che il prodotto funzioni |
| `docs/` | I documenti aziendali: processo, policy di accesso, ingresso e uscita delle persone, cosa fare se una credenziale finisce esposta |
| `TEAM.md` | Le dieci persone di Officina Digitale e cosa deve poter fare ciascuna |
| `listino.csv` | Il listino prezzi. Serve alle lezioni sui conflitti |
| `.github/` | Modelli di segnalazione, modello di proposta di modifica, controlli automatici |
| `CODEOWNERS.esempio` | Il file dei responsabili, da attivare durante il percorso |

## Un avviso sul controllo automatico

Appena fai la tua copia, il controllo automatico su `main` è **rosso**. Non è un errore di configurazione: in `src/fatture.js` c'è un difetto vero, lo sconto non viene applicato al totale. È il primo lavoro che farai, ed è il filo conduttore dei moduli sul lavoro di squadra.

## Licenza

MIT. Vedi `LICENSE`.
