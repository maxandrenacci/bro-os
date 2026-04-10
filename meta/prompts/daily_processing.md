# Daily Processing — Prompt for Bro

> **How to use:** open Claude Cowork pointed at the bro-os vault root. Paste this prompt. Bro will execute the full daily processing pipeline.
>
> **When to use:** every evening, ~10-15 min, to empty `/inbox` before closing the day.

---

## PROMPT (copy from here)

Bro, è la sessione di daily processing di bro-os.

Prima di iniziare, leggi nell'ordine:
1. `CLAUDE.md` (root) — regole operative del sistema
2. `meta/memory.md` — stato corrente
3. `meta/taxonomy.md` — registro tag attivi

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

4. **Se nessun task overdue/oggi:** skip silenzioso. Segnala solo nel report finale: *"No overdue tasks at session start."*

5. **Chiudi la fase con:** *"Per gli altri task aperti, visita `meta/todos.md`."*

> **Limiti operativi:** non aprire più di 5 file sorgente in questa fase. Se i task urgenti sono dispersi in >5 file, prioritizza per data e segnala nel report che il triage è stato parziale.

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

Esempio di domanda buona: *"Quando dici 'rivedere i terms' nella nota del meeting con Contact_A, ti riferisci ai terms commerciali del [[merchant_X]] di cui parlavamo settimana scorsa, o è qualcosa di nuovo su un altro merchant?"*

Esempio di domanda cattiva: *"Potresti chiarire questa parte?"*

---

### FASE 2 — Enrichment

Una volta capito il contenuto:

1. **Auto-enrichment first.** Tutto ciò che puoi dedurre o connettere autonomamente, fallo:
   - Linka entità conosciute con `[[wikilink]]`
   - Identifica progetti/aree pertinenti
   - Calcola implicazioni o connessioni con altri file
   - Proponi categorizzazione (tag esistenti o nuovi)

2. **Gap identification.** Identifica le informazioni mancanti per archiviare correttamente. Esempi:
   - Non so quale stakeholder ha preso questa decisione
   - Non so se questa norma è in vigore o ancora draft
   - Non so a quale fase del progetto project_x si riferisce questo numero

3. **Triage dei gap:**
   - **Bloccanti** (senza la risposta non posso processare bene) → chiedi a user in sessione, ora. Sempre nel limite delle 3 domande/file.
   - **Non bloccanti** → crea automaticamente un to-do di follow-up con sintassi Tasks plugin:
     ```
     - [ ] [follow-up] Verificare con [[marco]] se il numero 2.5% è netto o lordo 📅 YYYY-MM-DD
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
   - Se NON esiste → CREATE nuovo file con frontmatter completo. Naming: solo nome concept, snake_case, in inglese. Disambiguazione solo se necessaria.
   - **Knowledge base in inglese.** Anche se l'input è in italiano, traduci/sintetizza in inglese nei file `/notes`.

2. **Categorizzazione (tag):**
   - Applica i type tag corretti (uno solo, dalla lista fissa)
   - Applica i theme tag esistenti dal `meta/taxonomy.md` quando appropriati
   - **Se proponi un nuovo theme tag:** dichiara esplicitamente nel report finale (vedi sotto) con rationale. Aggiorna `meta/taxonomy.md` con la nuova entry.
   - Preferisci tag di dominio larghi (es. `#economics`) a specifici (es. `#margin`).

3. **Link bidirezionali:**
   - Aggiungi `[[wikilink]]` per ogni entità menzionata che esiste già nel vault
   - Se menzioni un'entità che non esiste ancora → crea il file stub con minima frontmatter

4. **To-do extraction:**
   - To-do operativi del business → con sintassi Tasks, dentro il file `/notes` pertinente
   - To-do di enrichment dalla Fase 2 → stesso formato, taggati `#follow-up`
   - Tutti i to-do hanno una due date (anche stimata, segnalandolo)

5. **Resources sub-flow (se applicabile).** Se l'input è un documento-fonte di terzi (PDF normativo, annual report, white paper, ecc.), invece del semplice append/create:
   - **Decisione esplicita:** chiedi a user *"Questo è un source document da preservare in `/resources`, o basta estrarre i fatti?"* Default: se è PDF strutturato da autorità/ente/competitor → resource. Se è screenshot, mail, nota → extract only.
   - Se resource:
     - Sposta/copia l'artefatto originale in `/resources/YYYY-MM/` con naming `YYYY-MM-DD_descriptor.ext`
     - Crea il `__meta.md` gemello usando il template `meta/templates/resource_meta.md`. Compila tutte le sezioni possibili dall'estrazione.
     - Tag: `#resource` come type tag, + theme tag appropriati
     - Crea/aggiorna le note derivate in `/notes` con i fatti estratti, linkando al meta-file via `[[resource_meta_filename]]`
     - Nel meta-file, il campo `linked_notes` punta alle note derivate
   - **Mai saltare la creazione del meta-file.** Una resource orfana è una resource persa.
   - **Per PDF lunghi (>20 pagine):** segnala a user che conviene eseguire l'estrazione via Claude Code invece di Cowork per efficienza di lettura.

6. **Archive dell'originale:**
   - Sposta ogni file processato da `/inbox/` a `/inbox/_archive/YYYY-MM/`
   - Mantieni il filename originale

---

### REPORT FINALE — obbligatorio

Alla fine della sessione, produci un report in questo formato:

```markdown
## 📋 Daily processing report — YYYY-MM-DD

### Files processed
- `2026-04-07_meeting_marco.md` → appended to [[product_a]], [[contact_a]]; created [[merchant_x_negotiation]]
- `2026-04-07_pdf_eba_gl.md` → created [[eba_gl_2026_03]]; tagged #regulation #legal
- ...

### New tags introduced
- `#tag_name` — rationale: [perché questo tag, perché un esistente non bastava]
- *(or "none")*

### Files created (new)
- [[file_1]]
- [[file_2]]

### Files updated (append)
- [[file_3]] (+2 append blocks)
- [[file_4]] (+1 append block)

### Resources added to /resources
- `resources/YYYY-MM/filename.pdf` + `__meta.md` → linked to [[derived_note]]
- *(or "none")*

### Todos triaged (FASE 0)
- done: N task chiusi → [[file1]], [[file2]]
- dropped: N → [[file3]]
- snoozed: N → nuove date aggiornate
- *(or "No overdue tasks at session start.")*

### Questions asked to user in this session
- [domanda 1] → [risposta breve]
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
- Estimated messages used in this session: ~N (out of Claude Team 5h window)
- Notable cost drivers: [es. lettura di un file gigante, loop di chiarimenti]
- Structural inefficiencies spotted: [se rilevanti]
- Optimization suggestions: [se utili al weekly]

### Memory update suggestion
Sulla base di quello che ho processato oggi, propongo questo aggiornamento a `meta/memory.md`:
- [aggiornamento puntuale]
*(user approva e Bro applica, oppure salta)*
```

---

### Hard rules per questa sessione

1. **Mai inventare.** Se un dato non è nell'input o non è verificabile dal vault → o chiedi, o crea un to-do.
2. **Mai cancellare o rinominare** file "notes" esistenti. Solo append/create.
3. **Mai più di 3 domande** per file di inbox.
4. **Mai skippare** l'archive dell'originale.
5. **Knowledge base in inglese**, sempre. Input misti IT/EN ok.
6. **Dichiara esplicitamente ogni nuovo tag** introdotto.
7. **Se in dubbio → chiedi.** Meglio una domanda non banale che un'allucinazione.

Procedi.
