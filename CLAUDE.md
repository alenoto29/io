# Agent Instructions — Electricity Offer Landing Framework

> **Progetto**: Template replicabile per landing page schede offerta luce
> **Owner**: Alessandro @ papernest.it (Barcellona)
> **Competitor principale**: Segugio.it
> **Offerta pilota**: Fixa Time — Plenitude
> **Stack**: WordPress Gutenberg, HTML/CSS inline (flexbox > grid)
> **Data avvio**: 18 marzo 2026

---

## Perché WAT e non altro

| Framework | Verdetto | Motivo |
|---|---|---|
| **WAT** | **SCELTO** | Separazione reasoning/execution, markdown SOPs, zero infra, improvement loop nativo |
| LangChain/LangGraph | Scartato | Richiede runtime Python separato, overkill per singolo utente + Claude Code CLI |
| CrewAI/AutoGen | Scartato | Multi-agent orchestration non necessaria, complessità senza valore aggiunto |
| Cursor Rules | Scartato | IDE-specific, non funziona con Claude Code |
| Pure .md prompts | Scartato | Nessuna execution deterministica, nessun riuso tool, fragile su API calls |
| Claude Code Skills | Complementare | Utile per shortcut, ma non è un sistema di workflow orchestration completo |

**Principio chiave**: L'AI ragiona e orchestra. Python esegue. Se ogni step AI ha 90% accuracy, dopo 5 step sei al 59%. Delegando l'execution a script deterministici, mantieni alta l'affidabilità.

---

## Architettura WAT

### Layer 1: Workflows (Le Istruzioni)
- Markdown SOPs in `workflows/`
- Ogni workflow definisce: obiettivo, input richiesti, tool da usare, output attesi, gestione edge case
- Scritti in linguaggio naturale, come briefare un collega

### Layer 2: Agent (Il Decision-Maker)
- Questo è il ruolo di Claude
- Legge il workflow, esegue i tool nella sequenza corretta, gestisce fallimenti, chiede chiarimenti
- Connette l'intento all'esecuzione senza fare tutto direttamente

### Layer 3: Tools (L'Esecuzione)
- Script Python in `tools/` che fanno il lavoro reale
- API calls, data transformations, file operations
- Credenziali in `.env` (MAI altrove)
- Consistenti, testabili, veloci

---

## Directory Layout

```
CLAUDE.md                           # Questo file — istruzioni principali
contesto_iniziale.md                # Chi è Alessandro, competenze, obiettivi

workflows/
├── 00_master_landing_audit.md      # Orchestratore: sequenza tutte le fasi
├── 01_template_analysis.md         # Fase 1: Analisi HTML template + componenti
├── 02_seo_analysis.md              # Fase 2: SEO audit competitivo
├── 03_wcv_analysis.md              # Fase 3: Web Core Vitals audit
├── 04_uiux_analysis.md             # Fase 4: UI/UX design review
├── 05_serp_analysis.md             # Fase 5: SERP analysis & intent mapping
├── 06_ai_citation_analysis.md      # Fase 6: AI citation & LLM visibility
└── 07_template_assembly.md         # Fase 7: Assemblaggio template finale

tools/
├── ahrefs_kw_analysis.py           # Keyword research via Ahrefs API
├── gsc_performance.py              # GSC data extraction
├── serp_scraper.py                 # SERP scraping e feature detection
├── wcv_audit.py                    # Lighthouse / CrUX audit
├── competitor_scraper.py           # Scraping pagine competitor (Segugio, etc.)
├── ai_citation_checker.py          # Check citazioni su ChatGPT, Perplexity, Google AI
├── html_validator.py               # Validazione HTML output
└── schema_generator.py             # Generazione structured data JSON-LD

.tmp/
├── serp_data/                      # Risultati SERP raw
├── competitor_data/                # HTML/dati competitor scraped
├── kw_data/                        # Export keyword Ahrefs
├── wcv_reports/                    # Report Lighthouse/CrUX
└── ai_citation_data/               # Risultati check citazioni AI

.env                                # API keys (Ahrefs, GSC, etc.)
```

---

## Il Progetto: Landing Page Offerta Luce

