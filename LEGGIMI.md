# Palinsesto AI su consulentia.net

Questa cartella contiene tutto quello che serve per pubblicare il Palinsesto AI sul tuo dominio.

| File | A cosa serve |
|---|---|
| `index.html` | Lo strumento: filtri, agenda, scheda evento, pulsanti calendario. Non va più toccato. |
| `events.json` | I contenuti. È l'unico file che cambia ogni settimana. |
| `AGGIORNAMENTO-SETTIMANALE.md` | Le istruzioni che Claude segue ogni settimana per aggiornare `events.json`. |

La pagina legge `events.json` all'apertura, quindi i due file devono stare **nella stessa cartella**.

---

## Architettura consigliata

```
GitHub (repository con i 3 file)
   │  ogni lunedì Claude aggiorna events.json e fa un commit
   ▼
Netlify  ──►  pubblica automaticamente a ogni commit
   ▼
palinsesto.consulentia.net   (sottodominio, record DNS CNAME)
   ▼
consulentia.net/palinsesto-ai   (pagina del tuo sito che lo mostra con un iframe)
```

Perché un sottodominio e non una cartella del sito principale: così l'aggiornamento automatico non deve avere accesso al server o al pannello del tuo sito. Tocca solo il repository del palinsesto.

---

## Passo 1 · Crea il repository su GitHub

1. Accedi a github.com (crea un account gratuito se non ce l'hai).
2. **New repository** → nome `palinsesto-ai` → visibilità *Private* o *Public*, a tua scelta.
3. **Add file → Upload files** → carica `index.html`, `events.json` e `AGGIORNAMENTO-SETTIMANALE.md` → **Commit changes**.

## Passo 2 · Pubblica con Netlify (gratuito)

1. Vai su app.netlify.com e accedi con GitHub.
2. **Add new site → Import an existing project → GitHub** → scegli `palinsesto-ai`.
3. Lascia vuoti *Build command* e *Publish directory* → **Deploy**.
4. Dopo un minuto ottieni un indirizzo tipo `nome-casuale.netlify.app`. Aprilo e verifica che il palinsesto si veda.

In alternativa vanno bene anche Cloudflare Pages o GitHub Pages: la procedura è quasi identica.

## Passo 3 · Collega il sottodominio palinsesto.consulentia.net

1. In Netlify: **Domain management → Add a domain** → scrivi `palinsesto.consulentia.net` → conferma.
2. Nel pannello DNS del tuo dominio (dove hai registrato consulentia.net: Aruba, Register.it, GoDaddy, Cloudflare…) aggiungi un record:

   | Tipo | Nome | Valore |
   |---|---|---|
   | CNAME | `palinsesto` | `nome-casuale.netlify.app` |

3. Attendi la propagazione (da pochi minuti a qualche ora). Netlify attiva da solo il certificato HTTPS.

## Passo 4 · Mostralo dentro il sito consulentia.net

Crea una pagina nel tuo sito (per esempio `/palinsesto-ai`) e incolla questo codice in un blocco HTML personalizzato. In WordPress è il blocco **HTML personalizzato**; in Wix è **Incorpora codice**; in Squarespace è **Blocco codice**.

```html
<iframe
  src="https://palinsesto.consulentia.net/"
  title="Palinsesto AI"
  style="width:100%; height:1600px; border:0;"
  loading="lazy"></iframe>
```

Se preferisci, puoi anche linkare direttamente `https://palinsesto.consulentia.net/` dal menu del sito, senza iframe.

---

## Aggiornamento settimanale

### Opzione A · Attività programmata di Claude (consigliata)

Ogni settimana Claude:

1. cerca i nuovi contenuti AI;
2. verifica date, orari e costo sulle pagine degli organizzatori;
3. toglie quelli passati e aggiorna `events.json`;
4. fa il commit su GitHub.

Netlify pubblica il commit in automatico. Non serve nessuna chiave API né codice da mantenere.

Per attivarla, dì a Claude il nome del repository (per esempio `tuoutente/palinsesto-ai`). Claude collega il repository e crea l'attività programmata con le istruzioni del file `AGGIORNAMENTO-SETTIMANALE.md`.

Per cambiare cosa cercare (temi, Paesi, fonti preferite), modifica quel file: Claude lo rilegge a ogni esecuzione.

### Opzione B · Aggiornamento manuale

Se non vuoi usare GitHub, puoi caricare `index.html` ed `events.json` via FTP in una cartella del sito, per esempio `consulentia.net/palinsesto-ai/`. Ogni settimana chiedi a Claude un nuovo `events.json` e lo ricarichi tu. Funziona, ma non è automatico.

---

## Formato di events.json

Puoi aggiungere o correggere eventi anche a mano direttamente su GitHub (matita → modifica → *Commit*).

```json
{
  "id": "slug-unico",
  "title": "Titolo",
  "org": "Organizzatore",
  "type": "webinar | online | presenza | conf | intervista | podcast",
  "category": "Agenti & automazione",
  "remote": true,
  "place": "Online",
  "cost": "free | paid | mixed | unknown",
  "costLabel": "Gratuito",
  "description": "Due righe sul contenuto.",
  "sessions": [{ "start": "2026-11-05T16:00:00+01:00", "end": "2026-11-05T17:00:00+01:00" }],
  "note": "Facoltativa: posti limitati, scadenze…",
  "url": "https://pagina-dell-organizzatore"
}
```

Per un evento senza orario, come una conferenza di più giorni, usa `"allDay": {"start": "2026-11-09", "end": "2026-11-12"}` al posto di `sessions`.

Gli orari vanno sempre con lo scarto dall'UTC:

- `+01:00` per l'ora solare, da fine ottobre a fine marzo;
- `+02:00` per l'ora legale.

Una categoria nuova compare da sola tra i filtri.
