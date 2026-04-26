# tabeamittmann.github.io

Personal site for Tabea C. Mittmann. Static GitHub Pages — hand-written HTML/CSS, no build step.

## Layout

- `index.html` — single-page-scroll homepage (hero, currently, about, publications, experience, honors, languages, contact)
- `cv/index.html` — formal CV page (also single-page scroll)
- `styles.css` — all site styles, shared across both pages
- `resume/` — separate LaTeX project that builds the PDF CV and 1-page resume into `docs/`
- `docs/` — built PDFs (committed): `Tabea-Mittmann-CV.pdf`, `Tabea-Mittmann-Resume.pdf`
- `data/` — source material (CV PDF, LinkedIn export) — gitignored; lives only on the local machine

## Local preview

```bash
python3 -m http.server
# open http://localhost:8000/
```

## Rebuilding the PDFs

```bash
cd resume
make           # builds both
make cv        # CV only
make resume    # resume only
```