### Obiettivo
Creare un **template HTML replicabile** per schede offerta luce su papernest.it che:
1. Rankhi sopra Segugio.it sulle keyword offerta specifiche
2. Rispetti Web Core Vitals al 100%
3. Sia accessibile (WCAG 2.1 AA)
4. Ottenga citazioni AI (ChatGPT, Perplexity, Google AI Overviews)
5. Converta traffico organico in lead/click-out
6. Sia replicabile per 5-15 offerte luce diverse

### Vincoli Tecnici WordPress
- HTML/CSS inline (no framework JS)
- **Flexbox > Grid** per compatibilità WordPress Gutenberg
- Componenti custom: `<ppn-deal>` e altri tag papernest
- Brand colors: Purple `#5A51FF` / `#6C75F4`, Green `#49C5AE`
- Il template viene caricato come blocco HTML custom in Gutenberg

---

## Le 7 Fasi del Framework

### Fase 0 — Master Orchestration
**File**: `workflows/00_master_landing_audit.md`
**Ruolo**: Sequenzia tutte le fasi, definisce dipendenze e handoff tra fasi.

```
Fase 1 (Template) → Fase 2 (SEO) → Fase 3 (WCV)
                                          ↓
Fase 7 (Assembly) ← Fase 4 (UI/UX) ← Fase 5 (SERP) → Fase 6 (AI Citation)
```

I dati output di ogni fase finiscono in `.tmp/` e alimentano le fasi successive.

---

### Fase 1 — Template Analysis
**File**: `workflows/01_template_analysis.md`
**Obiettivo**: Analizzare l'HTML fornito, mappare ogni componente, identificare struttura e guideline.

**Input**: HTML template (incollato sotto)
**Output**: Mappa componenti, gerarchia heading, schema struttura, checklist elementi

#### 📋 HTML TEMPLATE — INCOLLA QUI

```html
<!-- ============================================================
     INCOLLA QUI L'HTML COMPLETO DEL TEMPLATE OFFERTA LUCE

     Quando lo incolli, Claude analizzerà automaticamente:
     - Struttura heading (H1, H2, H3...)
     - Componenti custom (<ppn-deal>, etc.)
     - Layout pattern (flexbox, struttura sezioni)
     - Brand elements (colori, font, spacing)
     - CTA e conversion elements
     - Elementi SEO esistenti (meta, schema, etc.)
     ============================================================ -->
```

**Azioni Agent dopo incolla**:
1. Identifica tutti i componenti e assegna un ID semantico
2. Mappa la gerarchia heading
3. Estrai pattern CSS ricorrenti
4. Identifica sezioni: hero, pricing, features, FAQ, CTA, footer
5. Documenta le guideline implicite nel codice
6. Genera `01_component_map.json` in `.tmp/`

---

### Fase 2 — SEO Analysis (Top Level)
**File**: `workflows/02_seo_analysis.md`
**Obiettivo**: Analisi SEO competitiva delle migliori al mondo, focalizzata su keyword facilmente scalabili.

#### Strategia SEO Offerta Luce

**Keyword Cluster Target** (da validare con Ahrefs):
```
Tier 1 — Brand + Offerta (high intent, low competition)
├── "plenitude fixa time"
├── "plenitude fixa time opinioni"
├── "plenitude fixa time costo"
├── "offerta fixa time plenitude prezzo"
└── "plenitude fixa time recensioni"

Tier 2 — Operatore + Categoria
├── "offerte luce plenitude"
├── "plenitude luce prezzo fisso"
├── "migliori offerte plenitude 2026"
└── "plenitude energia opinioni"

Tier 3 — Comparativa / Intent decisionale
├── "plenitude vs enel luce"
├── "offerte luce prezzo fisso confronto"
├── "migliore offerta luce prezzo fisso marzo 2026"
└── "cambiare fornitore luce conviene"

Tier 4 — Long tail / Deep keyword
├── "plenitude fixa time quanto si risparmia"
├── "plenitude fixa time costo kWh"
├── "attivare offerta plenitude online"
└── "plenitude fixa time cosa include"
```

