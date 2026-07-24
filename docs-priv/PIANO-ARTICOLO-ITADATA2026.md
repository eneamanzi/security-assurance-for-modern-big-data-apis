# Piano articolo ITADATA2026 — Security Assurance per API di servizi Big Data

*Derivato integralmente da: `Tesi/`, `Riassunto/`, `Presentazione/`, `docs-priv/`. Nessun contenuto esterno alla tesi.*

---

## 0. Vincolo temporale — da verificare subito

Il CFP nel materiale indica **full submission: 21 luglio 2026** (oggi) e abstract due il 1° luglio. Non ho trovato online un'ulteriore proroga. Prima di ogni altra cosa: verifica su `itadata.it` / CMT se la deadline è ancora quella e se l'abstract è già stato registrato (senza abstract registrato entro il 1° luglio la submission full potrebbe non essere aperta). Se la finestra è chiusa, il piano resta valido per il *regular paper* di un'altra call — la struttura non cambia.

Track di riferimento: **Research paper — regular (fino a 12 pagine + references)**, CEUR-ART 1 colonna, `ceurart.cls` già presente in `Article/`.

---

## 1. Come si scrive per ITADATA (analisi di stile)

Ho letto il template `Article/main.tex`, la `Article/CHECKLIST.md`, i volumi CEUR ITADATA 2022–2025 e, come campione rappresentativo del gruppo, il paper ITADATA2025 di Ardagna et al. (*A Secure AI-enabled Platform to Support the Automated Alzheimer's Disease Diagnosis*, Vol-4152/short79). Ne emerge un profilo molto preciso.

### 1.1 Struttura tipica

```
1. Introduction            → contesto big data, sfide, cosa fa il paper, contributi, outline
2. Related Work            → unico punto di stato dell'arte (a volte fuso in Intro nei short)
3. Reference scenario /
   The <Nome> platform     → il sistema proposto, architettura + metodologia
4. Experiments             → setup, dataset/testbed, risultati in tabella, discussione
5. Conclusions             → sintesi + future work
Acknowledgments (ambiente, non \section)
Declaration on Generative AI  (obbligatoria)
References
```

L'ultimo paragrafo dell'introduzione è **sempre** l'outline: *"The remainder of the paper is organized as follows: Section 2 …; Section 3 …; Section 4 draws some conclusions and highlights possible future work."* Va replicato letteralmente come pattern.

### 1.2 Registro

- Prima persona plurale: *we propose*, *we evaluate*, *our approach*. Mai "this thesis", mai "the author".
- Presente per il metodo, passato per gli esperimenti.
- **Frasi dichiarative, corte, senza subordinate a cascata.** La prosa della tesi è ottima ma è prosa da tesi: periodi lunghi, connettivi argomentativi, riprese anaforiche. Va **compressa di circa 4:1**, non semplicemente tagliata.
- Il lettore è esperto: niente definizioni di REST, microservizi, JWT, CI/CD. Si entra direttamente nel merito.
- Citazioni numeriche `[1]`, `[2, 3]`, integrate nella frase.
- Nessun elenco puntato decorativo: liste solo quando l'enumerazione è reale (i componenti dell'architettura, i contributi).

### 1.3 Vincoli formali (da `CHECKLIST.md`, non negoziabili)

- `\documentclass{ceurart}` senza `twocolumn` né `hf`.
- Sectioning solo con `\section`/`\subsection`/`\subsubsection`/`\paragraph`, **sempre numerati**. Vietato simulare titoli col grassetto.
- Nessun `\vspace` manuale, nessuna modifica a margini/font/interlinea.
- Caption tabelle **sopra**, caption figure **sotto**.
- Un `\author` per autore, nomi per esteso, email, ORCID; `\author[N]` deve corrispondere a `\address[N]`.
- `\maketitle` ultimo comando del front matter, dopo le keywords.
- Ordine blocchi finali: Acknowledgments → Declaration on Generative AI → Bibliography → Appendix.
- `\copyrightyear{2026}`, `\conference{ITADATA2026: …, November 10--12, 2026, Bari, Italy}`.
- Rimuovere tutte le sezioni-manuale ereditate dal template.

