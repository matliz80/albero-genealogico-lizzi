# Albero genealogico Lizzi — istruzioni

Il file `index.html` è un sito statico completo. Legge i dati dal Google Sheet a ogni apertura della pagina, quindi si aggiorna da solo appena modifichi il foglio.

**Novità rispetto alla versione precedente:** la disposizione delle caselle e il tracciato delle linee non sono più coordinate fisse incorporate nel file, ma vengono **ricalcolati ogni volta** a partire dai legami di parentela (`ID Padre` / `ID Madre`) e, per l'ordine dei fratelli, dalla data di nascita. Il vantaggio: per aggiungere una persona — anche "in mezzo" a fratelli già esistenti — basta una riga nel foglio, senza mai toccare il codice. Il rovescio della medaglia: l'albero non riproduce più esattamente il disegno a mano del PDF originale, ma una ricostruzione ordinata secondo la stessa logica.

## 1. Aggiorna il Google Sheet

Usa **`albero_lizzi.xlsx`**, non il CSV: apri il tuo foglio → **File → Importa → Carica** → seleziona `albero_lizzi.xlsx` → **"Sostituisci il foglio corrente"** (non "Aggiungi nuovo foglio") → Importa dati.

> ### ⚠️ Perché l'xlsx e non il CSV
> Google Sheets, importando un CSV, "riconosce" da solo numeri e date e riscrive le celle. Sui dati di un albero genealogico fa disastri:
> - `1580` (anno di nascita di Del Lizij Bernardo) viene letto come **numero di giorni** dal 30.12.1899 e diventa `28.04.1904`;
> - `4.12.1610` (Pietro) viene letto come **durata** 4 ore 12 minuti 1610 secondi e diventa `4.38.50`.
>
> Nel file `.xlsx` le colonne sono già marcate come *Testo semplice*, quindi Sheets non tocca niente.
> Se preferisci comunque il CSV, nella finestra di importazione metti **"Converti testo in numeri, date e formule" su NO**.
>
> Per controllare che sia andata bene: la cella `Data Nascita` di `P003` deve contenere `1580` e quella di `P006` deve contenere `4.12.1610`. Se vedi `28/04/1904` o `4.38.50`, la conversione è avvenuta e va rifatta l'importazione.

Colonne del file:

| Colonna | Contenuto |
|---|---|
| ID Persona | P001 … P403 (o oltre) — la chiave che collega ogni riga ai suoi genitori/figli. **Non modificarla dopo averla assegnata.** |
| Nome | nome così com'è scritto nel disegno |
| Sesso (M/F) | dedotto dal nome (utile solo per il colore della barretta): correggilo pure dove sbaglia |
| Data Nascita / Data Morte | date come nel disegno. **Importante ora**: la data di nascita determina anche l'ordine tra fratelli — vedi punto 4 |
| ID Padre | genitore. È il campo che decide dove finisce la casella nell'albero |
| Annotazioni | matrimoni, luoghi, note |
| **Colonna H** | una **X** apre un nuovo gruppo di colore: quella casella e tutte le successive nel foglio prendono la stessa tinta, fino alla X seguente. Celle vuote = si prosegue col gruppo in corso |
| **Colonna I** | testo scritto sulla barra orizzontale sotto il cartiglio di quella persona (la barra da cui scendono i suoi figli). Vedi sezione 4ter |

Il foglio deve restare condiviso con **"Chiunque abbia il link" → Visualizzatore** (pulsante Condividi). Senza questa impostazione il sito non riesce a leggere i dati.

Sono riconosciute anche, se le aggiungi in futuro: `Cognome`, `Luogo Nascita`, `Luogo Morte`, `Coniuge`, `ID Madre`, `Foto`.

## 2. Ricarica il sito su GitHub

1. Vai su `https://github.com/matliz80/albero-genealogico-lizzi`
2. Clicca sul file `index.html` → icona della **matita** (Edit) → seleziona tutto e incolla il nuovo contenuto. In alternativa: **Add file → Upload files**, trascina il nuovo `index.html` e conferma (sovrascrive il precedente).
3. Commit. Dopo circa un minuto il sito online è aggiornato: se vedi ancora la versione vecchia fai un ricaricamento forzato (Ctrl+F5, o Cmd+Shift+R su Mac).

Non serve creare un secondo repository per poter tornare indietro: GitHub conserva ogni versione nella cronologia dei commit (tab **"History"** del file). Per ripristinare la versione precedente basta aprire il commit di prima e cliccare **"Restore this file"**.

