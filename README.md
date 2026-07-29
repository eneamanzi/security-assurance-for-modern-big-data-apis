# LaTeX user template and guide

To compile user guide:

1. `pdflatex main`
2. `bibtex main`
3. `pdflatex main`
4. `pdflatex main`
or

use the makefile:

`make`

## Mirror su GitHub

Questa repo è collegata in **push mirror** (one-way, sola lettura dall'altra parte) verso [github.com/eneamanzi/security-assurance-for-modern-big-data-apis](https://github.com/eneamanzi/security-assurance-for-modern-big-data-apis), configurato via GitLab → Settings → Repository → Mirroring repositories (autenticazione SSH con deploy key dedicata). Ogni push su questa repo GitLab si riflette automaticamente su GitHub entro qualche minuto — non serve fare nulla lato GitHub, è solo una copia per visibilità/portfolio.
