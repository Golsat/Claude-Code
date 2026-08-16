# SmartLife AI Guides — Team di agenti Claude Code

Questo repo ospita un team di **subagenti Claude Code** dedicati alla collana
editoriale **SmartLife AI Guides** (SmartLife Publishing, ebook di prompt AI
su Amazon KDP, 10 volumi previsti, edizioni IT/EN). I file vivono in
`.claude/agents/` e vengono attivati automaticamente da Claude Code in base
alla richiesta — non serve invocarli per nome, basta chiedere in linguaggio
naturale ("scrivi il prossimo capitolo", "rivedi la bozza", "cerca i
competitor", "preparami la scheda KDP").

## Come funziona il team

Non è uno sciame autonomo: la sessione Claude Code fa da **orchestratore**,
richiama l'agente giusto per il compito, gli passa il contesto/output
dell'agente precedente e raccoglie i risultati. Il flusso tipico per un
nuovo capitolo/volume è:

```
researcher → writer → editor → marketing
(dati mercato) (bozza) (revisione onesta) (schede KDP)
```

Ogni passaggio è indipendente: puoi anche chiamare un solo agente per un
compito puntuale (es. solo l'editor su un capitolo già scritto).

## Gli agenti

| Agente | File | Cosa fa | Cosa NON fa |
|---|---|---|---|
| **smartlife-writer** | `smartlife-writer.md` | Scrive/continua capitoli e volumi secondo gli ANCORAGGIO di Drive; gestisce build EPUB/PDF/copertine | Non valuta la qualità del proprio lavoro in modo indipendente — quello è l'editor |
| **smartlife-editor** | `smartlife-editor.md` | Revisiona le bozze contro il manuale editoriale e la voce di collana; verdetto onesto pronto/da correggere | Non scrive contenuto nuovo, non riscrive silenziosamente le bozze |
| **smartlife-researcher** | `smartlife-researcher.md` | Ricerca competitor, keyword KDP, posizionamento di mercato (WebSearch/WebFetch) | Non decide prezzo/lancio — porta solo dati |
| **smartlife-marketing** | `smartlife-marketing.md` | Prepara schede KDP, descrizione, keyword, calendario promo | Non pubblica nulla — KDP non è raggiungibile da qui, output sempre da incollare a mano |

## Regola condivisa: checkpoint su Gianluca

Tutti e quattro gli agenti si fermano e chiedono conferma prima di decidere
su ciò che l'**ANCORAGGIO 3 - Decisioni e Strategia** (su Drive) classifica
come strategico: prezzo, KDP Select/DRM, data di lancio, via libera a un
nuovo volume, contenuti sensibili. Propongono opzioni con pro/contro, non
scelgono al posto tuo.

## Fonte di verità

Gli agenti non contengono lo stato del progetto — lo leggono ogni volta da
Google Drive, cartella **"SmartLife Publishing"**, dai file ANCORAGGIO 1-5.
Questo repo contiene solo i *ruoli* (chi fa cosa), non lo stato del progetto.

> **Nota (16/08/2026):** la struttura degli ANCORAGGIO su Drive è ancora
> quella storica (file versionati `_v8`, `_v9`, ecc., con stato duplicato tra
> ANCORAGGIO 1 e il "PROMPT per continuare"). È in programma uno snellimento
> (nomi fissi, split stato/mappa-file/storico, meno token per sessione) —
> non ancora eseguito su Drive alla data di questo file.