## 3. Funzioni del sito

Tutti i comandi stanno in **un'unica barra in cima alla finestra dell'albero**: cerca · Adatta · − zoom + · ⛶ · Esporta PDF. L'albero comincia sotto la barra e non ci passa mai sopra.

- **− valore +**: il numero al centro è lo zoom in corso; cliccandolo torni al 100%. Su computer anche Ctrl+rotellina.
- **Su cellulare**: pizzica con due dita per ingrandire e rimpicciolire, trascina con un dito per spostarti.
- **Schermo intero**: icona ⛶. La scheda della persona funziona anche a schermo intero.
- **Cerca**: scrivi un nome, la casella viene evidenziata e la vista ci si sposta sopra.
- **Click su una casella**: apre la scheda con tutti i dati e permette di "mettere a fuoco" solo quel ramo. In modalità ramo compare **↩ Vista completa** e gli altri comandi restano spenti finché non torni alla vista intera.
- **Esporta PDF**: apre la stampa del browser (scegli "Salva come PDF").

- **Cerca**: trova sia nomi sia date. Funzionano «Mattia», «1650», «5.1.1606», «05.01.1606», i cognomi dei coniugi citati nelle annotazioni e anche gli ID (`P154`). Sotto la barra compare il numero di risultati.
- **Colori dei gruppi** (icona a tavolozza): accende e spegne le tinte definite dalla colonna H. Vedi sezione 4bis.
- **Stampa**: apre il pannello per il plotter. Vedi sezione 7.

Le date vengono mostrate come **gg.mm.aaaa** anche se nel foglio sono scritte in forma breve (5.3.1804 → 05.03.1804). I valori ambigui presenti nel disegno originale — «21 / 22.2.1720», «1650?», «a 6 mesi» — restano invece come sono, così si vedono e si possono correggere nel foglio.

## 4bis. Colorare i rami (colonna H)

Nella **colonna H** del foglio, una **X** segna l'inizio di un nuovo gruppo di colore. Quella riga e tutte le righe successive del foglio ricevono la stessa tinta, finché non si incontra un'altra X, che apre il gruppo seguente. Le celle vuote non interrompono nulla.

Esempio: se metti una X sulla riga di `P005` e la successiva su `P154`, tutte le persone comprese fra le due righe (nell'ordine del foglio, non dell'albero) condivideranno un colore.

Le tinte sono dodici, tutte **scure**, scelte distanti fra loro in tinta e simili in luminosità: bruno, verde bosco, terra bruciata, blu notte, prugna, oliva, ottanio, indaco, mattone, muschio, ambra, violetto. Oltre la dodicesima ricominciano da capo.

Il colore si applica a **linee di collegamento, cornice del cartiglio e nome**. Lo sfondo del cartiglio resta avorio in tutti i gruppi, così il testo mantiene sempre il massimo contrasto e la carta non risulta sporcata sui grandi formati.

Sulle linee la regola è: il tronco che scende dal genitore e la barra orizzontale prendono il colore del **genitore**; la calata verticale verso ciascun figlio prende il colore del **figlio**. Così il punto esatto in cui comincia un nuovo gruppo si vede a colpo d'occhio anche da lontano.

L'icona a tavolozza nella barra spegne e riaccende i colori senza toccare il foglio.

**Nota importante:** il gruppo segue **l'ordine delle righe nel foglio**, non la struttura dell'albero. Se vuoi che un colore corrisponda esattamente a un ramo, le righe di quel ramo devono essere contigue nel foglio.

## 4. Aggiungere persone in futuro (anche in mezzo a fratelli esistenti)

**Prima di scrivere date a mano**, assicurati che le colonne **D** e **E** siano formattate come testo: clicca sulla lettera D, tieni Ctrl (Cmd su Mac) e clicca sulla E, poi **Formato → Numero → Testo semplice**. Senza questo, scrivendo `4.04.2024` Google Sheets lo legge come una durata (4 ore, 4 minuti, 2024 secondi) e lo trasforma in `4.37.44`. In alternativa, per una cella singola, scrivi un apostrofo davanti: `'4.04.2024` — l'apostrofo non si vede nel foglio e la data resta intatta.