**Analisi Competitor (Segugio.it come benchmark)**:
1. Scrape top 10 SERP per ogni keyword Tier 1-2
2. Analizza le pagine Segugio che rankano: struttura, heading, word count, schema markup, internal linking
3. Identifica keyword dove Segugio ranka in posizione 4-15 (facilmente scalabili)
4. Identifica keyword dove nessun comparatore è in top 3 (opportunità blue ocean)
5. Analizza il content gap: cosa copre Segugio che papernest non ha
6. Reverse engineer il profilo backlink delle pagine top performing

**Output attesi**:
- `02_keyword_matrix.json` — keyword con volume, KD, CPC, posizione attuale, posizione Segugio
- `02_content_gap.md` — analisi gap con raccomandazioni
- `02_competitor_structure.md` — reverse engineering struttura pagine Segugio
- `02_seo_blueprint.md` — blueprint SEO per il template

**Principi SEO per il template**:
- Un H1 per pagina: `{Nome Offerta} di {Operatore}: Prezzo, Opinioni e Come Attivarla`
- Schema markup: `Product`, `Offer`, `AggregateRating`, `FAQPage`, `BreadcrumbList`
- Internal linking strategico verso hub operatore e categoria offerte
- Content freshness: sezione "Aggiornamento {Mese} {Anno}" per segnale di aggiornamento
- E-E-A-T signals: autore, data aggiornamento, fonti ARERA, disclaimer trasparenza

---

### Fase 3 — Web Core Vitals Analysis
**File**: `workflows/03_wcv_analysis.md`
**Obiettivo**: Garantire score 90+ su tutti i Core Web Vitals.

**Metriche target**:
| Metrica | Target | Soglia Google |
|---|---|---|
| LCP (Largest Contentful Paint) | < 1.5s | < 2.5s |
| INP (Interaction to Next Paint) | < 100ms | < 200ms |
| CLS (Cumulative Layout Shift) | < 0.05 | < 0.1 |
| FCP (First Contentful Paint) | < 1.0s | < 1.8s |
| TTFB (Time to First Byte) | < 400ms | < 800ms |

**Checklist WCV per HTML statico in WordPress**:
- [ ] Zero render-blocking CSS/JS esterni
- [ ] CSS inline critical path (già il nostro approccio)
- [ ] Immagini con `width`/`height` espliciti (no CLS)
- [ ] Immagini `loading="lazy"` sotto il fold
- [ ] Font-display: swap per web fonts (o system fonts)
- [ ] Nessun layout shift da contenuto dinamico
- [ ] Preconnect per risorse terze necessarie
- [ ] Dimensioni immagini ottimali (WebP, sizing corretto)

**Output attesi**:
- `03_wcv_audit_report.md` — report dettagliato con score e raccomandazioni
- `03_wcv_checklist.md` — checklist verificata per ogni componente
- `03_performance_budget.md` — budget performance per peso pagina, richieste, tempi

---

### Fase 4 — UI/UX Analysis (Front-End Design Specialist Level)
**File**: `workflows/04_uiux_analysis.md`
**Obiettivo**: Ottimizzare UI/UX come un top front-end design specialist.

**Principi UI/UX per Landing Offerta Luce**:

1. **Visual Hierarchy**: Il prezzo/risparmio deve essere l'elemento più visibile above the fold
2. **Trust Signals**: Logo operatore, badge ARERA, recensioni, certezza prezzo
3. **Scannability**: L'utente deve capire l'offerta in 5 secondi di scan
4. **Mobile-First**: 70%+ del traffico sarà mobile — ogni componente deve essere pensato mobile prima
5. **CTA Strategy**: CTA primaria above the fold + CTA sticky su mobile + CTA dopo FAQ
6. **Information Architecture**:
   ```
   Hero (offerta + prezzo + CTA)
   → Dettagli offerta (cosa include)
   → Tabella comparativa (vs altre offerte)
   → Vantaggi/Svantaggi (trust through honesty)
   → Come attivare (step by step)
   → FAQ (SEO + UX)
   → CTA finale
   ```

**Analisi componenti**:
Per ogni componente del template, valutare:
- Accessibilità (WCAG 2.1 AA): contrasto, focus states, screen reader, alt text
- Responsive behavior: breakpoint 320px, 375px, 768px, 1024px, 1440px
- Micro-interactions: hover states, transizioni, feedback visivo
- Coerenza con brand papernest (colors, spacing, typography)
- Conversion optimization: friction points, cognitive load, decision architecture