### 1.4 Budget pagine

ITADATA dichiara ~2500 caratteri/pagina ≈ **380–400 parole/pagina**. 12 pagine + references significano circa **4.000–4.300 parole di prosa effettiva**, una volta sottratto lo spazio di figure e tabelle. La tesi ha ~5 capitoli di contenuto: il fattore di compressione è drastico e va progettato, non subìto.

---

## 2. Il riposizionamento richiesto dal relatore

Il prof ha dato una direzione precisa, e cambia l'inquadramento (non il contenuto) del lavoro:

> *i big dati sono offerti come servizi, dipendono anche dal servizio erogato e ci concentriamo su quello · l'obiettivo deve essere contestualizzarlo nell'ambiente big data · assurance ecc. deve valere a tutti i livelli e sempre, noi ci concentriamo sull'interfaccia ovvero API · sviluppo di pipeline big data dovrebbe essere lo scenario, noi analizziamo il sistema che offre questo servizio*

Tradotto in una catena argomentativa a cinque passi, da usare come **spina dorsale dell'introduzione**:

1. **I sistemi big data sono erogati come servizi.** Il valore non nasce da un monolite analitico ma da pipeline composte di servizi (ingestion, storage, processing, serving) orchestrati nel continuum edge-cloud.
2. **La trustworthiness di un sistema big data deve valere a tutti i livelli** — infrastruttura, piattaforma/pipeline, interfaccia — e deve valere in modo continuo, non come attributo certificato una tantum.
3. **La letteratura di assurance big data ha coperto i livelli bassi e intermedi**: processi di assurance per la trustworthiness delle pipeline, assurance basata su SLA, certificazione multi-dimensionale di sistemi distribuiti, DevSecOps per big data analytics. Il livello **interfaccia** — l'API REST attraverso cui *ogni* accesso ai dati transita — è quello meno coperto.
4. **L'interfaccia è però il punto di enforcement effettivo**: autenticazione, autorizzazione a livello di oggetto ed esposizione dei dati sono applicate lì, tipicamente da un gateway. Una garanzia dimostrata a livello di pipeline non si propaga end-to-end se l'API che espone quella pipeline è permeabile.
5. **E lì il testing automatico non arriva**: la quasi totalità dei contributi verifica conformità funzionale (72/92 nella survey di Golmohammadi et al., security testing esplicito in 8/92) e il problema dell'oracolo di sicurezza resta il meno risolto.

→ **Da qui il paper**: un approccio contract-driven alla security assurance dell'interfaccia dei servizi big data, con oracoli specificati derivati dalla semantica del controllo e un motore deterministico integrabile in pipeline di rilascio.

### 2.1 Come far diventare Forgejo uno scenario big data credibile

Indicazione del prof: *"quando arrivo agli esperimenti racconto dello scenario big data in questo modo di Forgejo cercando di condurlo al servizio per caricare pipeline big data"*.

La strada onesta — e l'unica che regge in peer review — è **non** dire che Forgejo è una piattaforma big data. È dire questo:

> Lo scenario di riferimento è lo sviluppo e il rilascio di pipeline big data. In questo scenario, il codice della pipeline, i descrittori di deployment, gli artefatti e i webhook di orchestrazione sono gestiti da un **servizio di repository esposto via API REST dietro un gateway**: è il servizio attraverso cui i data engineer di tenant diversi caricano, versionano e attivano le proprie pipeline. Quel servizio è parte del perimetro di trustworthiness della pipeline: un difetto di autorizzazione a livello di oggetto su quell'API espone il codice e la configurazione della pipeline di un altro tenant; un webhook non verificato consente di innescare l'esecuzione di una pipeline non autorizzata.

