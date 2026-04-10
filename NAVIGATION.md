# Obsidian Navigation — bro-os cheatsheet

> Come navigare bro-os in autonomia da Obsidian, senza chiamare Bro.
> Regola d'oro: **Obsidian è gratis e veloce, Bro consuma messaggi. Per consultare → Obsidian. Per sintetizzare/generare → Bro.**

---

## Setup iniziale (2 minuti, una volta sola)

1. Settings → Core plugins → attiva: Backlinks, Outgoing links, Tags view, Graph view, Quick switcher, Search, Page preview
2. View → trascina nella sidebar destra: **Backlinks** e **Tags** come pannelli sempre visibili
3. Verifica hotkey (Settings → Hotkeys):
   - `Cmd+O` → Quick switcher
   - `Cmd+Shift+F` → Global search
   - `Cmd+Opt+B` → Backlinks pane
   - `Cmd+G` → Graph view
   - `Cmd+Opt+G` → Local graph

---

## 1. Navigazione per link (wikilink)

**Meccanismo base.** Ogni `[[product_a]]` nel testo è cliccabile → apre il file. Da lì segui altri link, navighi come Wikipedia.

**Backlinks pane (il più utile dell'intero sistema).** In ogni file, mostra automaticamente chi linka a questo file. Sei su `contact_a.md` → vedi tutti i meeting, decisioni, progetti che lo menzionano. Zero sforzo. Tienilo sempre aperto nella sidebar destra.

**Outgoing links pane.** Specularmente, mostra tutti i link che partono dal file corrente. Utile per capire "cosa tocca questo file".

**Page preview.** Passa il mouse su un wikilink tenendo `Cmd` → anteprima del file senza aprirlo. Rapidissimo per verificare.

---

## 2. Navigazione per tag

**Tag pane** (sidebar destra, icona tag). Mostra tutti i tag con conteggio. Click su `#economics` → elenca tutti i file taggati. Click su un file → lo apri.

**Tag gerarchici.** Se esistono sotto-tag (`#economics/margin`, `#economics/funding`), il pane li mostra come albero collassabile. Click sul parent → tutti i file di tutti i sotto-tag.

**Limite:** il tag pane mostra tag singoli. Per intersezioni complesse (`#economics AND #project_x`) usa Search o Dataview.

---

## 3. Search full-text (`Cmd+Shift+F`)

Operatori essenziali, usali.

| Query | Risultato |
|---|---|
| `tag:#economics` | Tutti i file con quel tag |
| `tag:#economics tag:#project_x` | Intersezione di tag |
| `-tag:#archived` | Esclusione |
| `path:notes "take rate"` | Frase esatta solo in `/notes` |
| `path:resources` | Tutti i file in `/resources` |
| `file:pay_in` | Fuzzy match sul filename |
| `line:(#decision 2026)` | Linee con entrambi i termini |
| `"exact phrase"` | Ricerca letterale |
| `content:OR (task OR todo)` | OR logico |

**Esempio reale:** stai preparando un memo sul passporting. Query: `tag:#regulation passporting` → trovi tutte le note regulation che menzionano passporting. 10 secondi.

---

## 4. Dashboard Dataview (già costruite)

Le dashboard in `meta/dashboards/` sono **viste calcolate e auto-aggiornate** del vault.

| Dashboard | Quando aprirla |
|---|---|
| `today.md` | Ogni mattina — brief del giorno, overdue, inbox depth |
| `project_x_launch.md` | Prima di ogni context-switch sul Project X |
| `weekly_review.md` | Venerdì, prima del refactoring con Bro |
| `resources.md` | Quando cerchi una fonte autorevole da citare |

Puoi crearne altre copiando il pattern. Esempi utili da aggiungere quando il vault cresce:
- `people.md` — tutti gli stakeholder con ultima interazione
- `decisions.md` — timeline delle decisioni prese
- `economics.md` — tutte le note con `#economics` per prodotto

---

## 5. Graph view

**Full graph** (`Cmd+G`): vista dell'intero vault come grafo. Filtrabile per tag. Utile per:
- Spottare orphan notes (nodi isolati)
- Vedere cluster emergenti
- Weekly refactoring health check

**Local graph** (`Cmd+Opt+G`): sotto-grafo del file corrente a N hop di distanza. Questo lo usi davvero. Es.: apri `product_a.md` → `Cmd+Opt+G` → vedi tutto ciò che lo tocca a 2 gradi.

**Verità onesta:** il full graph è bellissimo e poco utile nel daily. Il local graph è utile. I backlinks e il tag pane sono utilissimi.

---

## 6. Quick switcher (`Cmd+O`)

Il navigatore più usato in assoluto. Digiti parte del nome file → fuzzy match → invio → aperto.

Esempi tipici:
- `Cmd+O` "contact" → apre `contact_a.md`
- `Cmd+O` "eba" → trova tutte le regulation EBA
- `Cmd+O` "dash today" → apre `meta/dashboards/today.md`

---

## Flusso tipico — preparazione call (5-7 min, zero Bro)

Devi prepararti per una call con Contact_A sul margin di Product_A:

1. `Cmd+O` → "contact" → apri `contact_a.md`
2. Backlinks pane → vedi tutti i meeting passati, decisioni, file dove compare
3. Click `[[product_a]]` inline → apre il file prodotto
4. Leggi stato take rate / compressioni
5. `Cmd+Opt+G` → local graph → spotti `[[economics_moc]]` che non ricordavi
6. `Cmd+O` → "today" → apri la dashboard today, vedi to-do correlati
7. `Cmd+Shift+F` → `"tier-1" compression` → trovi una nota dimenticata di 3 settimane fa
8. Chiudi → alla call preparato

**Totale:** 5-7 min, zero messaggi consumati.

---

## Quando usare Obsidian vs quando chiamare Bro

### Obsidian (gratis, veloce)
- Sai cosa cerchi (file specifico, tag, pattern noto)
- Consultazione veloce pre-call
- Esplorazione del grafo per weekly review
- Verifica se qualcosa esiste già
- Rilettura di un file già noto

### Bro / Claude Code (consuma messaggi)
- Non sai cosa cerchi, serve sintesi trasversale
- Devi produrre un output (memo, slide, email) combinando più file
- Analisi cross-cutting ("come si collega X a Y?")
- Processamento di input grezzi
- Generazione di documenti formali
- Domande che richiedono ragionamento, non lookup

**Test pratico:** se la risposta alla tua domanda è "leggi questo file" → Obsidian. Se è "sintetizza questi 7 file e produci X" → Bro.

---

## Anti-patterns

1. **Non chiedere a Bro "dimmi cosa dice product_a.md"** → apri il file, è più veloce e gratis.
2. **Non usare il full graph per lavoro quotidiano** → è weekly review material.
3. **Non memorizzare i path dei file** → usa sempre `Cmd+O` con fuzzy match.
4. **Non scrollare le sidebar manualmente** → Dataview + Search sono infinitamente più veloci.
5. **Non scrivere nelle dashboard** → sono auto-generate, le tue modifiche si perdono al prossimo re-render delle query.

---

## Hotkey da memorizzare (top 6)

| Hotkey | Azione |
|---|---|
| `Cmd+O` | Quick switcher — apri qualunque file |
| `Cmd+Shift+F` | Global search con operatori |
| `Cmd+Opt+B` | Backlinks pane |
| `Cmd+Opt+G` | Local graph del file corrente |
| `Cmd+click` | Apri link in nuovo tab |
| `Cmd+hover` | Page preview |

Il resto impari usando.