Aggiungi poi una riga con un ID nuovo, il nome, la **data di nascita** (anche solo l'anno, es. `1650`) e l'**`ID Padre`** di chi la genera. Esempio — un fratello dimenticato di P005, nato nel 1650, che deve inserirsi tra i suoi fratelli già presenti:

| ID Persona | Nome | Sesso (M/F) | Data Nascita | Data Morte | ID Padre | Annotazioni |
|---|---|---|---|---|---|---|
| P404 | Osvaldo | M | 1650 | | P005 | |

**Come si posiziona:** il sito ordina automaticamente tutti i figli di uno stesso genitore per data di nascita, e mette ciascuno nella riga giusta sotto il genitore, centrato rispetto ai propri discendenti. Il nuovo Osvaldo (1650) si inserirà da solo tra il fratello del 1648 e quello del 1651, spostando visivamente chi viene dopo — nessuna coordinata da scrivere, nessun intervento sul codice.

**Chi non ha una data di nascita leggibile** (o ce l'ha ambigua tipo «1650?») **non viene mai spostato**: resta fermo nella posizione in cui compare nel foglio, esattamente come oggi. Questo vale sia per le persone già presenti sia per quelle nuove: se aggiungi qualcuno senza data, verrà semplicemente accodato dove la riga lo colloca, senza sfalsare gli altri.

Funziona a catena su tutte le generazioni: aggiungere un figlio a una persona già presente ricalcola solo il ramo sotto di lei; il resto dell'albero resta dov'era.

## 4ter. Etichette sulle linee (colonna I)

Il testo scritto nella **colonna I** di una persona viene stampato **sulla barra orizzontale che sta sotto il suo cartiglio**, cioè quella da cui scendono i suoi figli. La barra si interrompe e il testo si centra nel vuoto lasciato in mezzo, in **maiuscolo** e in **Cinzel bold**, un serif lapidario adatto alle diciture d'insieme.

Serve a dare un nome ai rami: «CAPORIACCO», «I TRESEMAN», «RAMO DI RAGOGNA». Scrivi in minuscolo se vuoi, il sito converte da solo in maiuscolo.

Il corpo del testo si adatta da solo: parte da 46 e si riduce fino a 20 se la barra è corta, così l'etichetta non sborda mai oltre i figli. Se la persona ha **un solo figlio** non esiste una barra su cui scrivere, quindi l'etichetta viene messa in corpo ridotto accanto alla calata verticale. Se la persona non ha figli, l'etichetta viene ignorata.

L'etichetta è compresa anche nella ricerca: cercando «Treseman» trovi la persona che porta quell'etichetta.

## 5. Chi può modificare i dati

Non serve un login sul sito: invita le persone direttamente sul Google Sheet come **Editor** (Condividi → email → ruolo Editor). Le modifiche compaiono sul sito al ricaricamento della pagina.

## 6. Cambiare foglio in futuro

Apri `index.html` con un editor di testo e modifica la riga vicino all'inizio dello `<script>`:

```js
const SHEET_ID = "12tz3Vr_gPEchYxVHoF8Ot9IqC1zvfYYdD8I2RvlWdXk";
```

sostituendo l'ID con quello del nuovo foglio (si trova nell'URL tra `/d/` e `/edit`).

## 7. Stampa su plotter

Il pulsante **Stampa** apre un pannello con due regolazioni e quattro misure aggiornate in tempo reale.

**Come funziona la geometria.** L'albero è largo e basso: circa 49.300 unità di larghezza contro 5.160 di altezza, cioè un rapporto di poco meno di **10 a 1**. Fissata l'altezza del telo, la lunghezza è una conseguenza automatica di quel rapporto. Con un telo alto **1 m** vengono circa **9,6 m di lunghezza**, con cartigli di 5,7 × 4,2 cm e il nome in corpo 5,4 mm.

**I due comandi:**

- **Altezza del telo (cm)** — la misura utile del rotolo. Cambiandola cambiano proporzionalmente lunghezza e dimensione dei cartigli.
- **Distanza fra le generazioni** — è la leva vera per accorciare il telo. Allontanando le righe l'albero diventa proporzionalmente meno lungo a parità di altezza, ma i cartigli rimpiccioliscono. Valori indicativi con telo da 1 m:

| Distanza | Lunghezza telo | Cartiglio | Nome |
|---|---|---|---|
| 324 (iniziale) | 9,6 m | 5,7 × 4,2 cm | 5,4 mm |
| 500 | 6,3 m | 3,7 × 2,8 cm | 3,6 mm |
| 700 | 4,6 m | 2,7 × 2,0 cm | 2,6 mm |
| 900 | 3,6 m | 2,1 × 1,6 cm | 2,0 mm |

Il riquadro delle misure diventa rosso quando il corpo del nome scende **sotto i 2 mm**, soglia oltre la quale la stampa non è più comodamente leggibile. Il pulsante **Ripristina valori iniziali** riporta tutto ai valori di partenza.

**Consiglio pratico:** prima di lanciare i 20 metri, stampa una striscia di prova di 1-2 m con le impostazioni scelte, per verificare dal vivo la leggibilità del corpo tipografico sul supporto che userai. La resa cambia molto fra carta opaca e materiale lucido.

**Da dove viene il risparmio.** La prima versione a layout dinamico misurava 103.900 unità (20,1 m di telo). Il passaggio al packing a contorni descritto in fondo a questo documento ha dimezzato la larghezza portandola a 49.300 unità: **53% in meno**, cioè oltre dieci metri di telo risparmiati a parità di dimensione dei cartigli.

Sotto quella soglia non si scende senza rimpicciolire: la lunghezza residua è determinata dal numero di persone che devono stare fisicamente affiancate nella riga più affollata, ognuna larga quanto un cartiglio.

---

### Nota sulla provenienza dei dati

Nomi, date e annotazioni sono stati estratti dal testo vettoriale del PDF `albero exp3.pdf` (nessuna lettura "a occhio"): il testo è quindi identico all'originale, refusi compresi. Esempio: «Nicolò n. 2.11.1611» compare così nel disegno pur essendo fratello di persone nate dal 1711 al 1718 — è un errore del documento di partenza, non della trascrizione.

I legami di parentela (chi è figlio di chi) sono stati ricostruiti a partire dal disegno originale e restano quelli anche nella versione attuale: quello che è cambiato è solo **come vengono disegnate** le caselle, non i legami stessi.

**11 persone non hanno un `ID Padre`** (nessuna linea disegnata verso l'alto nel PDF originale, verificato ingrandendo il documento) e restano quindi "radici" separate finché non si completa il dato a mano nel foglio:

| ID | Nome | Nato |
|---|---|---|
| P001 | Del Lizij Bernardo | 20.11.1537 — è il capostipite, corretto così |
| P105 | Domenica | 26.5.1728 |
| P106 | Anna | 4.4.1730 |
| P107 | Giovanni | 27.7.1732 |
| P108 | Valentina | 29.3.1735 |
| P109 | Mattia | 19.2.1737 |
| P110 | Sebastiana | 1.11.1740 |
| P111 | Valentino | 16.10.1745 |
| P112 | Bernardo | 1.12.1747 |
| P113 | Bernardo | 14.7.1750 |
| P239 | Giuseppe | 2.7.1862 |

Da P105 a P113 sono nove fratelli: la loro barra orizzontale nel disegno finisce senza risalire a nessuna casella. Basta scrivere l'ID del padre nella riga P105 e ripeterlo sulle altre otto: al ricaricamento della pagina il gruppo si aggancerà automaticamente al resto dell'albero, nella posizione cronologicamente corretta tra gli altri figli di quel genitore.

**Tre date incomplete già nel disegno**, lasciate come sono perché mancano delle cifre — da correggere nel foglio se si scopre il valore giusto:

| ID | Valore nel disegno | Nota |
|---|---|---|
| P072 | `8.3.171 0` | anno spezzato, presumibilmente 1710 |
| P207 | `sposato il 6.2.187 con Giovanni Beinat` | anno di tre cifre |
| P284 | `3.0.6.1907` | un punto di troppo |

---

### Come è fatto il sito, per chi ci mette mano

**Struttura del file.** Un solo `index.html`, senza compilazione né dipendenze. Dentro, nell'ordine: CSS, corpo della pagina, e uno `<script>` con: lettura/parsing del CSV, il **motore di layout dinamico**, il disegno delle caselle, e i comandi (zoom, ricerca, scheda dettaglio, focus-ramo, export). Non esiste più l'oggetto `GEO` con le coordinate incorporate: nessun file da rigenerare quando cambiano i dati.

**Come funziona il motore di layout.** Per ogni persona si individua il genitore (ID Padre, o ID Madre se manca il primo) e si costruisce l'albero dei discendenti. Dentro ogni gruppo di fratelli: chi ha una data di nascita leggibile (si estrae il primo anno plausibile 1300–2099 dalla stringa, anche se scritta in modo sporco) viene riordinato cronologicamente **nelle posizioni già occupate da fratelli con data**; chi non ha data resta fermo nella posizione in cui compare nel foglio. La posizione verticale è la generazione moltiplicata per il passo fra le righe. Le persone senza genitore riconosciuto diventano radici indipendenti. Se un errore nel foglio creasse un ciclo (qualcuno antenato di sé stesso), il collegamento viene tagliato automaticamente invece di bloccare la pagina.

**Il packing orizzontale, che è la parte delicata.** La prima versione accodava i sottoalberi uno dopo l'altro: ogni persona senza figli si prendeva una colonna intera e ogni ramo riservava tutto il proprio ingombro complessivo, anche dove era vuoto. Da qui una larghezza enorme.

La versione attuale usa un **algoritmo a contorni** (della famiglia Reingold–Tilford). Ogni sottoalbero porta con sé due profili, quello destro e quello sinistro, con una voce per ciascuna riga che occupa. Quando due sottoalberi fratelli vanno affiancati, il secondo scivola verso sinistra finché i due profili non si toccano da qualche parte, invece di partire dopo l'ingombro totale del primo.

La conseguenza voluta è esattamente quella richiesta: **un fratello senza figli ha un profilo alto una sola riga**, quindi vincola soltanto la propria riga e non riserva un centimetro in nessuna di quelle sottostanti. Il fratello successivo gli si accosta e fa scendere la propria discendenza sotto di lui. Nei figli di P005, per esempio, i quattro fratelli senza discendenza stanno alla distanza minima assoluta l'uno dall'altro e il primo di essi si affianca direttamente a un fratello che ha invece un ramo profondo.

Le distanze minime sono tarate sugli ornamenti del cartiglio, che sporgono 12,34 unità per lato: due caselle non possono avvicinarsi oltre ~25 unità senza che le volute si tocchino. Da qui **30 unità fra fratelli**, **52 fra rami diversi alle generazioni inferiori**, **240 fra alberi senza antenato comune**.

Il centraggio dei genitori sui figli è preservato: verificato su tutti i 108 genitori dell'albero, nessuno risulta fuori centro di più di mezza unità.

**Le linee di collegamento** non sono più tracciate a mano: per ogni genitore si disegna una verticale dal bordo inferiore della sua casella, una barra orizzontale che copre tutti i figli, e una verticale da quella barra a ciascuno di loro — lo schema classico "a squadra". I segmenti sono raggruppati per colore e disegnati in un tracciato per tinta, così l'intero albero costa dodici elementi grafici invece di migliaia. Quando una persona ha un testo nella colonna I, la barra viene spezzata in due tronconi e l'etichetta occupa il vuoto centrale.

**Grafica dei cartigli.** La cornice ornata è ricalcata curva per curva dal file `blocco.pdf` fornito da Matteo: volute agli angoli, cerchietti, montanti e filetti sottili esterni. Nome centrato in Fraunces, date in Newsreader, note in corsivo. Quando l'annotazione contiene le date del coniuge, il codice le stacca e le mette su una riga propria in fondo.

Due differenze rispetto al `blocco.pdf` originale: (1) i **quattro tratti diagonali corti** agli angoli della cornice sono stati rimossi su richiesta — il tracciato ora parte e finisce sui raccordi curvi, senza le codine oblique; (2) i colori non sono più fissi (`#fffff3` di fondo, `#603813` di cornice) ma vengono dal gruppo assegnato dalla colonna H, con il gruppo 0 che coincide esattamente con i colori originali.

**Comandi.** Barra unica in cima alla finestra dell'albero, centrata, forzata su una riga (scorre lateralmente se non ci sta). La finestra è un contenitore flex in colonna: barra fissa sopra, area che scorre sotto — così l'albero non passa mai sotto ai tasti e i pulsanti non si spostano col pinch. Il cassetto laterale e la velatura stanno **dentro** la finestra, altrimenti a schermo intero non si vedrebbero.

**Trappola in cui si è già caduti in passato:** riordinando il CSS a colpi di sostituzione di blocchi, sono sparite senza accorgersene delle regole intere (`#stage`/`#viewport`, poi `.btn`/`.field`), con effetti non ovvi (pan bloccato, sfondo nero a schermo intero, tasti senza stile). Prima di consegnare `index.html` va sempre verificato che il foglio di stile contenga tutte le regole attese e abbia le parentesi bilanciate.

**Verifiche fatte su questa versione** (403 persone reali): nessun ciclo, nessuna sovrapposizione tra caselle, tutte le 403 persone posizionate correttamente, inserimento testato di un fratello nuovo in un ramo con 9 figli già presenti (si è inserito cronologicamente al posto giusto) e verificato che una persona senza data non cambi mai posizione.