Forgejo 14.0.3 dietro Kong 3.9 è quindi **un'istanza reale di quel ruolo**, non una scelta di comodo: gestisce repository, utenti, token e webhook, espone una OAS nativa, ha autenticazione reale e ruoli distinti. E — punto chiave da dire esplicitamente — il tool **non contiene alcun riferimento a Forgejo**: cambia il `config.yaml`, non il codice. Il che è precisamente ciò che rende il risultato trasferibile a un servizio di pipeline reale.

**Da non fare**: non affermare che sono state testate pipeline Spark/Flink/Airflow, non introdurre metriche di volume dati, non gonfiare lo scenario oltre ciò che il testbed dimostra. La sezione "Threats to validity" deve dichiarare che la validazione è su un servizio rappresentativo per proprietà strutturali, non su una piattaforma di analytics in produzione.

---

## 3. Struttura dell'articolo con budget

| § | Sezione | Pag. | Contenuto |
|---|---------|-----:|-----------|
| — | Abstract + keywords | 0,4 | ~180 parole |
| 1 | Introduction | 1,5 | **DA SCRIVERE PER ULTIMA** — catena §2 sopra, gap, contributi, outline |
| 2 | Related Work | 1,3 | Unico punto di SOTA. Due filoni + Tabella 1 |
| 3 | Reference Scenario and Threat Surface | 0,8 | Pipeline big data come servizi; il gateway come punto di enforcement; Figura 1 |
| 4 | Contract-Driven API Assurance Methodology | 3,2 | **Cuore.** Principi, tassonomia domini, **oracoli specificati**, box gradient, motore deterministico |
| 5 | Experimental Evaluation | 3,6 | Testbed, design, risultati, determinismo, footprint, threats |
| 6 | Conclusions | 0,4 | Sintesi + future work |
| — | Ack. + GenAI + References | 1,2 | ~28 voci |
| | **Totale** | **~12,4** | da limare in revisione |

### Dettaglio §4 (il contributo — massima evidenza)

- **4.1 Design principles** (~0,5 p.) — agnosticismo applicativo (tutta la conoscenza del target da OAS + config, zero hardcoding); contract-driven (OAS come fondamento contrattuale: costruzione di probe valide, delimitazione della superficie, criterio strutturale primario); config-driven; determinismo. In prosa compressa, senza sottosezioni.
- **4.2 An eight-domain assurance taxonomy** (~0,6 p.) — **Tabella 2** (D0–D7). Catena di precondizioni D0→D1→D2. D5 dichiarato predisposto ma non implementato (onestà epistemica: va detto).
- **4.3 Specified oracles for security guarantees** (~1,0 p.) — **la sottosezione più importante del paper.** Barr et al.: oracolo come funzione parziale `D: T_A → B`; tassonomia specified/derived/implicit/no-oracle; scelta di *specified oracles* fissati a priori; **Tabella 3 (nuova)** che mappa classe di controllo → sorgente del criterio → esempio di verdetto; oracolo a tre esiti (ENFORCED / BYPASSED / INCONCLUSIVE) come meccanismo anti-falsi-positivi; conseguenza: verdetto invariante ⇒ riproducibilità. Chiudere col limite: la correttezza del verdetto è condizionale alla correttezza del contratto.
- **4.4 Privilege-aware coverage: the box gradient** (~0,4 p.) — BLACK/GREY/WHITE come *prodotto delle precondizioni*, non etichetta retroattiva. Testo + micro-tabella o prosa se serve spazio.
- **4.5 Deterministic assessment engine** (~0,7 p.) — pipeline a 7 fasi (4 bloccanti + 3 non bloccanti), DAG scheduler con `TopologicalSorter`, ordinamento lessicografico intra-batch come unica fonte di ordine totale, rilevazione cicli e stall, teardown LIFO, exit code semantici 0/1/2/10. **Figura 1** (architettura) va citata qui o in §3.

### Dettaglio §5

