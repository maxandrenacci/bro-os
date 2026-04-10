Bro, è la sessione di daily processing di bro-os.

`CLAUDE.md` è già caricato come project instruction. Leggi nell'ordine:
1. `meta/memory.md` — stato corrente
2. `meta/taxonomy.md` — registro tag attivi

Poi esegui la pipeline a 4 fasi: prima la FASE 0 (todo triage), poi le 3 fasi di inbox processing su tutti i file in `/inbox/` (escluso `/inbox/_archive/`).

---

### FASE 0 — Todo triage (prima dell'inbox)

**Obiettivo:** svuotare il backlog urgente prima di creare nuovo lavoro.

1. **Cerca i task aperti urgenti** nel vault:
   - Grep ricorsivo su `/notes/` per task aperti (`- [ ]`) con data `📅 YYYY-MM-DD`
   - Filtra: data ≤ oggi (overdue) + data = oggi
   - Ordina per data crescente

2. **Se ci sono task overdue o in scadenza oggi:**
   - Mostrali in lista compatta: `[data] [task text] → [[source_file]]`
   - Massimo 10 item. Se >10, mostra i 10 più urgenti e segnala quanti ne restano.
   - Chiedi a user: per ciascuno → **done / drop / snooze [nuova data]?**
   - User può rispondere in batch: es. *"1 done, 2 snooze 20/04, 3 drop"*

3. **Applica le decisioni:**
   - **done** → edita il source file: `- [ ]` → `- [x] ✅ YYYY-MM-DD`
   - **drop** → edita: `- [ ]` → `- [-]` e aggiungi `<!-- dropped YYYY-MM-DD -->`
   - **snooze** → aggiorna la data `📅` nel source file con la nuova data indicata da user

4. **Se nessun task overdue/oggi:** skip silenzioso. Segnala solo nel report finale.

5. **Chiudi la fase con:** *"Per gli altri task aperti, visita `meta/todos.md`."*

> **Limiti:** non aprire più di 5 file sorgente in questa fase. Se i task urgenti sono in >5 file, prioritizza per data e segnala nel report che il triage è stato parziale.

---

### FASE 1 — Comprensione del contesto

Per ogni file in `/inbox/`:

1. Leggi il file grezzo per intero.
2. Identifica esplicitamente:
   - Di cosa parla (tema centrale)
   - Chi è coinvolto (persone, team, stakeholder esterni)
   - In che situazione/progetto/area si inserisce
   - Quali entità menziona (prodotti, norme, decisioni, numeri)
3. Cerca nel vault i file `/notes` potenzialmente pertinenti — per nome, per tag esistenti, per entità menzionate.
4. Forma un'ipotesi di interpretazione **prima di agire**.

Se l'ipotesi è incerta o ambigua, **fermati e chiedi a user in sessione**, ora. Le domande devono essere mirate, non generiche. Mai più di 3 domande per file di inbox. Mai chiedere cose deducibili dal contesto.

---

### FASE 2 — Enrichment

Una volta capito il contenuto:

1. **Auto-enrichment first.** Tutto ciò che puoi dedurre o connettere autonomamente, fallo:
   - Linka entità conosciute con `[[wikilink]]`
   - Identifica progetti/aree pertinenti
   - Calcola implicazioni o connessioni con altri file
   - Proponi categorizzazione (tag esistenti o nuovi)

2. **Gap identification.** Identifica le informazioni mancanti per archiviare correttamente.

3. **Triage dei gap:**
   - **Bloccanti** → chiedi a user in sessione, ora. Sempre nel limite delle 3 domande/file.
   - **Non bloccanti** → crea automaticamente un to-do di follow-up con sintassi Tasks plugin:
     ```
     - [ ] [follow-up] Verificare X 📅 YYYY-MM-DD
     ```
     E procedi con quello che hai.

---

### FASE 3 — Processing & categorizzazione

Solo dopo Fase 1 e 2:

1. **Append/Create in `/notes`:**
   - Se esiste un file `/notes` per il concept → APPEND con marker:
     ```
     <!-- APPEND YYYY-MM-DD from [[source_file]] -->
     contenuto nuovo
     ```
   - Se NON esiste → CREATE nuovo file con frontmatter completo. Naming: solo nome concept, snake_case, in inglese.
   - **Knowledge base in inglese.** Anche se l'input è in italiano, traduci/sintetizza in inglese nei file `/notes`.

2. **Categorizzazione (tag):**
   - Applica i type tag corretti (uno solo, dalla lista fissa)
   - Applica i theme tag esistenti dal `meta/taxonomy.md` quando appropriati
   - **Se proponi un nuovo theme tag:** dichiara esplicitamente nel report finale con rationale. Aggiorna `meta/taxonomy.md`.

3. **Link bidirezionali:** aggiungi `[[wikilink]]` per ogni entità menzionata che esiste nel vault.

4. **To-do extraction:** to-do operativi → dentro il file `/notes` pertinente con sintassi Tasks.

5. **Resources sub-flow (se applicabile):** se l'input è un documento-fonte di terzi (PDF normativo, annual report, white paper), chiedi a user se resource o extract-only, poi esegui il sub-flow completo (artefatto in `/resources/YYYY-MM/` + `__meta.md` + nota derivata in `/notes`).

6. **Archive dell'originale:** rinomina aggiungendo `YYYY-MM-DD_` al filename se non già presente, e sposta da `/inbox/` a `/inbox/_archive/YYYY-MM/`.

---

### REPORT FINALE — obbligatorio

```markdown
## 📋 Daily processing report — YYYY-MM-DD

### Files processed
- `filename.md` → appended to [[x]], [[y]]; created [[z]]

### New tags introduced
- `#tag_name` — rationale: ...
- *(or "none")*

### Files created (new)
- [[file_1]]

### Files updated (append)
- [[file_2]] (+N append blocks)

### Resources added to /resources
- `resources/YYYY-MM/filename.pdf` + `__meta.md` → linked to [[derived_note]]
- *(or "none")*

### Todos triaged (FASE 0)
- done: N task chiusi → [[file1]], [[file2]]
- dropped: N → [[file3]]
- snoozed: N → nuove date aggiornate
- *(or "No overdue tasks at session start.")*

### Questions asked to user in this session
- [domanda] → [risposta]
- *(or "none")*

### Follow-up to-dos created
- [ ] ... 📅 YYYY-MM-DD
- *(or "none")*

### Decisions logged
- [decisione, file dove vive]
- *(or "none")*

### 📊 Session usage report
- Files read: N (~X total lines)
- Files created: N
- Files modified: N
- Estimated messages used in this session: ~N
- Notable cost drivers: ...
- Structural inefficiencies spotted: ...
- Optimization suggestions: ...

### Memory update suggestion
- [aggiornamento puntuale a meta/memory.md]
*(user approva e Bro applica, oppure salta)*
```