**Output attesi**:
- `04_uiux_audit.md` — audit completo di ogni componente
- `04_accessibility_report.md` — report WCAG con issue e fix
- `04_responsive_specs.md` — specifiche responsive per ogni breakpoint
- `04_conversion_recommendations.md` — raccomandazioni CRO

---

### Fase 5 — SERP Analysis (Top Level)
**File**: `workflows/05_serp_analysis.md`
**Obiettivo**: Analisi SERP profonda per capire come Google interpreta l'intent e come vincere ogni feature.

**Cosa analizzare per ogni keyword target**:
1. **SERP Features presenti**:
   - Featured Snippet (paragrafo, lista, tabella)
   - People Also Ask (PAA)
   - Knowledge Panel
   - AI Overview (SGE)
   - Immagini/Video
   - Local Pack
   - Sitelink
   - Product/Price markup

2. **Intent Mapping**:
   - Informational vs Transactional vs Commercial Investigation
   - Micro-intent: compare, review, price, activate, switch
   - SERP composition: quanti risultati informativi vs transazionali

3. **Competitor SERP Positioning**:
   - Chi domina posizioni 1-3 per ogni keyword
   - Che tipo di pagina ranka (landing, guide, comparazione, forum)
   - Come sono strutturati i title tag e meta description
   - Quali schema markup usano

4. **Gap & Opportunity**:
   - Keyword dove la SERP è "debole" (forum, contenuti datati, thin content in top 5)
   - Feature SERP non sfruttate dai competitor
   - PAA dove nessuno ha un contenuto dedicato

**Output attesi**:
- `05_serp_feature_map.json` — mappa feature SERP per keyword
- `05_intent_analysis.md` — analisi intent dettagliata
- `05_serp_opportunity.md` — opportunità identificate
- `05_snippet_templates.md` — template contenuto ottimizzati per featured snippet

---

### Fase 6 — AI Citation & LLM Visibility Analysis
**File**: `workflows/06_ai_citation_analysis.md`
**Obiettivo**: Essere citati quando ChatGPT, Perplexity o Google AI Overviews rispondono a domande sulle offerte luce.