- **5.1 Testbed** — Forgejo 14.0.3 / Kong 3.9 DB-less / PostgreSQL 15-alpine su `lab-net`, container effimero di provisioning (`thesis-admin`, `user-a`, `user-b`), porte 8000/8443/8001. DB-less come garanzia di stato identico a ogni avvio. **Figura 2**.
- **5.2 Experimental design** — il punto metodologicamente forte: *validare su un ambiente perfettamente configurato produce tutti PASS ed è tautologico*. Il testbed è configurato per produrre **FAIL attesi** (rate-limiting disattivato, certificato TLS self-signed, `security` globale nella OAS) e **PASS attesi** (security header via `response-transformer`, `KONG_HEADERS=off`, redirect 301 HTTP→HTTPS).
- **5.3 Results** — **Tabella 4** (18 test / status / findings). 9 PASS, 7 FAIL, 2 SKIP, 0 ERROR, 98 findings, pass rate 56.2%. Lettura trasversale: i 7 FAIL corrispondono uno-a-uno alle condizioni configurate; il test 1.1 da solo produce 74/98 findings (75,5%); i 2 SKIP dimostrano la degradazione documentata (0.3 nessun endpoint deprecato, 1.6 nessun session store esterno).
- **5.4 The specification–behaviour discrepancy** — paragrafo dedicato, **è il risultato più interessante del paper**: Forgejo dichiara `security` a livello globale, ma alcuni endpoint sono genuinamente pubblici. Il tool li marca BYPASSED. Il verdetto è contrattualmente corretto e mostra esattamente la classe di anomalia che il contract-driven testing esiste per rilevare: il disallineamento fra ciò che il contratto promette e ciò che il sistema fa. In un contesto big data questo è precisamente il rischio: la specifica pubblicata di un servizio di pipeline è ciò su cui i consumatori basano le proprie assunzioni.
- **5.5 Determinism and idempotence** — **Tabella 5**: due run indipendenti, contatori byte-identici, status per-test identici, exit code identico, Δ wall-clock 0,32% (286,04 s vs 286,96 s) attribuibile a latenza di rete. Teardown: 0 risorse residue su entrambe le run, registro LIFO drenato (4 risorse, 0 fallimenti).
- **5.6 Computational footprint** — ~290 s wall-clock, 35 s user + 41 s sys = 76 s CPU (26%): regime **dominato dalla latenza di rete**, non dal calcolo. Picco memoria 287–295 MB, output su disco 4,5 MB. Compatibile con runner CI/CD. Topologia DAG: Phase A 16 test, Phase B 2 test (1.4 e 2.1 dipendono da 1.1).
- **5.7 Threats to validity** — (i) dipendenza dalla qualità della OAS (*ground-truth paradox*); (ii) validazione su singolo target scelto per proprietà strutturali; (iii) copertura WHITE_BOX subordinata all'accesso all'Admin API, non disponibile nei deployment cloud managed; (iv) D5 non implementato.

---

## 4. Figure e tabelle (riuso dalla tesi)

Con 12 pagine il tetto realistico è **2 figure + 5 tabelle**. Selezione:

| Elemento | Sorgente in repo | Azione |
|---|---|---|
| **Fig. 1** — Architettura + flusso di assessment | `Tesi/assets/figures/3-architettura-apiguard.tex` | riusare, semplificando le etichette dei moduli |
| **Fig. 2** — Topologia del testbed | `Tesi/assets/figures/5-testbed-topology.tex` | riusare così com'è |
| **Tab. 1** — Approcci contract-driven a confronto | `Tesi/assets/tables/2-contract-driven-tools.tex` | comprimere a 4 colonne × 7 righe |
| **Tab. 2** — Otto domini di assurance | `Tesi/assets/tables/3-domini.tex` | tradurre, accorciare la colonna "ambito" |
| **Tab. 3** — Sorgenti degli oracoli **(nuova)** | sintesi da §3 metodologia + catalogo test | **da costruire**, vedi §5 |
| **Tab. 4** — Risultati per test | `Tesi/assets/tables/5-risultati-per-test.tex` | riusare |
| **Tab. 5** — KPI idempotenza + footprint | `5-idempotenza-kpi.tex` + `5-performance-baseline.tex` | **fondere in una sola tabella** |

