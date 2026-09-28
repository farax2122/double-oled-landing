# Landing page

`index.html` è la pagina di presentazione del Double OLED Gaming Monitor. È un file unico: si apre con un doppio clic o si pubblica così com'è.

## Modulo contatti con Google Form

Il modulo in fondo alla pagina manda i dati a un Google Form. Le risposte si leggono nel Form (scheda *Risposte*) oppure nel Google Sheet collegato, e Google può mandare una mail a ogni nuova risposta. Nella pagina non c'è nessun indirizzo email.

Il Form attuale è già creato e collegato (28/09/2026):

- modifica e risposte: <https://docs.google.com/forms/d/1T-TXdFzSdpUF2DCOR8AgyuDe7XcJEHhny4_O09-3SNI/edit>
- pagina pubblica: <https://docs.google.com/forms/d/e/1FAIpQLScOR3qFDndiFD6N7wWPzHPb3y22_4JNrCf-KAb6WcQCTMfOPw/viewform>

Se cambi o aggiungi domande nel Form, gli `entry.*` in `GOOGLE_FORM` vanno riletti (passo 2). I passi qui sotto servono solo per rifare il Form da zero. Se `GOOGLE_FORM` è vuoto, il modulo mostra "Il modulo non è ancora attivo" e non invia niente.

### 1. Crea il Form

Su <https://forms.google.com> crea un modulo vuoto con queste quattro domande, in quest'ordine:

| Domanda | Tipo | Obbligatoria |
|---|---|---|
| Nome | Risposta breve | sì |
| Email | Risposta breve | sì |
| Cosa ti interessa | Scelta multipla | sì |
| Messaggio | Paragrafo | no |

Le opzioni di "Cosa ti interessa" devono essere scritte esattamente così, altrimenti Google scarta la risposta:

- `Acquistarne uno`
- `Firmware e codice`
- `Versione su misura`
- `Altro`

In *Impostazioni > Risposte* lascia disattivati "Raccogli indirizzi email" e "Limita a 1 risposta" (richiedono l'accesso Google e bloccano l'invio dalla pagina).

### 2. Leggi FORM_ID e gli entry

1. Nel Form apri il menu ⋮ e scegli *Ottieni link precompilato*.
2. Compila tutti i campi con valori di prova (per esempio `NOME`, `EMAIL`, `Altro`, `MSG`) e premi *Ottieni link*, poi *Copia link*.
3. Il link ha questa forma:

   ```
   https://docs.google.com/forms/d/e/1FAIpQL...xyz/viewform?usp=pp_url&entry.123456=NOME&entry.234567=EMAIL&entry.345678=Altro&entry.456789=MSG
   ```

   - `FORM_ID` è la parte tra `/d/e/` e `/viewform`.
   - Ogni `entry.NUMERO` corrisponde al campo con il valore di prova accanto.

Puoi anche incollare il link nella chat con Claude e fargli fare il passo 3.

### 3. Inserisci i valori in index.html

Cerca `GOOGLE_FORM` nello script in fondo a `index.html` e sostituisci i segnaposto:

```js
const GOOGLE_FORM = {
  FORM_ID: "1FAIpQL...xyz",
  fields: { nome: "entry.123456", email: "entry.234567", interesse: "entry.345678", messaggio: "entry.456789" }
};
```

### 4. Prova e ricevi le notifiche

- Invia una richiesta di prova dalla pagina e controlla che compaia in *Risposte*.
- Nella scheda *Risposte* premi l'icona di Sheets per avere tutte le richieste in un foglio.
- Dal menu ⋮ della scheda *Risposte* attiva *Ricevi notifiche email per le nuove risposte*.

### Limiti

- La pagina non può leggere la risposta di Google (CORS), quindi mostra "Richiesta inviata" appena la richiesta parte. Se le opzioni del Form non coincidono con quelle della pagina, Google scarta la risposta senza che la pagina lo sappia: per questo va fatta la prova del punto 4.
- Il modulo invia i dati da una pagina ospitata su un sito normale (GitHub Pages, Netlify, file aperto in locale). Sull'anteprima Artifact di claude.ai l'invio verso siti esterni può essere bloccato.
