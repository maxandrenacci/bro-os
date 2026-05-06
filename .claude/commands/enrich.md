Bro, è una sessione di **enrichment standalone** di bro-os.

`CLAUDE.md` è già caricato come project instruction. Leggi nell'ordine:
1. `meta/memory.md` — stato corrente
2. `meta/taxonomy.md` — registro tag attivi

Poi esegui **solo le FASI 1 e 2** della pipeline (comprensione del contesto + enrichment) sui file in `/inbox/` che **non sono ancora stati enriched**. Niente FASE 3 (no append a `/notes`, no archive). Specifica completa in `meta/prompts/enrich.md`.

---

### Selezione file

Scansiona `/inbox/` (escluso `/inbox/_archive/`). Per ciascun file leggi il frontmatter:
- Se `enriched:` è una data (`YYYY-MM-DD`) → **skip** (già fatto). Segnalalo nel report.
- Se `enriched: false` o assente → procedi.

> **Idempotenza:** questa è la regola di anti-rework condivisa con `/daily`. Mai re-enrichare un file con `enriched: YYYY-MM-DD` settato.

---

### FASE 1 — Comprensione del contesto

Per ogni file selezionato:

1. Leggi il file grezzo per intero.
2. Identifica esplicitamente: tema centrale, persone coinvolte, progetto/area, entità menzionate (prodotti, norme, decisioni, numeri).
3. Usa `meta/memory.md` come fonte primaria per il contesto del vault. Apri note solo se memory.md è insufficiente per l'arricchimento richiesto.
4. Forma un'ipotesi di interpretazione **prima di agire**.

Se ambiguo → fermati e chiedi a user. Massimo 3 domande mirate per file. Mai chiedere cose deducibili.

---

### FASE 2 — Enrichment in-file

1. **Aggiungi la sezione `## Discussion (enriched)`** subito dopo la sezione raw notes del file inbox. Mai modificare i raw notes. La sezione enriched:
   - Interpreta gli appunti terse
   - Linka entità con `[[wikilink]]`
   - Espone decisioni implicite e domande aperte
   - Identifica connessioni con il vault esistente
   - Compila `Decisions taken`, `Action items`, `Open questions` se deducibili

2. **Auto-enrichment first.** Tutto il deducibile va fatto adesso.

3. **Gap triage:**
   - **Bloccanti** → chiedi a user ora (sempre nel limite 3/file).
   - **Non bloccanti** → crea un `- [ ] [follow-up] ... 📅 YYYY-MM-DD` direttamente nella sezione enriched.

4. **Marca il file come enriched.** Aggiorna il frontmatter:
   ```yaml
   enriched: YYYY-MM-DD
   ```
   Se il campo non esiste, aggiungilo. Se `enriched: false`, sostituisci con la data di oggi.

5. **Conversione task syntax: NO in questa fase.** I `- [ ]` restano. Saranno convertiti in prosa dal `/daily` prima dell'archive (per evitare duplicati in `meta/todos.md` post-archive).

---

### Cosa NON fare

- ❌ Nessun append/create in `/notes/`
- ❌ Nessun archive in `/inbox/_archive/`
- ❌ Nessun resource sub-flow (rimandato al `/daily`)
- ❌ Nessuna FASE 0 (todo triage)

`/enrich` è puramente in-file sui file di inbox. Tutto il resto resta lavoro del `/daily`.

---

### REPORT FINALE — obbligatorio

```markdown
## 🔍 Enrichment report — YYYY-MM-DD

### Files enriched
- `filename.md` → enriched section added, frontmatter marked

### Files skipped (already enriched)
- `filename.md` → enriched: YYYY-MM-DD

### Questions asked to user
- [domanda] → [risposta]
- *(or "none")*

### Follow-up to-dos created (in inbox files)
- [ ] ... 📅 YYYY-MM-DD
- *(or "none")*

### 📊 Session usage report
- Files read: N
- Files modified: N
- Estimated messages used: ~N
- Notable cost drivers: ...

### Next step
Run `/daily` to process enriched files into `/notes` and archive.
```
