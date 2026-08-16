# QuestForge Publishing — Team di agenti Claude Code

Questo repo ospita un team di **subagenti Claude Code** dedicati alla collana
editoriale **QuestForge Publishing** (nome di lavoro, da confermare), coloring
book fantasy a tema tabletop RPG per Amazon KDP, doppia edizione IT/EN. I file
vivono in `.claude/agents/` e si attivano automaticamente in base alla
richiesta — non serve invocarli per nome, basta chiedere in linguaggio
naturale ("cerca i competitor", "scrivi il filo narrativo del Vol.X",
"preparami i prompt immagine", "rivedi il volume", "preparami la scheda
KDP", "preparami dei post per il lancio").

> **Idea di origine e vincolo di copyright**: il progetto nasce come "coloring
> book a sfondo avventure D&D". Non può usare il marchio Dungeons & Dragons,
> i loghi Wizards of the Coast né le sue creature originali protette (fan
> content policy di WotC vieta la vendita non licenziata) — il prodotto è un
> coloring book fantasy/tabletop RPG **originale**. Dettaglio completo,
> regole bloccanti e cosa è invece sicuro: `⚓ ANCORAGGIO 6 - COPYRIGHT &
> COMPLIANCE` su Google Drive (cartella "QuestForge Publishing").

## Come funziona il team

Non è uno sciame autonomo: i subagenti non si chiamano tra loro. La sessione
Claude Code fa da **orchestratore** — richiama l'agente giusto per il
compito, gli passa il contesto/output dell'agente precedente e raccoglie i
risultati. Stesso schema di `SMARTLIFE-TEAM.md`: tu mantieni il controllo dei
checkpoint, io eseguo la sequenza.

## Il flusso standard — trigger "QUEST€"

Scrivere **"QUEST€"** avvia la produzione di un nuovo volume, con lo stesso
loop di revisione a tetto massimo di SmartLife (per evitare sia il "va bene
tutto" pigro sia il rimbalzo infinito):

```
1. [researcher]    Ricerca mercato, se non già fatta di recente         → report dati
2. [worldbuilder]  STEP 0: sinossi + story bible del volume (OGNI volume
                   ha un proprio storyline completo, non scene sparse con
                   un tema in comune), poi brief di scena per ogni tavola → brief
3. [illustrator]   Trasforma i brief in prompt immagine + style guide,
                   mantenendo protagonista/creature ricorrenti coerenti
                   con la story bible tavola dopo tavola               → prompt
      ↳ Gianluca genera le immagini FUORI da Claude Code (strumento
        scelto in ANCORAGGIO 3) e le carica su Google Drive, cartella
        del volume ("Vol.X/Tavole/") — MAI in chat: Claude legge PNG/
        JPEG direttamente da Drive
4. [editor]        Revisiona narrativa + prompt (+ tavole, se già generate)
      → verdetto PRONTO / DA CORREGGERE (gate compliance BLOCCANTE, vedi
        ANCORAGGIO 6 — un solo riferimento vietato = DA CORREGGERE, sempre;
        stesso peso per il "test storybook": le didascalie in fila devono
        leggersi come un racconto, non come scene scollegate)
      ↳ DA CORREGGERE → torna al passo 2 o 3 con le note dell'editor
      ↳ dopo 2 cicli senza arrivare a PRONTO → mi fermo e chiedo a te,
        non continuo a girare in loop da solo
5. [illustrator]   Solo dopo verdetto PRONTO e immagini ricevute: assembla
                   l'interno PDF e la copertina
6. [marketing]     Scheda KDP, keyword, calendario promo
7. [social]        Post di lancio e copy ads (bozze, in parallelo al punto 6)
8. Riepilogo finale a te, con evidenziati i checkpoint ANCORAGGIO 3
   (nome brand, prezzo, trim size, strumento immagini, lancio) da confermare
```

Per un compito puntuale non serve il flusso intero: puoi chiamare un solo
agente (es. solo l'editor su un brief già scritto, solo il social per una
campagna su un volume già live).

## Gli agenti

| Agente | File | Cosa fa | Cosa NON fa |
|---|---|---|---|
| **questforge-researcher** | `questforge-researcher.md` | Ricerca competitor, keyword KDP, posizionamento di mercato (WebSearch/WebFetch) | Non decide prezzo/lancio — porta solo dati |
| **questforge-worldbuilder** | `questforge-worldbuilder.md` | Mondo originale, filo narrativo, didascalie, brief di scena per ogni tavola | Non scrive il prompt tecnico di generazione immagine — quello è l'illustrator |
| **questforge-illustrator** | `questforge-illustrator.md` | Prompt immagine + style guide di coerenza visiva; assembla interno PDF/copertina una volta ricevute le immagini | Non genera immagini raster (nessun tool di image-gen in questa sessione) — produce solo i prompt per lo strumento esterno scelto |
| **questforge-editor** | `questforge-editor.md` | Revisiona narrativa, prompt e tavole; verdetto onesto pronto/da correggere; **gate copyright bloccante** | Non scrive contenuto nuovo, non decide strategia |
| **questforge-marketing** | `questforge-marketing.md` | Schede KDP, descrizione, keyword, calendario promo | Non pubblica nulla — KDP non è raggiungibile da qui |
| **questforge-social** | `questforge-social.md` | Post organici, copy ads, calendario editoriale social | Non pubblica sui social né spende budget — nessuna piattaforma è raggiungibile da qui |

## Differenze principali rispetto al team SmartLife

- **Ogni volume è uno storybook con un proprio storyline e un protagonista
  fisso** (decisione strutturale, ANCORAGGIO 4), non una raccolta di
  contenuti indipendenti con un tema in comune — a differenza dei 50
  prompt indipendenti (interscambiabili tra loro) di ogni volume SmartLife.
- **Prodotto fisico (paperback POD)**, non ebook: economics diverse
  (royalty 60% meno costo di stampa, non 70% fisso) — vedi ANCORAGGIO 2/3.
- **Contenuto visivo, non testuale**: c'è un agente in più
  (`questforge-illustrator`) e un anello umano obbligato nella pipeline
  (generazione immagini fuori da Claude Code).
- **Rischio IP molto più specifico e alto** (marchio Dungeons & Dragons):
  esiste un ANCORAGGIO 6 dedicato e un gate di compliance bloccante
  nell'editor — in SmartLife il vincolo di contenuto era solo "non citare la
  vita reale di Gianluca", qui è un vero rischio di violazione di marchio.
- **Decisioni chiave del Vol.1 ormai chiuse** (nome brand, formato, prezzo
  indicativo, strumento immagini) — vedi ANCORAGGIO 3. Restano aperte solo
  poche voci a basso impatto immediato (piano serie oltre il Vol.1, KDP
  Select). Gli agenti trattano le voci chiuse come date, non le rimettono
  in discussione ad ogni sessione.

## Regola condivisa: checkpoint su Gianluca

Tutti gli agenti si fermano e chiedono conferma prima di decidere su ciò che
**ANCORAGGIO 3 - Decisioni e Strategia** (su Drive) classifica come
strategico: nome brand definitivo, prezzo, trim size, numero pagine,
strumento di generazione immagini, data di lancio, budget pubblicitario, via
libera a un nuovo volume. Propongono opzioni con pro/contro, non scelgono al
posto tuo. In più, **tutti** applicano il gate di copyright di ANCORAGGIO 6
prima di proporre titoli, keyword o contenuti — non è negoziabile nemmeno
come "risparmio di tempo".

## Fonte di verità

Gli agenti non contengono lo stato del progetto — lo leggono ogni volta da
Google Drive, cartella **"QuestForge Publishing"**, dai file ANCORAGGIO 1-6.
Questo repo contiene solo i *ruoli* (chi fa cosa), non lo stato del progetto.

> **Policy formati/versioni** (decisa il 16/8/2026): su Drive si usano solo
> file `.md`/`.html` puri, mai Google Docs/Sheets nativi (più pesanti da
> leggere, conversione automatica altera la formattazione). Nessuna
> proliferazione di versioni nel nome file: ogni ancoraggio ha un nome
> fisso e si aggiorna sostituendolo (trash + ricreazione con lo stesso
> titolo), non creando `_v2`/`_v3`. Dettaglio in ANCORAGGIO 1.