**Da tagliare** (non entrano): box gradient come tabella autonoma (→ prosa), tabella versioni testbed (→ prosa), figura DAG topology, figura coverage map, figura pipeline a 7 fasi, figura decision tree CI/CD.

### Bozza della Tabella 3 (contributo nuovo, da rifinire sul catalogo test)

| Class of guarantee | Source of the evaluation criterion | Verdict rule (example) |
|---|---|---|
| Response structural conformance | OAS contractual schema | field absent from the declared response schema → violation |
| Protocol-level requirements | Published technical standards (RFC, TLS baselines) | missing `Sunset` header on a deprecated endpoint → violation |
| Authentication enforcement | OAS `security` markers + differential probing | `2xx` on a credential-less request to an endpoint declared as protected → BYPASSED |
| Authorization (RBAC / object level) | Comparison of responses under distinct role credentials | resource of role A readable with credentials of role B → violation |
| Infrastructural guarantees | Direct inspection of the gateway configuration plane | rate-limiting plugin absent from the active configuration → violation |
| Attack-surface conformance | Set difference between responding and documented endpoints | endpoint responding but not declared → shadow API |

---

## 5. Bibliografia

### 5.1 Già in `Tesi/biblio.bib` — riusabili senza modifiche

`golmohammadiTestingRESTfulAPIs2023`, `barrOracleProblemSoftware2015`, `atlidakisRESTlerStatefulREST2019a`, `atlidakisCheckingSecurityProperties2020`, `dengNAUTILUSAutomatedRESTful2023`, `schemathesisDerivingSemanticsawareFuzzers2022`, `alonsoAGORAAutomatedGeneration2023`, `alonsoAgora+TestOracleGeneration2025`, `alonsoSATORIStaticTest2025`, `sahinEnhancingRESTAPI2026`, `corradiniAutomatedBlackBoxTesting2023`, `corradiniRESTAPIs2024`, `duVulnerabilityorientedTestingRESTful2024`, `chandramouliGuidelinesAPIProtection2026` (NIST SP 800-228), `OWASPTop10`, `roseZeroTrustArchitecture2020` (NIST SP 800-207), `OpenAPISpecificationV320`, `dragoniMicroservicesYesterdayToday2017`, `forgejoContributors2026`, `kongGateway2026`, `ProjectdiscoveryNuclei2026`.

### 5.2 Da aggiungere — ancoraggio big data / assurance (mancano tutte in `biblio.bib`)

Sono il ponte verso ITADATA e verso il gruppo: **senza queste il paper non è ITADATA-ready.**

1. M. Anisetti, C. A. Ardagna, F. Berto, *An assurance process for Big Data trustworthiness*, Future Generation Computer Systems 146 (2023) 34–46. DOI 10.1016/j.future.2023.04.003
2. C. A. Ardagna, N. Bena, C. Hébert, M. Krotsiani, C. Kloukinas, G. Spanoudakis, *Big Data Assurance: An Approach Based on Service-Level Agreements*, Big Data 11(3) (2023). DOI 10.1089/big.2021.0369
3. M. Anisetti, N. Bena, F. Berto, G. Jeon, *A DevSecOps-based Assurance Process for Big Data Analytics*, ICWS 2022, pp. 1–10.
4. M. Anisetti, C. A. Ardagna, N. Bena, *Multi-Dimensional Certification of Modern Distributed Systems*, IEEE Trans. on Services Computing 16(3) (2023).
5. M. Anisetti, C. A. Ardagna, M. Banzi, F. Berto, R. Bondaruc, E. Damiani, A. Pedretti, A. Pisati, A. Retico, *MUSA: A Platform for Data-Intensive Services in Edge-Cloud Continuum*, AINA 2024, pp. 327–337.
6. C. A. Ardagna, V. Bellandi, M. Luzzara, A. Polimeno, *Data Pipelines Assessment: The Role of Data Engine Deployment Models*, SEBD 2024, pp. 538–550.
7. F. Berto, C. A. Ardagna, M. Banzi, M. Anisetti, *Assurance in Advanced 5G Edge Continuum*, IEEE Access 12 (2024) 178659–178671.

