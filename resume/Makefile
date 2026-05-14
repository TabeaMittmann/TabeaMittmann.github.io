# Build Tabea's CV and Resume PDFs.
#
# Default: produces ../docs/Tabea-Mittmann-CV.pdf and ../docs/Tabea-Mittmann-Resume.pdf
# via three pdflatex passes (latexmk may not be installed; passes resolve refs/hyperlinks).
#
# Usage:
#   make           # build both
#   make cv        # build CV only
#   make resume    # build resume only
#   make clean     # remove build/ but keep the committed PDFs
#   make watch     # rebuild on every cv.tex / publications.tex / resume.tex change

LATEX           := pdflatex
LATEXOPTS       := -interaction=nonstopmode -halt-on-error -output-directory=build
CV_SOURCES      := cv.tex publications.tex
CV_TARGET       := ../docs/Tabea-Mittmann-CV.pdf
CVP_SOURCES     := cv-photo.tex publications.tex photo.jpg
CVP_TARGET      := ../docs/Tabea-Mittmann-CV-Photo.pdf
RES_SOURCES     := resume.tex
RES_TARGET      := ../docs/Tabea-Mittmann-Resume.pdf

.PHONY: all cv cv-photo resume clean watch

all: cv cv-photo resume

cv: $(CV_TARGET)
cv-photo: $(CVP_TARGET)
resume: $(RES_TARGET)

$(CV_TARGET): $(CV_SOURCES) | build
	$(LATEX) $(LATEXOPTS) cv.tex
	$(LATEX) $(LATEXOPTS) cv.tex
	$(LATEX) $(LATEXOPTS) cv.tex
	cp build/cv.pdf $(CV_TARGET)

$(CVP_TARGET): $(CVP_SOURCES) | build
	$(LATEX) $(LATEXOPTS) cv-photo.tex
	$(LATEX) $(LATEXOPTS) cv-photo.tex
	$(LATEX) $(LATEXOPTS) cv-photo.tex
	cp build/cv-photo.pdf $(CVP_TARGET)

$(RES_TARGET): $(RES_SOURCES) | build
	$(LATEX) $(LATEXOPTS) resume.tex
	$(LATEX) $(LATEXOPTS) resume.tex
	cp build/resume.pdf $(RES_TARGET)

build:
	mkdir -p build

clean:
	rm -rf build

watch:
	@command -v inotifywait >/dev/null || { echo "inotifywait required (apt install inotify-tools)"; exit 1; }
	@while true; do \
	  inotifywait -qe modify,close_write $(CV_SOURCES) $(CVP_SOURCES) $(RES_SOURCES); \
	  $(MAKE) all; \
	done
