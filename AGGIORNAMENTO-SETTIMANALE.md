# Istruzioni per l'aggiornamento settimanale del Palinsesto AI

Sei incaricato di aggiornare `events.json` in questo repository. La pagina `palinsesto.consulentia.net` lo legge così com'è. Non modificare `index.html`.

## 1. Pulizia

- Rimuovi gli eventi completamente terminati.
  - Per un evento con `sessions`: l'ultima sessione è già finita.
  - Per un evento con `allDay`: la data `end` è passata.
- Togli dalle `sessions` gli incontri già passati degli eventi ancora in corso.

## 2. Ricerca di nuovi contenuti

L'orizzonte è **le prossime 8 settimane**. Cerca contenuti sull'intelligenza artificiale di questi formati:

- webinar;
- corsi online;
- corsi in presenza;
- conferenze;
- interviste e fireside chat;
- podcast e videocast in diretta.

Priorità:

1. Italia, in italiano. Dai precedenza a Milano e Lombardia, poi al resto d'Italia.
2. Grandi eventi internazionali seguibili online o rilevanti per professionisti italiani.

Fonti da controllare sempre, oltre alla ricerca web generale. Apri **tutte le pagine** dei calendari, non solo la prima:

- https://www.24orebs.com/eventi (Sole 24 Ore Business School)
- https://www.assolombarda.it/forgia-en/eventi
- https://www.t2i.it
- https://www.fbk.eu
- https://www.aiweek.it
- https://aixia.it/eventi/
- https://neuronfeed.com/conferences?topic=AI
- i calendari dei Punti Impresa Digitale delle Camere di Commercio

Fai almeno 10 ricerche diverse. Usa query in italiano e in inglese, per esempio:

- "webinar intelligenza artificiale <mese> <anno>"
- "corso AI online <mese> <anno>"
- "AI conference <mese> <anno>"
- "podcast live AI"

## 3. Verifica di ogni nuovo evento

Prima di aggiungere un evento:

- apri la pagina dell'organizzatore;
- conferma titolo, data, orario e luogo;
- conferma il costo.

**Regole sugli orari:**

- Scrivili in ora di Roma, con lo scarto dall'UTC: `+01:00` in ora solare, `+02:00` in ora legale.
- Se un sito mostra orari incoerenti con altre fonti, verifica. Il sito 24orebs.com, letto in automatico, mostra gli orari in UTC.
- Se l'orario non è pubblicato, usa `allDay` e spiegalo in `note`.

**Regole sul costo:**

| Situazione | `cost` | `costLabel` |
|---|---|---|
| Gratuito | `"free"` | `"Gratuito"` |
| A pagamento | `"paid"` | Il prezzo, se pubblicato, per esempio `"€ 290"`. Altrimenti `"A pagamento"`. |
| Gratis solo in parte | `"mixed"` | Una spiegazione, per esempio `"Gratis online · a pagamento in presenza"`. |
| Non trovato | `"unknown"` | `"Costo non indicato"` |

Non inventare mai un costo. Se non lo trovi, usa `"unknown"`.

**Altre regole:**

- Scarta gli eventi senza una data certa.
- Scarta i duplicati: lo stesso evento può comparire su più siti.

## 4. Campi

Rispetta il formato descritto in `LEGGIMI.md`.

- **`type`**: uno tra `webinar`, `online` (corso online), `presenza` (corso in presenza), `conf`, `intervista`, `podcast`.
- **`category`**: usa preferibilmente una di queste:
  - Agenti & automazione
  - AI generativa & prompt
  - Marketing & comunicazione
  - Regole, etica & sicurezza
  - Business & strategia
  - Sviluppo & ingegneria
  - Ricerca
  - Lavoro & competenze
  - Scuola & società
  - AI nei settori
- **`description`**: una o due frasi scritte da te. Non copiare testo dalle fonti.
- **`remote`**: `true` se si può seguire online.
- **`url`**: la pagina dell'evento o dell'organizzatore.

## 5. Chiusura

- Ordina gli eventi per data di inizio.
- Aggiorna `"updated"` con la data di oggi nel formato `AAAA-MM-GG`.
- Controlla che il file sia JSON valido, per esempio con `python3 -m json.tool events.json`.
- Fai un commit su `main` con il messaggio `Palinsesto AI: aggiornamento del <data>` e fai il push.

In chiusura, invia all'utente un riepilogo breve:

- quanti eventi hai aggiunto e quanti rimosso;
- i 3 aggiunti più interessanti;
- gli eventi con dati incerti.
