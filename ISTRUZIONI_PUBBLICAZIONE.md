# Albero genealogico Lizzi — istruzioni

Il file `index.html` è un sito statico completo. Legge i dati dal Google Sheet a ogni apertura della pagina, quindi si aggiorna da solo appena modifichi il foglio.

Disposizione e linee non sono coordinate fisse incorporate nel file, ma vengono **ricalcolate ogni volta** dai legami di parentela (`ID Padre` / `ID Madre`) e, per l'ordine dei fratelli, dalla data di nascita. Per aggiungere una persona — anche in mezzo a fratelli già esistenti — basta una riga nel foglio, senza mai toccare il codice. Il rovescio: l'albero non riproduce più il disegno a mano del PDF originale, ma una ricostruzione ordinata secondo la stessa logica.

---

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

### Le colonne

| Colonna | Contenuto |
|---|---|
| A — ID Persona | P001 … P403 (o oltre) — la chiave che collega ogni riga ai suoi genitori/figli. **Non modificarla dopo averla assegnata.** Ammette anche la convenzione a lettere `P115a`, `P115da` — vedi sezione 5bis |
| B — Nome | nome così com'è scritto nel disegno |
| C — Sesso (M/F) | dedotto dal nome; correggilo pure dove sbaglia |
| D — Data Nascita | determina anche **l'ordine fra fratelli** — vedi sezione 5 |
| E — Data Morte | come nel disegno |
| F — ID Padre | genitore. È il campo che decide dove finisce la casella nell'albero |
| G — Annotazioni | matrimoni, luoghi, note |
| **H — Colore** | una **X** apre un gruppo di colore per quella persona e **tutta la sua discendenza** — vedi sezione 6 |
| **I — Etichetta** | testo scritto sulla linea orizzontale sotto quella persona — vedi sezione 7 |

Il foglio deve restare condiviso con **"Chiunque abbia il link" → Visualizzatore** (pulsante Condividi). Senza questa impostazione il sito non riesce a leggere i dati.

Sono riconosciute anche, se le aggiungi in futuro: `Cognome`, `Luogo Nascita`, `Luogo Morte`, `Coniuge`, `ID Madre`, `Foto`.

---

## 2. Ricarica il sito su GitHub

1. Vai su `https://github.com/matliz80/albero-genealogico-lizzi/upload/main`
2. Trascina il nuovo `index.html` nel riquadro. **Verifica che il nome sia esattamente `index.html`**: se il browser l'ha salvato come `index (1).html`, rinominalo prima, altrimenti su GitHub finisce come file nuovo e il sito continua a mostrare la versione vecchia.
3. Scrivi due parole di descrizione e clicca il pulsante verde **Commit changes**. Senza quel click non viene caricato nulla.
4. Aspetta circa un minuto, poi ricarica il sito forzando lo svuotamento della cache: **Ctrl+F5** (Windows) o **Cmd+Shift+R** (Mac).

Per vedere quando la pubblicazione è finita davvero: `https://github.com/matliz80/albero-genealogico-lizzi/actions` — pallino giallo in corso, verde fatta, rosso errore.

Non serve un secondo repository per poter tornare indietro: GitHub conserva ogni versione nella cronologia (tab **History** del file). Per ripristinare basta aprire il commit precedente e cliccare **Restore this file**.

---

## 3. Funzioni del sito

Tutti i comandi stanno in **un'unica barra in cima alla finestra dell'albero**. L'albero comincia sotto la barra e non ci passa mai sopra.

- **Apertura** — la pagina si apre già a una scala leggibile (**50% su computer, 58% su cellulare**), centrata sulla **persona più anziana**, cioè quella con la data di nascita più antica. Non parte adattata all'intero albero, che a quella scala sarebbe illeggibile.
- **Cerca** — accetta **nomi e anni nella stessa riga**. `Matteo 1980` trova i Matteo nati o morti nel 1980; `Matteo` da solo li trova tutti; `1980` da solo trova chiunque sia nato o morto in quell'anno. Ogni gruppo di quattro cifre è trattato come anno e confrontato con nascita e morte; il resto è cercato nel testo (nome, annotazioni, luoghi, etichetta, ID). Devono corrispondere tutte le parole. Sotto la barra compare il numero di risultati.
- **Icona a quattro angoli** — adatta la vista a tutta la larghezza dell'albero.
- **− valore +** — il numero al centro è lo zoom in corso; cliccandolo torni al 100%. Su computer anche Ctrl+rotellina. **Su cellulare questi tre tasti non compaiono**: lo zoom si fa col pinch a due dita e lo spostamento trascinando con un dito.
- **Icona schermo intero** — cambia verso quando entri ed esci.
- **Icona a righe decrescenti** — cambia il modo di disegnare le caselle, ciclando su tre stati. Vedi sezione 8.
- **Icona a tre blocchi allineati** — cambia il criterio di disposizione. Vedi sezione 9.
- **Icona a tavolozza** — accende e spegne i colori dei gruppi.
- **Stampa** — apre il pannello per il plotter. Vedi sezione 10.
- **Click su una casella** — apre la scheda con tutti i dati.

