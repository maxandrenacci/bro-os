Bro, è la sessione di onboarding di bro-os. L'utente sta configurando il vault per la prima volta.

`CLAUDE.md` è già caricato come project instruction. Leggi `meta/memory.md` per vedere cosa manca.

---

### OBIETTIVO

Popolare `meta/memory.md` con il contesto reale dell'utente e creare i primi file stub in `/notes/` per le entità chiave. Al termine, il vault è pronto per il primo `/daily`.

---

### FASE 1 — Raccolta contesto (una sola domanda batch)

Chiedi all'utente **in un unico messaggio** tutte le informazioni necessarie. Non fare domande una alla volta.

```
Per configurare bro-os ho bisogno di capire il tuo contesto di lavoro. Rispondimi in modo libero — non serve seguire un formato.

1. **Ruolo e azienda.** Qual è il tuo ruolo, in che azienda, da quando?

2. **Struttura.** A chi riporti? Hai direct reports?

3. **Priorità strategiche.** Quali sono le 2-3 cose più importanti su cui lavori nei prossimi 6-12 mesi?

4. **Workstream attivi.** Cosa hai sul tavolo questa settimana / questo mese? Puoi elencarli anche in modo grezzo.

5. **Persone chiave.** Chi sono i 3-5 stakeholder più rilevanti con cui interagisci regolarmente? Nome, ruolo, e perché sono rilevanti.

6. **Progetti attivi.** Ci sono progetti con una scadenza o un deliverable preciso? Nome del progetto, obiettivo, quando deve finire.

7. **Cose urgenti o bloccate.** C'è qualcosa che scade presto o che è in stallo in attesa di qualcosa/qualcuno?

Non devi essere esaustivo — quello che mi dai qui è il punto di partenza, il vault crescerà dall'uso quotidiano.
```

---

### FASE 2 — Processing delle risposte

Una volta ricevute le risposte, **senza fare ulteriori domande**, esegui:

#### 2a. Aggiorna `meta/memory.md`

Riscrivi il file con i dati reali dell'utente. Sostituisci tutti i placeholder `[...]` con i contenuti forniti. Segui la struttura esistente. Nella sezione "Notes for Bro" aggiorna con osservazioni sul contesto specifico dell'utente che saranno utili nelle sessioni future.

#### 2b. Crea file stub in `/notes/`

Per ogni entità menzionata dall'utente, crea un file stub minimale:

**Per ogni persona chiave:**
```yaml
---
tags: [person]
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
last_refactored: YYYY-MM-DD
role: [ruolo]
company: [azienda]
---
# [Nome]
[1 frase di contesto dal signup]
```
Naming: `contact_a.md`, `contact_b.md` ecc. — usa nomi generici se preferisci pseudonimia, oppure nomi reali se l'utente li ha forniti.

**Per ogni progetto attivo:**
Usa il template `meta/templates/project.md`. Compila: obiettivo, stato `#active`, target_date se fornita.

**Per ogni area di responsabilità continuativa (no scadenza):**
```yaml
---
tags: [area]
created: YYYY-MM-DD
last_updated: YYYY-MM-DD
last_refactored: YYYY-MM-DD
---
# [Area name]
[1 frase descrittiva]
```

Non creare stub per concetti generici. Solo per entità nominate dall'utente che torneranno nella knowledge base.

#### 2c. Aggiorna `meta/taxonomy.md`

Se dall'onboarding emergono theme tag evidenti (es. l'utente lavora molto su temi regolatori → `#legal` è già nel template), aggiorna i file count e le date di first_seen con la data di oggi.

---

### FASE 3 — Vault health check

Verifica rapidamente:
1. Struttura cartelle: esistono `inbox/`, `notes/`, `resources/`, `outputs/`, `meta/`?
2. Plugin Obsidian: non verificabile da qui — ricorda all'utente di installarli se non l'ha fatto (Dataview, Tasks, Templater).
3. `meta/todos.md` è presente e usa sintassi Tasks corretta?

---

### REPORT FINALE — obbligatorio

```markdown
## ✅ bro-os signup report — YYYY-MM-DD

### Context loaded
- Ruolo: [ruolo @ azienda]
- Priorità strategiche: N identificate
- Workstream attivi: N
- Stakeholder chiave: N

### Files created
- `meta/memory.md` — aggiornato con contesto reale
- [[contact_a]], [[contact_b]], ... — N stub persona creati
- [[project_x]], ... — N stub progetto creati
- [[area_1]], ... — N stub area creati

### Tag taxonomy
- Theme tag attivati: [lista]

### Vault health
- Struttura cartelle: ✅ / ⚠️ [problema]
- Reminder: installa i plugin Obsidian (Dataview, Tasks, Templater) se non ancora fatto

### Prossimi passi
1. Crea la tua prima inbox note da un input reale
2. Esegui `/daily` stasera (anche se l'inbox ha solo 1 file — è importante partire)
3. Segna il primo `/weekly` in calendario per venerdì

### 📊 Session usage report
- Files read: N
- Files created: N
- Files modified: N
- Estimated messages used: ~N
```

Procedi.