**Verifica prima della submission**: pagine, volumi e DOI vanno controllati uno per uno (CEUR richiede autori per esteso, titolo, anno, volume, numero, pagine, DOI). Non ho verificato ogni campo bibliografico direttamente sulle fonti primarie.

Distribuzione consigliata: 1–2–3–4 in §1 e §2 (filone assurance big data), 5–6–7 in §3 (scenario), il resto in §2 (filone REST API testing).

---

## 6. Cosa va scritto ex novo e cosa si riusa

| Sezione articolo | Origine | Lavoro |
|---|---|---|
| Abstract | `Article/docs-priv/abstarct-short.md` | già in inglese e già big-data-oriented → limare a ~180 parole |
| §1 Introduction | — | **ex novo, per ultima** (indicazione del prof) |
| §2 Related Work | Tesi §2.4 + Tab. 2-contract-driven-tools | traduzione + compressione 4:1 |
| §3 Reference Scenario | — | **ex novo**: è il pezzo che manca del tutto nella tesi |
| §4 Methodology | Tesi cap. 3 (+ presentazione slide 5–8) | traduzione + compressione; §4.3 va *espansa* rispetto al peso relativo che ha in tesi |
| §5 Experiments | Tesi cap. 5 + `AUDIT_milestone1_release.md` | traduzione + selezione dei dati; tutti i numeri già tracciati |
| §5.7 Threats | Tesi cap. 6 §6.1 | compressione forte |
| §6 Conclusions | Tesi cap. 7 | riscrittura breve |

Nota su cosa **non** entra: pipeline a 7 fasi in dettaglio implementativo, gerarchia dei connector, `BaseTest`/`ExternalToolTest`, evidence store JSONL, sanitizzazione credenziali, modalità fail-fast, distinzione Finding/InfoNote, tutto il cap. 4 della tesi. Sono contenuti di implementazione: in un paper da 12 pagine occupano spazio che serve agli oracoli e agli esperimenti. Se un revisore li chiede, stanno nella tesi.

---

## 7. Titolo, abstract, keywords

**Titolo proposto** (Title Case coerente, nessun a-capo):

> **Contract-Driven Security Assurance for the APIs of Big Data Services**

Alternative: *Assurance at the Interface: Contract-Driven Security Verification of Big Data Service APIs* · *Towards Reproducible Security Assurance of Big Data Service Interfaces*.

**Keywords**: `Big Data assurance \sep API security \sep REST \sep contract-driven testing \sep test oracles \sep reproducibility`

L'abstract redatto è nel file `main-itadata2026.tex`.

---

## 8. Piano operativo

1. Verificare deadline e stato dell'abstract su CMT. *(bloccante)*
2. Concordare col relatore titolo, ordine autori e affiliazioni.
3. Compilare `main-itadata2026.tex` così com'è per fissare il baseline di pagine reale — il budget del §3 è stimato, va misurato.
4. Portare le 7 voci bibliografiche big data in `biblio.bib` verificandone i campi.
5. Riprendere e tradurre Fig. 1 e Fig. 2 dal TikZ della tesi.
6. Rifinire la Tabella 3 (oracoli) sul catalogo test effettivo — è il contributo che va difeso in review.
7. Scrivere l'introduzione per ultima, sulla catena a cinque passi del §2.
8. Passata di compressione finale contro il budget pagine e contro la `CHECKLIST.md`.
9. Compilare la Declaration on Generative AI secondo la tassonomia `ceur-ws.org/genai-tax.html` (obbligatoria e verificata dai chair).