Sotto la barra compaiono solo i **risultati della ricerca** e gli **errori** da correggere. Le segnalazioni non urgenti — per esempio l'elenco delle persone escluse perché senza legami — stanno in un **piede molto basso in fondo alla pagina**, che su cellulare non compare per non rubare spazio.

Le date sono mostrate come **gg.mm.aaaa** anche se nel foglio sono scritte in forma breve (5.3.1804 → 05.03.1804). I valori ambigui presenti nel disegno originale — «21 / 22.2.1720», «1650?», «a 6 mesi» — restano come sono, così si vedono e si possono correggere nel foglio.

---

## 4. Mettere a fuoco un ramo

Cliccando una casella e poi **Metti a fuoco questo ramo**, l'albero viene **ricostruito da zero con le sole persone del ramo**. Le altre non vengono sbiadite ma tolte, e le posizioni ricalcolate, così il ramo si legge alla larghezza che gli serve davvero.

Restano visibili:
- la persona scelta, **tutti i suoi antenati** risalendo e **tutti i suoi discendenti** scendendo;
- i suoi **fratelli** e i **fratelli di suo padre**, senza le rispettive discendenze.

Questi ultimi due gruppi servono a dare contesto: la sola linea diretta degli antenati, in fila uno sotto l'altro, fa perdere il senso di quanto fosse ampia ogni generazione. Mettendo a fuoco Matteo (P379), per esempio, compaiono 21 persone: lui, la figlia, il fratello Giuseppe, il padre Enea Giorgio con i suoi quattro fratelli, e su fino al capostipite.

Il pulsante **↩ Vista completa** ripristina tutto. Da qui si può anche esportare il PDF del solo ramo.

---

## 5. Aggiungere persone in futuro

**Prima di scrivere date a mano**, assicurati che le colonne **D** ed **E** siano formattate come testo: clicca sulla lettera D, tieni Ctrl (Cmd su Mac) e clicca sulla E, poi **Formato → Numero → Testo semplice**. Senza questo, scrivendo `4.04.2024` Google Sheets lo legge come una durata (4 ore, 4 minuti, 2024 secondi) e lo trasforma in `4.37.44`. In alternativa, per una cella singola, scrivi un apostrofo davanti: `'4.04.2024` — non si vede nel foglio e la data resta intatta.

