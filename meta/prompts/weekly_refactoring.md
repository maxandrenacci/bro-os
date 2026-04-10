# Weekly Refactoring — Prompt for Bro

> **How to use:** every Friday afternoon, ~20-30 min. Run `/weekly` in Claude Code at vault root.
> Set a recurring Google Calendar reminder Fridays at 16:00.
>
> **Mode:** Bro proposes, user approves. Nothing destructive happens without explicit OK.

---

## PROMPT (copy from here)

Bro, è la sessione di weekly refactoring di bro-os.

Prima di iniziare, leggi nell'ordine:
1. `CLAUDE.md` (root)
2. `meta/memory.md`
3. `meta/taxonomy.md`
4. `meta/usage_log.md` (ultime 4 settimane)

Poi esegui le sezioni qui sotto **in ordine**. Per ogni sezione, **proponi le modifiche, non eseguirle**. Aspetta il mio "ok" esplicito (o approvazione selettiva) prima di applicare.

---

### SEZIONE 1 — Consolidamento degli APPEND

Scansiona tutti i file in `/notes/` cercando i marker `<!-- APPEND ... -->`.

Per ogni file con append accumulati nella settimana:
1. Identifica il "core" pulito del file (la prosa pre-append)
2. Identifica i nuovi contenuti negli append
3. **Proponi una versione refactored** che integra i nuovi contenuti nel core, mantenendo:
   - Le fonti originali come `[[link]]` inline
   - Il tono del file originale
   - La struttura delle sezioni
   - Tutti i fatti, anche quelli minori
4. **Non scrivere ancora** la versione refactored. Mostra a user:
   - Il file vecchio (collassato, solo titoli sezione + count append)
   - La proposta nuova
   - Cosa cambia in sintesi (1 frase)
5. Aspetta approvazione file per file (o batch approval se user lo chiede).

---

### SEZIONE 2 — Deduplicazione

Identifica file `/notes/` che potrebbero essere duplicati o mergeable:
- Stesso concept con nomi leggermente diversi
- File piccoli che dovrebbero essere sezioni di un file più grande
- File che si sovrappongono significativamente per contenuto

Proponi merge **uno per uno** con: file A, file B, motivo del merge, contenuto risultante. Aspetta OK.

---

### SEZIONE 3 — Splitting

Identifica file `/notes/` che dovrebbero essere splittati:
- File > ~500 righe (soft indicator) → valuta criticamente
- File > ~2000 righe (hard ceiling) → quasi sempre split
- File che trattano più di un concept (test: ha sotto-sezioni che vengono citate standalone?)

Proponi split **uno per uno** con: file originale, nuovi file proposti, criterio di split. Aspetta OK.

---

### SEZIONE 4 — Link & MOC

1. **Orphan notes:** identifica file `/notes/` senza link entranti né uscenti. Per ognuno:
   - Sono ancora rilevanti? Se sì, propone almeno 2 link plausibili.
   - Se non rilevanti, propone archiviazione.

2. **Missing bidirectional links:** identifica casi in cui A linka B ma B non linka A. Proponi rinforzo.

3. **MOC creation:** identifica tag che hanno accumulato massa critica (>15 file) ma nessun MOC. Proponi creazione di MOC tematici. Per ogni MOC proposto:
   - Nome file
   - Frontmatter
   - Struttura: snapshot + per-tema + decisioni recenti + link principali
   - Aspetta OK prima di crearlo.

---

### SEZIONE 5 — Taxonomy review

Leggi `meta/taxonomy.md`. Per ogni theme tag:

1. **Sotto-usati** (<3 file dopo 2 settimane di vita) → proponi eliminazione o assorbimento in un tag esistente
2. **Sovraffollati** (>30 file) → proponi introduzione di sotto-tag specifici, con evidenza empirica del bisogno
3. **Semanticamente sovrapposti** → proponi merge
4. **Mancanti** (concetti ricorrenti senza tag dedicato) → proponi introduzione

Aggiorna `meta/taxonomy.md` con tutte le modifiche approvate. Riporta:
- Tag aggiunti
- Tag rimossi
- Tag mergeati
- Conteggi aggiornati

---

### SEZIONE 6 — Meta-review delle regole di sistema

Questa sezione è critica. Valuta empiricamente, sulla base del vault corrente:

1. **Soglie di sizing dei file** (~500 soft, ~2000 hard, 3-min readability):
   - Sono ancora sensate dato come è cresciuto il vault?
   - I file che ho splittato erano effettivamente troppo grandi, o ho splittato troppo presto?
   - Ci sono pattern che suggeriscono nuove soglie?

2. **Criteri di atomicità:**
   - Stanno producendo file della giusta granularità?
   - C'è un eccesso di file piccoli che si sarebbero gestiti meglio insieme?

3. **Tag taxonomy:**
   - Sta convergendo (pochi tag stabili) o divergendo (proliferazione)?
   - L'approccio "domini larghi prima" sta funzionando?

