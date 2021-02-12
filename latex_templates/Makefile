# A paper build file with some extra customizations.
# This assumes latexmk and does not use pdflatex directly.

# Change this to the name of the current paper, if desired:
PAPER=paper

OUTDIR= ./build
MKOPTS= -outdir=${OUTDIR} -pdf
LATEXMK=latexmk ${MKOPTS}

paper: ${OUTDIR}
	${LATEXMK} ${PAPER}.tex

# If you have trouble with latexmk for any reason, you can try this:
old:
	pdflatex ${PAPER}.tex

# Continuous building
cont:
continuous: ${OUTDIR}
	${LATEXMK} -pvc -interaction=nonstopmode ${PAPER}.tex

${OUTDIR}:
	mkdir -p ${OUTDIR}

# Build with editorial markup rather than the final pdf:
# ============================================================
ed: editingmarks
editingmarks:
	touch activateeditingmarks
	${LATEXMK} -g ${PAPER}.tex

plain: 
	rm -f activateeditingmarks
	${LATEXMK} -g ${PAPER}.tex

both: 
	touch activateeditingmarks
	${LATEXMK} -g ${PAPER}.tex
	cp ${OUTDIR}/${PAPER}.pdf ./wmarks.pdf

	rm -f activateeditingmarks
	${LATEXMK} -g ${PAPER}.tex
	cp ${OUTDIR}/${PAPER}.pdf ./plain.pdf

# A little shorthand for Ryan.  Easier on the eyes:
grey:
	touch activategreybg.tex
	${MAKE} paper
white:
	rm -f activategreybg.tex
	${MAKE} paper

# ============================================================

UNAME := $(shell uname)

# Open the PDF in a viewer after building:
open: ${OUTDIR}/${PAPER}.pdf
ifeq (${UNAME}, Darwin)
	open ${OUTDIR}/${PAPER}.pdf
endif
ifeq (${UNAME}, Linux)
	evince ${OUTDIR}/${PAPER}.pdf &
endif

# Skim is great for auto-reloading papers on Mac OS, even if it isn't
# set up as the default:
skim:
	open -a skim ${OUTDIR}/${PAPER}.pdf

# Export a source tarball for camera-ready version
# ============================================================

# Most common formats that we use
SOURCES ?= "*.tex" "*.bib" "*.bst" "*.cls" "*.eps"
# Pass in extra sources to be archived (properly escaped)
SOURCES_EXTRA ?=

sources: sources-zip

sources-zip:
	git ls-files --recurse-submodules $(SOURCES) $(SOURCES_EXTRA) | xargs zip sources.zip

sources-targz:
	git ls-files --recurse-submodules $(SOURCES) $(SOURCES_EXTRA) | xargs tar acvf sources.tar.gz

# ============================================================

clean:
	cd ${OUTDIR}; rm -f *.out *.aux *.bbl *.log *.blg *.fls *.fdb_latexmk ${PAPER}.pdf

.PHONY: editingmarks ed paper plain grey white continuous cont skim old sources sources-zip sources-targz