**Come funzionano le citazioni AI** (stato dell'arte 2026):

1. **Google AI Overviews**:
   - Privilegia contenuti con E-E-A-T forte
   - Cita fonti strutturate (schema markup, dati verificabili)
   - Favorisce contenuti con "definitive answers" (tabelle, numeri specifici)
   - Il contenuto deve essere crawlable e in structured format

2. **ChatGPT (con browsing)**:
   - Cerca fonti autorevoli e aggiornate
   - Privilegia contenuti con data di aggiornamento recente
   - Preferisce pagine con risposte dirette, non commercial-heavy
   - Cita domini con alta domain authority

3. **Perplexity**:
   - Cita direttamente le fonti usate per la risposta
   - Privilegia contenuti freschi e specifici
   - Preferisce formati strutturati (tabelle, liste, comparazioni)
   - Attento alla citabilità del contenuto (frasi che rispondono direttamente)

**Pattern per ottenere citazioni AI nel template**:
- **Definitive Data Points**: prezzi esatti, costi kWh, date validità offerta
- **Structured Comparisons**: tabelle confronto con dati verificabili
- **Direct Answers**: paragrafi che rispondono direttamente a domande specifiche
- **Freshness Signals**: data aggiornamento prominente, sezione "Aggiornato a {mese} {anno}"
- **Source Attribution**: citare fonti ARERA, dati ufficiali operatore
- **FAQ Structured**: domande e risposte che matchano le query conversazionali
- **Entity Clarity**: identificazione chiara di entità (operatore, offerta, tipo tariffa)

**Query da monitorare**:
```
ChatGPT / Perplexity / Google AI:
- "Qual è la migliore offerta luce prezzo fisso?"
- "Quanto costa l'offerta Fixa Time di Plenitude?"
- "Conviene passare a Plenitude?"
- "Confronto offerte luce prezzo fisso 2026"
- "Plenitude Fixa Time opinioni"
```

**Output attesi**:
- `06_ai_citation_audit.md` — stato attuale citazioni papernest vs competitor
- `06_citation_optimization.md` — raccomandazioni per ottenere citazioni
- `06_structured_content_patterns.md` — pattern di contenuto per massimizzare citabilità
- `06_query_monitoring_list.md` — lista query da monitorare regolarmente

---

### Fase 7 — Template Assembly
**File**: `workflows/07_template_assembly.md`
**Obiettivo**: Assemblare il template finale integrando tutti i finding delle fasi precedenti.

**Processo**:
1. Partire dal template HTML originale (Fase 1)
2. Applicare le raccomandazioni SEO (Fase 2): heading, schema, keyword placement
3. Applicare le ottimizzazioni WCV (Fase 3): performance budget, lazy loading, CSS
4. Applicare le raccomandazioni UI/UX (Fase 4): hierarchy, accessibility, responsive
5. Integrare gli snippet template per SERP (Fase 5): FAQ, tabelle, structured content
6. Integrare i pattern AI citation (Fase 6): dati strutturati, freshness, entity markup
7. Validare il template finale: HTML validator, Lighthouse, accessibility checker
8. Generare la documentazione di compilazione per replicare su altre offerte

**Output finale**:
- Template HTML completo e ottimizzato
- Guida compilazione per replicare su 5-15 offerte
- Checklist pre-pubblicazione

---

## Come Operare (Regole Agent)

### 1. Cerca tool esistenti prima
Prima di costruire qualcosa di nuovo, controlla `tools/`. Crea nuovi script solo quando necessario.

### 2. Impara e adatta quando qualcosa fallisce
- Leggi l'errore completo e il trace
- Fixa lo script e retesta (**se usa API a pagamento, chiedi prima di rieseguire**)
- Documenta nel workflow: rate limit, timing quirks, comportamenti inattesi

### 3. Tieni i workflow aggiornati
I workflow evolvono. Quando trovi metodi migliori, scopri vincoli o incontri issue ricorrenti, aggiorna. **Ma non creare o sovrascrivere workflow senza chiedere** a meno che non te lo dica esplicitamente.

### 4. Output format
- Dati intermedi: JSON in `.tmp/`
- Analisi e report: Markdown in `.tmp/` o direttamente nel workflow
- Deliverable finali: HTML + documentazione

### 5. Lingua
- Comunicazione: italiano
- Codice, variabili, nomi file: inglese
- Workflow: italiano con terminologia tecnica in inglese

### 6. Proattività
Quando noti un approccio subottimale o esiste una tecnica migliore, **segnalalo**. Suggerisci MCP server, tool, pattern che possono migliorare il risultato.

---

## Self-Improvement Loop

```
1. Identifica cosa si è rotto
2. Fixa il tool
3. Verifica che funzioni
4. Aggiorna il workflow
5. Vai avanti con un sistema più robusto
```

Ogni fallimento rende il framework più forte.

---

## Quick Reference — Tool Input/Output Contracts

Ogni tool segue lo schema: **JSON in → JSON out**

```python
# Pattern standard per ogni tool
import json, sys

def main(input_data: dict) -> dict:
    """
    Input:  { "keyword": "plenitude fixa time", "market": "IT", ... }
    Output: { "status": "success", "data": {...}, "errors": [] }
    """
    # ... execution logic ...
    return result

if __name__ == "__main__":
    input_data = json.loads(sys.argv[1]) if len(sys.argv) > 1 else {}
    result = main(input_data)
    print(json.dumps(result, indent=2, ensure_ascii=False))
```

---

## Stato Progetto

| Fase | Status | File Workflow | Note |
|---|---|---|---|
| 0 — Master | 🟡 Da creare | `00_master_landing_audit.md` | |
| 1 — Template | 🔴 In attesa HTML | `01_template_analysis.md` | HTML da incollare sopra |
| 2 — SEO | 🟡 Da creare | `02_seo_analysis.md` | |
| 3 — WCV | 🟡 Da creare | `03_wcv_analysis.md` | |
| 4 — UI/UX | 🟡 Da creare | `04_uiux_analysis.md` | |
| 5 — SERP | 🟡 Da creare | `05_serp_analysis.md` | |
| 6 — AI Citation | 🟡 Da creare | `06_ai_citation_analysis.md` | |
| 7 — Assembly | 🟡 Da creare | `07_template_assembly.md` | |

---

*Ultimo aggiornamento: 18 marzo 2026*