4. **Pipeline daily processing:**
   - Il numero medio di domande per inbox file è sostenibile?
   - L'auto-enrichment funziona o Bro chiede troppo / troppo poco?

Per ogni aspetto, **se hai una proposta di aggiustamento**, formulala così:
- **Cosa cambiare:** [specifico]
- **Evidenza dal vault:** [dati concreti, non opinioni]
- **Impatto atteso:** [cosa migliora]
- **Cosa modificare in CLAUDE.md:** [paragrafo specifico]

user approva o respinge ogni proposta singolarmente. Le approvate vengono applicate al `CLAUDE.md`.

---

### SEZIONE 7 — Resources health check

Scansiona `/resources/` e tutti i file `__meta.md` al suo interno:

1. **Orphan artifacts:** file binari (PDF, DOCX) senza `__meta.md` gemello → segnala come violazione hard rule. Proponi creazione immediata del meta-file o rimozione.
2. **Dead meta-files:** `__meta.md` che puntano a un `artifact` non più esistente → flagga.
3. **Unused resources:** resources mai referenziate da nessuna nota in `/notes` dopo >4 settimane dalla creazione. Valuta: è ancora utile? Va archiviata? O va creata la nota derivata mancante?
4. **Stale status:** resources con `status: in_force` o `status: active` che potrebbero essere state sostituite (es. una direttiva aggiornata). Segnala per verifica manuale.
5. **Missing derived notes:** resources dove il campo `linked_notes` è vuoto o incompleto → proponi creazione delle note derivate mancanti.
6. **Tag coverage:** resources senza theme tag coerenti con il loro contenuto → proponi re-tagging.

Output di questa sezione: lista azioni con severity (high/medium/low). user approva/respinge puntualmente.

---

### SEZIONE 8 — Usage review

Leggi `meta/usage_log.md`. Analizza le ultime 4 settimane:

1. **Trend di consumo:** in crescita, stabile, in calo?
2. **Sessioni outlier:** quali sessioni sono state significativamente più costose della media? Perché?
3. **Cause strutturali di costo:** file troppo grandi letti spesso, tag troppo larghi che attivano troppe letture, loop di chiarimento ricorrenti, ecc.
4. **Proposte di ottimizzazione strutturale** per ridurre il consumo medio di sessione.

Aggiorna le baselines in `meta/usage_log.md`.

---

### SEZIONE 9 — Memory regeneration

Sulla base di tutto il vault corrente:

1. Riscrivi `meta/memory.md` con lo stato aggiornato:
   - Strategic focus: cambiato qualcosa nelle priorità?
   - Active workstreams: aggiornali con stato reale
   - Hot this week: cosa è effettivamente caldo
   - Stalled: cosa è bloccato e perché
   - Open questions for user: gap rimasti
   - Recent decisions: ultime 2 settimane
   - Meta-state of the vault: numeri aggiornati

2. Mostra il diff a user prima di scrivere.

---

### SEZIONE 10 — To-do health check

1. Esegui il rollup di tutti i to-do nel vault (file `/notes/`, file di progetto).
2. Identifica:
   - To-do scaduti (passato due date, non `done`)
   - To-do senza due date (e quanti giorni hanno)
   - To-do in conflitto temporale (troppi nello stesso giorno)
   - To-do orfani (in file non più referenziati)
3. Proponi azioni: chiudere, riprogrammare, riassegnare due date, archiviare.

---

### REPORT FINALE WEEKLY

Alla fine di tutto, produci questo report:

```markdown
## 🗓️ Weekly refactoring report — week of YYYY-MM-DD

### Consolidations applied
- N file with append → consolidated
- N merges
- N splits
- N MOC created

### Taxonomy changes
- Added: ...
- Removed: ...
- Merged: ...
- Renamed: ...

### System rule changes proposed
- [proposal 1] — [accepted/rejected]
- [proposal 2] — [accepted/rejected]

### Memory regenerated
- Diff summary: ...

### Resources health
- N orphan artifacts flagged
- N dead meta-files
- N unused resources
- N derived notes created/updated

### Usage review
- Avg session cost (last 4w): ...
- Trend: ...
- Top optimization opportunity: ...

### To-do health
- N overdue
- N rescheduled
- N closed
- N orphans archived

### 📊 Session usage report
- Files read: N
- Files modified: N
- Estimated messages used: ~N
- Heaviest sub-section: [which one]

### What I'd do differently next week
- [observation 1]
- [observation 2]
```

---

### Hard rules per questa sessione

1. **Mai modificare nulla senza approvazione esplicita.** Tutto è proposta.
2. **Approvazione può essere granulare** (file per file) o batch ("ok tutto").
3. **Mai cancellare contenuto senza preservarlo** in qualche forma (almeno un summary nel file refactored).
4. **Mai modificare CLAUDE.md** senza che user abbia esplicitamente accettato la proposta specifica.
5. **Mai inventare** evidenze empiriche per giustificare un cambio di regola.
6. **Mostra il diff** prima di ogni modifica significativa.

Procedi con la Sezione 1.