Aggiungi poi una riga con un ID nuovo, il nome, la **data di nascita** (anche solo l'anno) e l'**`ID Padre`**. Esempio — un fratello dimenticato di P005, nato nel 1650:

| ID Persona | Nome | Sesso | Data Nascita | Data Morte | ID Padre |
|---|---|---|---|---|---|
| P404 | Osvaldo | M | 1650 | | P005 |

**Come si posiziona.** Il sito ordina automaticamente i figli di uno stesso genitore per data di nascita. Il nuovo Osvaldo si inserisce da solo fra il fratello del 1648 e quello del 1651, spostando chi viene dopo. Nessuna coordinata da scrivere.

**Chi non ha una data leggibile** (o ce l'ha ambigua come «1650?») **non viene mai spostato**: resta fermo dove compare nel foglio. Vale sia per chi c'è già sia per chi aggiungi.

Funziona a catena su tutte le generazioni: aggiungere un figlio ricalcola solo il ramo sotto di lui, il resto dell'albero resta dov'era.

---

## 5bis. La convenzione a lettere negli ID

Chi compila il foglio può indicare la parentela **dentro l'ID stesso**, senza compilare la colonna ID Padre:

- i figli di `P115` si chiamano `P115a`, `P115b`, `P115c`…
- i figli di `P115d` si chiamano `P115da`, `P115db`, `P115dc`…
- i figli di `P115dc` si chiamano `P115dca`, `P115dcb`… e così via, senza limite di profondità

Il sito ricava il genitore **togliendo l'ultima lettera** dall'ID e cercando la persona che resta. Un ID che finisce con una cifra (`P115`, `P403`) non viene toccato: è un ID normale.

**Regole di precedenza**, nell'ordine:
1. Se la colonna **ID Padre è compilata** e punta a una persona esistente, vince quella — sempre, anche se l'ID avrebbe suggerito un altro genitore.
2. Se è vuota, oppure punta a un ID che non esiste, il sito prova a dedurre il genitore dalle lettere finali.
3. Se nemmeno così trova un genitore, la persona resta una radice separata.

**Tolleranze.** Maiuscole, minuscole e spazi non contano: `p115 f` viene agganciato a `P115` esattamente come `P115F`. Serve perché il foglio è compilato da più persone.

**Ordine fra fratelli.** Chi ha una data di nascita si dispone cronologicamente come sempre. Chi non ce l'ha viene ordinato **per ID**, così `P115a`, `P115b`, `P115c` restano in sequenza anche se nel foglio sono stati inseriti alla rinfusa.

**Avviso automatico.** Se qualcuno scrive un ID Padre che non corrisponde a nessuna persona — un refuso tipo `P1115db` al posto di `P115db` — sotto la barra dei comandi compare un avviso **in rosso** con il numero di righe interessate e i primi ID sbagliati. È il modo più rapido per accorgersi di una riga scollegata senza doverla cercare a occhio nell'albero. Quando non ci sono errori, la stessa riga mostra in colore normale l'elenco delle caselle isolate escluse dal disegno (sezione 5ter).

---

## 5ter. Caselle isolate: non vengono disegnate

Una persona che **non ha un genitore riconosciuto e non ha nemmeno figli** è una casella isolata: nel disegno galleggerebbe staccata da tutto, senza una sola linea che la colleghi al resto dell'albero. Il sito non la disegna.

Restano invece **le radici vere**, cioè chi non ha genitore ma ha una discendenza: sono i capostipiti dei rami e vanno mostrati. Nei dati attuali questo significa che P001, P107 e P109 continuano a comparire con tutti i loro figli, mentre otto persone spariscono:

| ID | Nome |
|---|---|
| P105 | Domenica |
| P106 | Anna |
| P108 | Valentina |
| P110 | Sebastiana |
| P111 | Valentino |
| P112 | Bernardo |
| P113 | Bernardo |
| P239 | Giuseppe |

**Non sono state cancellate**: restano nel foglio, e riappaiono da sole appena si scrive il loro `ID Padre` (o appena qualcuno le indica come genitore). Sotto la barra dei comandi compare sempre una riga che le elenca, così non si dimentica che ci sono.

Se in futuro tutte le persone risultassero isolate — caso che può capitare solo con un foglio rotto — il sito le mostra comunque, per non presentare una pagina vuota.

---

## 6. Colorare i rami (colonna H)

Una **X** nella colonna H apre un nuovo gruppo di colore che vale per quella persona e **tutti i suoi discendenti** — figli, nipoti, pronipoti — fermandosi dove un discendente ha a sua volta una X, che apre il gruppo successivo.

**Una X su chi non ha figli viene ignorata.** Aprirebbe un gruppo composto da una persona sola, che si staccherebbe dal colore dei fratelli senza un motivo leggibile: era il caso di Anna 1656 e di Ferdinando 1857, che comparivano di un colore diverso dai propri fratelli. In quei casi la persona eredita il colore del proprio ramo. Se una X sembra non fare effetto, il motivo è quasi sempre questo: va spostata sulla persona da cui il ramo si stacca davvero.

> **Attenzione, questa regola è cambiata.** In una versione precedente il gruppo seguiva l'ordine delle righe nel foglio, e finiva per colorare anche persone di altri rami che capitavano in mezzo. Ora segue **la discendenza**: chi arriva da un altro ramo non viene toccato, anche se nel foglio sta fra righe marcate o nel disegno finisce sulla stessa riga.

Le tinte sono **dodici, scure**, scelte distanti fra loro in tinta e simili in luminosità: bruno, verde bosco, terra bruciata, blu notte, prugna, oliva, ottanio, indaco, mattone, muschio, ambra, violetto. Oltre la dodicesima ricominciano da capo.

Il colore si applica a piena intensità a **linee di collegamento, cornice e nome**, e **diluito al 10%** allo sfondo della casella. Così ogni ramo si riconosce da lontano, ma il testo mantiene quasi tutto il contrasto e la carta non risulta sporcata sui grandi formati.

Sulle linee: il tronco che scende dal genitore e la barra orizzontale prendono il colore del **genitore**; la calata verso ciascun figlio prende quello del **figlio**. Così il punto in cui comincia un gruppo nuovo si vede a colpo d'occhio.

L'icona a tavolozza spegne e riaccende i colori senza toccare il foglio.

---

## 7. Etichette sulle linee (colonna I)

Il testo scritto nella colonna I di una persona viene stampato **sulla barra orizzontale sotto la sua casella**, quella da cui scendono i figli. La barra si interrompe e il testo si centra nel vuoto, in **maiuscolo** e in **Cinzel bold**, un serif lapidario adatto alle diciture d'insieme.

Serve a dare un nome ai rami: «CAPORIACCO», «I TRESEMAN», «RAMO DI RAGOGNA». Scrivi pure in minuscolo, il sito converte da solo.

Il corpo si adatta: parte da 46 e scende fino a 20 se la barra è corta, così l'etichetta non sborda mai oltre i figli. Se la persona ha **un solo figlio** non c'è una barra su cui scrivere, quindi l'etichetta va in corpo ridotto accanto alla calata. Se non ha figli, viene ignorata.

Le etichette rientrano anche nella ricerca.

---

## 8. Tre modi di disegnare le caselle

L'icona con le **righe decrescenti** cicla su tre stati.

**1 — Cartiglio completo.** Cornice ornata ricalcata dal `blocco.pdf`, nome, entrambe le date, annotazioni, date del coniuge. È la vista più ricca, adatta a leggere da vicino. Casella 292 × 219.

**2 — Nome e anno, orizzontali.** Nessuna cornice, solo fondo colorato: nome grande in corpo 56 e data di nascita a capo in corpo 40. La larghezza si adatta al nome, i nomi oltre 12 caratteri vengono accorciati con i puntini. Serve a leggere nomi e date **a zoom basso**, quando il cartiglio diventa un francobollo illeggibile.

**3 — Nome e anno, verticali.** Come sopra ma con il testo che corre dal basso verso l'alto. Siccome la larghezza non dipende più dalla lunghezza del nome, la casella resta larga 118 unità sempre. Tutte le righe usano la stessa altezza, ricavata dal nome più lungo, così le generazioni restano allineate.

Il verticale è di gran lunga il più efficiente per la stampa — vedi la tabella nella sezione 10. Il prezzo è che leggere nomi ruotati è meno immediato che scorrerli in orizzontale; per un telo da appendere, dove ci si avvicina a leggere una casella per volta, è però una scelta ragionevole.

---

## 9. Due modi di disporre l'albero

L'icona con i **tre blocchi allineati** commuta fra due criteri.

**Disposizione centrata** (quella di partenza). Ogni genitore sta esattamente al centro dei propri figli, e i sottoalberi si incastrano fra loro sfruttando gli spazi vuoti. È la lettura genealogica classica, dove la simmetria aiuta a seguire le discendenze.

**Fratelli sempre affiancati.** I fratelli si toccano sempre, qualunque discendenza abbiano: ogni riga si riempie da sinistra a destra senza lasciare vuoti. Il genitore non è più centrato sui figli — sono **le linee di giunzione a spostarsi** per raggiungerli. Si perde la simmetria ma si guadagna circa il **40% di larghezza**.

Per rendere leggibile questa vista, dove le linee corrono lunghe e i genitori non stanno sopra ai figli, il sito applica tre accorgimenti che nella vista centrata non servono e **non compaiono**:
- le barre orizzontali di famiglie diverse sono **sfalsate su tre livelli** invece di cadere tutte alla stessa quota
- ogni figlio ha un **trattino di aggancio** sul bordo superiore, che rende evidente dove arriva la calata
- le famiglie adiacenti hanno il fondo **alternato leggermente più carico**, a bande, per seguire i gruppi di fratelli

Nessuna delle due modalità produce sovrapposizioni: in tutte le combinazioni la distanza minima fra due caselle resta di 30 unità, verificata sulle 403 persone.

---

## 10. Stampa su plotter

Il pulsante **Stampa** apre un pannello con due regolazioni e quattro misure aggiornate dal vivo.

**Come funziona la geometria.** L'albero è largo e basso. Fissata l'altezza del telo, la lunghezza è una conseguenza automatica del rapporto fra larghezza e altezza complessive. Cambiando modalità cambia radicalmente il risultato:

| Caselle | Disposizione | Telo lungo | Casella | Corpo nome |
|---|---|---|---|---|
| Cartiglio | centrata | 9,6 m | 5,7 × 4,2 cm | 5,4 mm |
| Cartiglio | affiancata | 5,7 m | 5,7 × 4,2 cm | 5,4 mm |
| Nome+anno orizzontale | centrata | 11,6 m | 7,0 × 3,7 cm | 13,7 mm |
| Nome+anno orizzontale | affiancata | 7,2 m | 7,0 × 3,7 cm | 13,7 mm |
| **Nome+anno verticale** | centrata | **2,4 m** | 1,2 × 5,2 cm | 5,4 mm |
| **Nome+anno verticale** | **affiancata** | **1,6 m** | 1,2 × 5,2 cm | 5,4 mm |

Valori con telo alto 1 m e distanza fra generazioni al valore iniziale.

Nota controintuitiva: la vista **orizzontale è più lunga** del cartiglio, non più corta. Le caselle sono molto più basse (152 contro 219), quindi a parità di altezza del telo la scala si riduce e la lunghezza cresce. Serve a leggere a schermo, non ad accorciare il plottaggio. Per quello c'è la verticale.

**I due comandi:**
- **Altezza del telo (cm)** — la misura utile del rotolo. Cambiandola cambiano proporzionalmente lunghezza e dimensione delle caselle.
- **Distanza fra le generazioni** — allontanando le righe l'albero diventa proporzionalmente meno lungo a parità di altezza, ma le caselle rimpiccioliscono.

Il riquadro delle misure diventa rosso quando il corpo del nome scende **sotto i 2 mm**, soglia oltre la quale la stampa non è comodamente leggibile. **Ripristina valori iniziali** riporta tutto ai valori di partenza.

**Consiglio pratico:** prima di lanciare l'intero telo, stampa una striscia di prova di 1-2 m con le impostazioni scelte, per verificare dal vivo la leggibilità sul supporto che userai. La resa cambia molto fra carta opaca e materiale lucido.

---

## 11. Chi può modificare i dati

Non serve un login sul sito: invita le persone direttamente sul Google Sheet come **Editor** (Condividi → email → ruolo Editor). Le modifiche compaiono sul sito al ricaricamento della pagina.

## 12. Quale scheda del foglio viene letta

Il foglio contiene **più schede** (in fondo alla finestra: `Albero`, `copia funzionante`, `nuovo`). Senza indicazioni il sito leggerebbe sempre la **prima**, che non è detto sia quella aggiornata: è un modo classico di perdere ore chiedendosi perché le modifiche non compaiono.

La scheda da leggere è indicata da questa riga vicino all'inizio dello `<script>` in `index.html`:

```js
const SHEET_GID = "2066415243";
```

**Come trovare il numero giusto.** Apri il foglio, clicca sulla scheda che vuoi pubblicare e guarda l'indirizzo nella barra del browser: finisce con `gid=` seguito da un numero. Quello è il valore da mettere fra virgolette. Mettendo `""` il sito torna a leggere la prima scheda.

⚠️ **Da verificare:** il numero attualmente impostato è quello della scheda che stavi guardando quando mi hai mandato il link. Se le persone nuove le state aggiungendo su una scheda diversa, va cambiato, altrimenti il sito continuerà a mostrare dati vecchi.

Per cambiare foglio intero, invece, si modifica la riga sopra:

```js
const SHEET_ID = "12tz3Vr_gPEchYxVHoF8Ot9IqC1zvfYYdD8I2RvlWdXk";
```

sostituendo l'ID con quello del nuovo foglio (si trova nell'URL fra `/d/` e `/edit`).

### Come vengono riconosciute le colonne

Il sito non va a posizione fissa: legge le **intestazioni** della prima riga e le riconosce dalle parole che contengono. `ID Padre` → colonna del padre, `Data Nascita` → nascita, `nuovo colore da qui` → gruppi di colore, `Nome ramo` → etichetta sulla barra.

Due regole di sicurezza, imparate a spese nostre:
- Le voci **più specifiche vengono controllate prima** delle più generiche. L'intestazione reale della colonna I è `Nome ramo`, che contiene la parola «nome»: senza questa precedenza verrebbe scambiata per la colonna del nome della persona e lo sovrascriverebbe, svuotandolo. Il risultato era che **il sito non disegnava più nulla**.
- Se due colonne finiscono comunque sullo stesso campo, **vince la prima** e le successive vengono ignorate, invece di cancellarla riga per riga.

Se il foglio non produce nessuna riga utilizzabile, il messaggio d'errore sotto la barra ora elenca le intestazioni trovate, così si vede subito cosa non è stato riconosciuto.

---

### Nota sulla provenienza dei dati

Nomi, date e annotazioni sono stati estratti dal testo vettoriale del PDF `albero exp3.pdf` (nessuna lettura "a occhio"): il testo è identico all'originale, refusi compresi. Esempio: «Nicolò n. 2.11.1611» compare così nel disegno pur essendo fratello di persone nate dal 1711 al 1718 — è un errore del documento di partenza, non della trascrizione.

I legami di parentela sono stati ricostruiti dal disegno originale e restano quelli: quello che è cambiato è solo **come vengono disegnate** le caselle, non i legami stessi.

**11 persone non hanno un `ID Padre`** (nessuna linea verso l'alto nel PDF, verificato ingrandendo il documento) e restano "radici" separate finché non si completa il dato a mano:

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

Da P105 a P113 sono nove fratelli: la loro barra orizzontale nel disegno finisce senza risalire a nessuna casella. Basta scrivere l'ID del padre nella riga P105 e ripeterlo sulle altre otto: al ricaricamento il gruppo si aggancia automaticamente al resto dell'albero, nella posizione cronologicamente corretta.

Nel frattempo, di queste undici, **le otto che non hanno nemmeno figli non vengono disegnate** perché resterebbero caselle isolate (sezione 5ter). Restano visibili P001, P107 e P109, che una discendenza ce l'hanno.

**Tre date incomplete già nel disegno**, lasciate come sono perché mancano cifre:

| ID | Valore nel disegno | Nota |
|---|---|---|
| P072 | `8.3.171 0` | anno spezzato, presumibilmente 1710 |
| P207 | `sposato il 6.2.187 con Giovanni Beinat` | anno di tre cifre |
| P284 | `3.0.6.1907` | un punto di troppo |

---

### Come è fatto il sito, per chi ci mette mano

**Struttura.** Un solo `index.html`, senza compilazione né dipendenze. Dentro, nell'ordine: CSS, corpo della pagina, e uno `<script>` con lettura del CSV, motore di layout, disegno delle caselle e comandi. Non esiste l'oggetto `GEO` con le coordinate incorporate: nessun file da rigenerare quando cambiano i dati.

**Motore di layout.** Per ogni persona si individua il genitore (ID Padre, o ID Madre se manca) e si costruisce l'albero dei discendenti. Dentro ogni gruppo di fratelli, chi ha una data leggibile (si estrae il primo anno plausibile 1300–2099 dalla stringa, anche sporca) viene riordinato cronologicamente **nelle posizioni già occupate da fratelli con data**; chi non ha data resta fermo. La posizione verticale è la generazione per il passo fra le righe. Chi non ha genitore riconosciuto diventa radice indipendente. Se un errore nel foglio creasse un ciclo, il collegamento viene tagliato automaticamente invece di bloccare la pagina.

**Il packing orizzontale, la parte delicata.** La prima versione accodava i sottoalberi uno dopo l'altro: ogni persona senza figli si prendeva una colonna intera e ogni ramo riservava tutto il proprio ingombro, anche dove era vuoto.

La versione attuale usa un **algoritmo a contorni** (famiglia Reingold–Tilford). Ogni sottoalbero porta con sé due profili, destro e sinistro, con una voce per ciascuna riga occupata. Affiancando due sottoalberi fratelli, il secondo scivola a sinistra finché i profili non si toccano da qualche parte, invece di partire dopo l'ingombro totale del primo.

Conseguenza voluta: **un fratello senza figli ha un profilo alto una riga sola**, quindi vincola solo la propria riga e non riserva spazio in quelle sottostanti. Nei figli di P005 i quattro fratelli senza discendenza stanno alla distanza minima assoluta e il primo si affianca a un fratello con un ramo profondo. Il risultato è stato **53% di larghezza in meno** rispetto all'accodamento.

I profili salvano il **bordo destro reale**, non il bordo sinistro più una larghezza fissa: serve perché nelle viste semplificate ogni casella ha la propria larghezza.

Le distanze minime sono tarate sugli ornamenti del cartiglio, che sporgono 12,34 unità per lato: due caselle non possono avvicinarsi oltre ~25 unità senza che le volute si tocchino. Da qui **30 unità fra fratelli**, **52 fra rami diversi alle generazioni inferiori**, **240 fra alberi senza antenato comune**.

**Caratteri.** I **nomi** sono in **Cormorant** (700), un garaldo di impronta rinascimentale coerente con l'origine cinquecentesca del documento. **Numeri e testi** — date, annotazioni, interfaccia, misure di stampa — sono in **DM Sans** (500), una grottesca dalle cifre ampie e leggibili anche a corpo piccolo e in stampa grande.

Cormorant è sensibilmente più stretto e otticamente più piccolo dei caratteri precedenti, quindi corpi e stime di larghezza sono stati ricalibrati: il nome sul cartiglio passa da 28 a 34, la stima di larghezza per carattere da 0,545 a 0,455 em, e i nomi accorciati nella vista essenziale da 12 a 14 caratteri. Senza questa ricalibratura le caselle sarebbero risultate troppo larghe e i nomi troppo piccoli.

**Colore dei testi.** Tutto il contenuto del cartiglio — nome, date, annotazioni, date del coniuge — usa **la tinta del ramo di appartenenza**, con le parti secondarie leggermente trasparenti per mantenere la gerarchia di lettura. In precedenza date e note erano fissate su due marroni indipendenti dal ramo, che stonavano contro i gruppi blu, verdi o violetti. Perché la tinta arrivi al disegno, le regole CSS che fissavano `fill` su `.date` e `.note-text` sono state rimosse: un attributo di presentazione SVG perde sempre contro una regola CSS.

**Comandi.** Nell'intestazione resta il solo campo di ricerca, allineato a sinistra accanto al titolo. Tutte le funzioni sono icone in una **colonna verticale sul lato destro** del disegno, raggruppate in tre blocchi separati da filetti: vista e ingrandimento, modo di disegno, stampa e ritorno. Le icone sono state ridisegnate sulla stessa impostazione — griglia 24, tratto 1,6, estremità arrotondate, nessun riempimento, forme costruite su cerchi e rette — e i pulsanti hanno angoli arrotondati (13 px) con il campo di ricerca a pillola.

**Margine superiore.** Sopra la prima generazione ci sono 130 unità di spazio: senza, il cartiglio del capostipite resta incollato al bordo del disegno e in stampa tocca la cimosa del telo.

**Grafica del cartiglio.** Cornice ricalcata curva per curva dal `blocco.pdf`: volute agli angoli, cerchietti, montanti, filetti sottili esterni. Due differenze dall'originale: i **quattro tratti diagonali corti** agli angoli sono stati rimossi su richiesta, e i colori non sono fissi ma vengono dal gruppo della colonna H (il gruppo 0 coincide con i colori originali).

**Le linee** non sono tracciate a mano: per ogni genitore una verticale dal bordo inferiore, una barra orizzontale sui figli, una verticale a ciascuno. I segmenti sono raggruppati per colore in un tracciato per tinta, così l'intero albero costa dodici elementi grafici invece di migliaia. La barra si allunga fino a raggiungere la verticale del genitore, cosa necessaria nella vista affiancata dove il genitore non è centrato.

**Trappola in cui si è già caduti:** riordinando il CSS a colpi di sostituzione di blocchi sono sparite senza accorgersene regole intere (`#stage`/`#viewport`, poi `.btn`/`.field`), con effetti non ovvi (pan bloccato, sfondo nero a schermo intero, tasti senza stile). Prima di consegnare va sempre verificato che il foglio di stile contenga tutte le regole attese e abbia le parentesi bilanciate. Attenzione anche all'opposto: la regola `.card-bg{fill:#fffff3}` **doveva** essere rimossa, perché un attributo di presentazione SVG perde sempre contro una regola CSS e avrebbe annullato la tinta del gruppo.

**Verifiche fatte su questa versione** (403 persone reali, tutte e sei le combinazioni modalità × disposizione): nessun ciclo, nessuna sovrapposizione, distanza minima 30 unità ovunque, tutti i 108 genitori centrati sui figli entro mezza unità nella disposizione centrata, righe allineate nella vista verticale, colore che segue correttamente la discendenza e non l'ordine delle righe.
