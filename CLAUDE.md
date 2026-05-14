# tabeamittmann.github.io — site notes

Personal site for Tabea C. Mittmann (MD candidate, University of Münster). Static GitHub Pages, hand-written HTML/CSS, no build step.

## File map

- `index.html` — homepage (single-page scroll: hero, currently, about, publications, experience, honors, languages, contact)
- `cv/index.html` — formal CV page (single-page scroll: summary, education, research, publications, honors, teaching, skills, languages)
- `styles.css` — all site styles, shared across both pages
- `data/` — source material from the user (CV PDF, LinkedIn export). Not deployed.
- `resume/` — separate LaTeX project building three PDFs:
  - `cv.tex` + `publications.tex` → `docs/Tabea-Mittmann-CV.pdf` (2 pages)
  - `cv-photo.tex` + `publications.tex` + `photo.jpg` → `docs/Tabea-Mittmann-CV-Photo.pdf` (Lebenslauf-style variant with headshot in the header)
  - `resume.tex` → `docs/Tabea-Mittmann-Resume.pdf` (1 page)
- `docs/` — built PDFs (committed; linked from `cv/index.html`).

## Design system

Editorial / book-frontmatter aesthetic. Two-column layout: sticky sidebar with the user's name + nav + contact, and a serif content column on the right. Distinctly different from the sibling site `ashirborah.github.io`, which uses a vertical-scroll-with-hero pattern.

- **Palette** (`:root` of `styles.css`):
  - `--bg: #f6f3eb` (warm cream paper)
  - `--ink: #1a1612` (warm near-black, headings)
  - `--text: #2a2622` (body)
  - `--muted: #7a7268`
  - `--rule: #d8d2c2`
  - `--accent: #6e3848` (muted plum — used sparingly for italic accents, links, Roman numerals)
  - Auto dark mode via `prefers-color-scheme`.
- **Type**: All Newsreader (Google Fonts variable serif) for headings and body — italics carry a lot of weight in this design. JetBrains Mono for tabular details (years, periods, DOIs, pub numbers).
- **Layout**: `.page` is a 260px sidebar + flexible content column, max 1040px wide, 80px outer padding. Below 900px the sidebar collapses above the content (no longer sticky).
- **Section heading pattern**: `<p class="eyebrow"><span class="roman">i.</span> Section name</p>` — small-caps tracked label with a lowercase Roman numeral in italic plum. The `<h2>` is hidden (the eyebrow replaces it visually) but kept in markup for semantic structure when needed.
- **No hero, no typing animation, no scroll-prompt arrow, no fade-in** — those are sibling-site patterns; this site is intentionally quieter.
- **Active nav state**: scrolling syncs an `.active` class onto the matching sidebar link.

## Updating content

- Both `index.html` and `cv/index.html` are hand-edited HTML — make changes directly.
- The publications list lives in three places that must stay in sync:
  - `index.html` (Publications section)
  - `cv/index.html` (Publications section)
  - `resume/publications.tex` (shared by `cv.tex` and `cv-photo.tex`; `resume.tex` keeps its own inline list)
- After changing `resume/*.tex` or `resume/publications.tex`, rebuild PDFs:
  ```bash
  cd resume && make
  ```
  PDFs land in `docs/` and are committed.

## Don'ts

- Do not introduce a build step (Jekyll, Vite, Astro, etc.) without explicit user approval.
- Do not commit transient LaTeX artifacts (`build/`, `.aux`, `.log`) — `.gitignore` already covers these.
- Do not add a `writing/` channel — this site intentionally does not have one (unlike the sibling site).

## Verification

After any HTML change, run from the repo root:

```bash
python3 -c "
from html.parser import HTMLParser
class V(HTMLParser):
    def __init__(self):
        super().__init__()
        self.stack = []; self.errors = []
        self.void = {'meta','link','br','hr','img','input','area','base','col','embed','source','track','wbr'}
    def handle_starttag(self, tag, attrs):
        if tag not in self.void: self.stack.append((tag, self.getpos()))
    def handle_endtag(self, tag):
        if not self.stack: self.errors.append(f'orphan </{tag}> at {self.getpos()}'); return
        top, pos = self.stack[-1]
        if top != tag: self.errors.append(f'expected </{top}> opened {pos} got </{tag}> at {self.getpos()}')
        self.stack.pop()
import glob
for f in sorted(glob.glob('**/*.html', recursive=True)):
    if f.startswith('resume/'): continue
    v = V(); v.feed(open(f).read())
    print(f'{f}: {\"OK\" if not v.stack and not v.errors else \"FAIL\"}')
"
```

Then preview locally: `python3 -m http.server`, open `http://localhost:8000/`.
