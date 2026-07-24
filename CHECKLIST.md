# Checklist ceurart / ITADATA — da rileggere prima della submission

## Vietato (CEUR-WS non lo accetta)
- [ ] `\documentclass` senza `twocolumn` né `hf`.
- [ ] Nessuna modifica a margini, dimensioni font, interlinea, definizioni di paragrafi/liste.
- [ ] Nessun `\vspace` manuale per aggiustare gli spazi verticali.
- [ ] Nessun titolo di sezione simulato con grassetto/corsivo a inizio paragrafo.
- [ ] Acknowledgments scritti con l'ambiente `acknowledgments`, mai con `\section`/`\section*`.

## Obbligatorio
- [ ] Sectioning solo con `\section`, `\subsection`, `\subsubsection`, `\paragraph`, sempre numerati.
- [ ] Titolo: stile coerente (Title Case oppure normale, non mischiare), nessun a-capo dentro `\title`.
- [ ] Autori: uno per `\author`, nomi per esteso (mai iniziali), email quando possibile.
- [ ] Abstract in `\begin{abstract}...\end{abstract}`; keywords in `\begin{keywords}...\end{keywords}` separate da `\sep`.
- [ ] `\maketitle` è l'ultimo comando del front matter, subito dopo le keywords (non prima, non in mezzo alle marcature).
- [ ] Bibliografia: nomi completi negli autori delle voci `.bib`, con titolo/anno/volume/numero/pagine/DOI quando disponibili.
- [ ] `\bibliography{nomefile}` (senza `.bib`) subito prima di `\end{document}`.
- [ ] Didascalia delle tabelle SOPRA la tabella; le tabelle non si spezzano su più pagine, vanno posizionate vicino al punto in cui sono citate la prima volta nel testo (per questo "flottano").
- [ ] Tabelle sempre dentro l'ambiente `table`/`table*`, col contenuto tabellare annidato dentro `tabular`.
- [ ] Didascalia delle figure SOTTO la figura; materiale di terzi identificato esplicitamente; descrizione screen-reader-friendly.
- [ ] Appendice aperta con `\appendix`; dentro, sezioni lettere non numeri.
- [ ] Sezione "Declaration on Generative AI" presente e compilata (nessuno strumento usato, oppure tassonomia ceur-ws.org/genai-tax.html + dichiarazione di revisione/responsabilità).
- [ ] Ordine dei blocchi finali rispettato: Acknowledgments → Declaration on Generative AI → Bibliografia → Appendice (non è arbitrario, così è nel template).
- [ ] `\copyrightyear{}` riporta l'anno reale della conferenza (attualmente `2026`, coerente con ITADATA2026 — ricontrollare se cambia).
- [ ] Il numero in `\author[N]` corrisponde esattamente al numero in `\address[N]` per ogni autore (numeri disallineati collegano l'affiliazione all'autore sbagliato, senza warning di compilazione).

## Opzionale (da usare solo se serve)
- Varianti `\title`: `title` (default), `alt`, `sub`/`\subtitle`, `trans`, `transsub`.
- Marcature front matter (tutte prima di `\maketitle`): `\tnotemark`/`\tnotetext`, `\fnmark`/`\fntext`, `\cormark`/`\cortext`, `\nonumnote`.
- Opzioni `\author`: `style`, `prefix`, `suffix`, `degree`, `role`, `orcid`, `email`, `url`.
- `table` vs `table*` (tutta larghezza); `$...$`/`math` (inline) vs `equation` (numerata) vs `displaymath` (non numerata).

## Prima di compilare/sottomettere
- [ ] Rimosse tutte le sezioni "manuale" ereditate dal template (Introduction/Modifications/Template parameters/Front matter/Sectioning Commands/Tables/Math Equations demo, Citations demo, "Online Resources" finale).
- [ ] `\bibliography{biblio}` punta davvero alle chiavi citate nel testo (`biblio.bib` ora vive in questa cartella, verificato funzionante).
