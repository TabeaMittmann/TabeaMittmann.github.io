# Tabea Mittmann — LaTeX CV / Resume

Two LaTeX projects that share a publications list:

- `cv.tex` + `publications.tex` → `../docs/Tabea-Mittmann-CV.pdf` (academic CV, 2 pages)
- `resume.tex` → `../docs/Tabea-Mittmann-Resume.pdf` (1-page resume)

## Build

```bash
make           # both
make cv        # CV only
make resume    # resume only
make clean     # remove build/ (keeps committed PDFs)
make watch     # rebuild on every save (needs inotifywait)
```

Three `pdflatex` passes resolve refs/hyperlinks. PDFs land in `../docs/`.

## Editing

- Add/edit a publication in `publications.tex`. Both `cv.tex` and `resume.tex` read it (resume.tex inlines a manual condensed copy — keep them in sync).
- Header contact info appears in both `cv.tex` and `resume.tex` — update both when it changes.
- Adapted from Jake Gutierrez's CV template (MIT) — <https://github.com/jakegut/resume>.
