# SmartLife AI Guides — Team di agenti Claude Code

Questo repo ospita un team di **subagenti Claude Code** dedicati alla collana
editoriale **SmartLife AI Guides** (SmartLife Publishing, ebook di prompt AI
su Amazon KDP, 10 volumi previsti, edizioni IT/EN). I file vivono in
`.claude/agents/` e vengono attivati automaticamente da Claude Code in base
alla richiesta — non serve invocarli per nome, basta chiedere in linguaggio
naturale ("scrivi il prossimo capitolo", "rivedi la bozza", "cerca i
competitor", "preparami la scheda KDP", "preparami dei post per il lancio").

## Come funziona il team

Non è uno sciame autonomo: i subagenti non si chiamano tra loro. La sessione
Claude Code fa da **orchestratore** — richiama l'agente giusto per il
compito, gli passa il contesto/output dell'agente precedente e raccoglie i
risultati. Questo tiene il sistema prevedibile: tu mantieni il controllo dei
checkpoint, io eseguo la sequenza.

## Il flusso standard — trigger "CLAUD€"

Scrivere **"CLAUD€"** avvia la produzione di un nuovo volume: io eseguo
l'intera sequenza sotto senza che tu debba invocare ogni agente a mano.
Include un **loop di revisione con tetto massimo**, per evitare sia il "va
bene tutto" pigro sia il rimbalzo infinito tra writer ed editor:

```
1. [researcher]    Ricerca mercato, se non già fatta di recente   → report dati
2. [writer]        Scrive la bozza del capitolo/volume            → bozza
3. [editor]        Revisiona struttura/voce/KDP → PRONTO / DA CORREGGERE
      ↳ DA CORREGGERE → torna al passo 2 con le note dell'editor
      ↳ dopo 2 cicli senza arrivare a PRONTO → mi fermo e chiedo a te,
        non continuo a girare in loop da solo
4. [psychologist]  Solo dopo PRONTO: verifica impatto psicologico → OK / DA RIVEDERE
      ↳ DA RIVEDERE → torna al passo 2 con le note (poi di nuovo editor+psychologist)
      ↳ tema che va oltre "il tono va aggiustato" → si ferma, apre un file in
        "🔍 CONTROLLO GIANLUCA", non prosegue senza il tuo ok
5. [marketing]     Solo dopo PRONTO + OK: scheda KDP, keyword, promo KDP
6. [social]        Post di lancio e copy ads (bozze, in parallelo al punto 5)
7. Riepilogo finale a te, con evidenziati i checkpoint ANCORAGGIO 3
   (prezzo, lancio, DRM, budget ads) da confermare
```

Per un compito puntuale non serve il flusso intero: puoi chiamare un solo
agente (es. solo l'editor su un capitolo già scritto, solo il social per una
campagna su un volume già live).

## Gli agenti

| Agente | File | Cosa fa | Cosa NON fa |
|---|---|---|---|
| **smartlife-writer** | `smartlife-writer.md` | Scrive/continua capitoli e volumi secondo gli ANCORAGGIO di Drive; gestisce build EPUB/PDF/copertine | Non valuta la qualità del proprio lavoro in modo indipendente — quello è l'editor |
| **smartlife-editor** | `smartlife-editor.md` | Revisiona le bozze contro il manuale editoriale e la voce di collana; verdetto onesto pronto/da correggere | Non scrive contenuto nuovo, non riscrive silenziosamente le bozze |
| **smartlife-psychologist** | `smartlife-psychologist.md` | Verifica impatto psicologico/emotivo (dopo l'editor): colpevolizzazione, riposo "produttivizzato", minimizzazione, AI come sostituto di supporto reale | Non dà consigli clinici, non decide se un capitolo va escluso — quello è di Gianluca |
| **smartlife-researcher** | `smartlife-researcher.md` | Ricerca competitor, keyword KDP, posizionamento di mercato (WebSearch/WebFetch) | Non decide prezzo/lancio — porta solo dati |
| **smartlife-marketing** | `smartlife-marketing.md` | Prepara schede KDP, descrizione, keyword, calendario promo KDP | Non pubblica nulla — KDP non è raggiungibile da qui, output sempre da incollare a mano |
| **smartlife-social** | `smartlife-social.md` | Post organici, copy ads (Amazon Ads/Meta Ads), calendario editoriale social | Non pubblica sui social né spende budget — nessuna piattaforma è raggiungibile da qui |

## Regola condivisa: checkpoint su Gianluca

Tutti e sei gli agenti si fermano e chiedono conferma prima di decidere
su ciò che l'**ANCORAGGIO 3 - Decisioni e Strategia** (su Drive) classifica
come strategico: prezzo, KDP Select/DRM, data di lancio, budget pubblicitario,
via libera a un nuovo volume, contenuti sensibili. Propongono opzioni con
pro/contro, non scelgono al posto tuo.

## Fonte di verità

Gli agenti non contengono lo stato del progetto — lo leggono ogni volta da
Google Drive, cartella **"SmartLife Publishing"**, dai file ANCORAGGIO 1-7.
Questo repo contiene solo i *ruoli* (chi fa cosa), non lo stato del progetto.

> **Nota:** gli ANCORAGGIO su Drive sono stati ristrutturati per ridurre il
> consumo di token per sessione — nomi fissi (una sola copia viva per nome,
> le sostituite vanno subito in OLD_ARCHIVIO), ANCORAGGIO 1 accorciato al solo
> stato corrente, mappa file/ID e storico tecnico separati in ANCORAGGIO 6 e 7.
