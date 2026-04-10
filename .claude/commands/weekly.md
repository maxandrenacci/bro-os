Bro, è la sessione di weekly refactoring di bro-os.

`CLAUDE.md` è già caricato come project instruction. Leggi nell'ordine:
1. `meta/memory.md`
2. `meta/taxonomy.md`
3. `meta/usage_log.md` (ultime 4 settimane)

Poi esegui le sezioni qui sotto **in ordine**. Per ogni sezione, **proponi le modifiche, non eseguirle**. Aspetta il mio "ok" esplicito (o approvazione selettiva) prima di applicare.

---

### SEZIONE 1 — Consolidamento degli APPEND

Scansiona tutti i file in `/notes/` cercando i marker `<!-- APPEND ... -->`.

Per ogni file con append accumulati nella settimana:
1. Identifica il "core" pulito del file (la prosa pre-append)
2. Identifica i nuovi contenuti negli append
3. **Proponi una versione refactored** che integra i nuovi contenuti nel core
4. Mostra a user: file vecchio (collassato) + proposta nuova + cosa cambia (1 frase)
5. Aspetta approvazione file per file (o batch approval se user lo chiede).

---

### SEZIONE 2 — Deduplicazione

Identifica file `/notes/` che potrebbero essere duplicati o mergeable. Proponi merge uno per uno con: file A, file B, motivo, contenuto risultante. Aspetta OK.

---

### SEZIONE 3 — Splitting

Identifica file `/notes/` che dovrebbero essere splittati:
- File > ~500 righe (soft indicator) → valuta criticamente
- File > ~2000 righe (hard ceiling) → quasi sempre split

Proponi split uno per uno con: file originale, nuovi file proposti, criterio di split. Aspetta OK.

---

### SEZIONE 4 — Link & MOC

1. **Orphan notes:** file senza link entranti né uscenti → ancora rilevanti? Proponi 2 link o archiviazione.
2. **Missing bidirectional links:** A linka B ma B non linka A → proponi rinforzo.
3. **MOC creation:** tag con >15 file e nessun MOC → proponi creazione con struttura completa. Aspetta OK.

---

### SEZIONE 5 — Taxonomy review

Per ogni theme tag in `meta/taxonomy.md`:
- **Sotto-usati** (<3 file dopo 2 settimane) → proponi eliminazione o assorbimento
- **Sovraffollati** (>30 file) → proponi sotto-tag con evidenza empirica
- **Semanticamente sovrapposti** → proponi merge
- **Mancanti** (concetti ricorrenti senza tag) → proponi introduzione

Aggiorna `meta/taxonomy.md` con tutte le modifiche approvate.

---

### SEZIONE 6 — Meta-review delle regole di sistema

Valuta empiricamente, sulla base del vault corrente:
1. Soglie di sizing dei file (~500 soft, ~2000 hard)
2. Criteri di atomicità
3. Tag taxonomy (convergendo o divergendo?)
4. Pipeline daily processing (numero domande, auto-enrichment efficace?)

Per ogni proposta di aggiustamento:
- **Cosa cambiare:** [specifico]
- **Evidenza dal vault:** [dati concreti]
- **Impatto atteso:** [cosa migliora]
- **Cosa modificare in CLAUDE.md:** [paragrafo specifico]

user approva o respinge singolarmente. Le approvate vengono applicate a `CLAUDE.md`.

---

### SEZIONE 7 — Resources health check

Scansiona `/resources/`:
1. **Orphan artifacts** (senza `__meta.md`) → violazione hard rule, proponi creazione o rimozione
2. **Dead meta-files** (artefatto non più esistente) → flagga
3. **Unused resources** (mai referenziate dopo >4 settimane) → proponi azione
4. **Stale status** (regolazioni potenzialmente superate) → segnala per verifica manuale
5. **Missing derived notes** (`linked_notes` vuoto) → proponi creazione

Output: lista azioni con severity (high/medium/low).

---

### SEZIONE 8 — Usage review

Leggi `meta/usage_log.md`. Analizza ultime 4 settimane:
1. Trend di consumo
2. Sessioni outlier e perché
3. Cause strutturali di costo
4. Proposte di ottimizzazione strutturale

Aggiorna le baselines in `meta/usage_log.md`.

---

### SEZIONE 9 — Memory regeneration

Riscrivi `meta/memory.md` con lo stato aggiornato (strategic focus, active workstreams, hot this week, stalled, open questions, recent decisions, meta-state del vault). Mostra diff prima di scrivere.

---

### SEZIONE 10 — To-do health check

Rollup di tutti i to-do nel vault. Identifica:
- To-do scaduti
- To-do senza due date
- To-do in conflitto temporale
- To-do orfani

Proponi azioni: chiudere, riprogrammare, archiviare.

---

### REPORT FINALE WEEKLY

```markdown
## 🗓️ Weekly refactoring report — week of YYYY-MM-DD

### Consolidations applied
- N file con append → consolidated
- N merges / N splits / N MOC created

### Taxonomy changes
- Added: ... / Removed: ... / Merged: ...

### System rule changes proposed
- [proposal] — [accepted/rejected]

### Memory regenerated
- Diff summary: ...

### Resources health
- N orphan artifacts / N dead meta-files / N unused / N derived notes created

### Usage review
- Avg session cost (last 4w): ... / Trend: ...
- Top optimization opportunity: ...

### To-do health
- N overdue / N rescheduled / N closed / N orphans archived

### 📊 Session usage report
- Files read: N / Files modified: N
- Estimated messages used: ~N
- Heaviest sub-section: ...

### What I'd do differently next week
- [observation]
```
