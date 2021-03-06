SHELL = /usr/bin/env bash

# configure engine
## LaTeX engine
### LaTeX workflow: pdf; xelatex; lualatex
latexmkEngine = pdf
### pandoc workflow: pdflatex; xelatex; lualatex
pandocEngine = pdflatex
## HTML
HTMLVersion = html5
## ePub
ePubVersion = epub

pantable = ../pantable/pantable.py
pantable2csv = ../pantable/pantable2csv.py

CSSURL:=https://cdn.jsdelivr.net/gh/ickc/markdown-latex-css

# command line arguments
pandocArgCommon = -f markdown+autolink_bare_uris-fancy_lists --toc -V linkcolorblue -V citecolor=blue -V urlcolor=blue -V toccolor=blue --pdf-engine=$(pandocEngine) -M date="`date "+%B %e, %Y"`"
# Workbooks
## MD
pandocArgMD = -f markdown+abbreviations+autolink_bare_uris+markdown_attribute+mmd_header_identifiers+mmd_link_attributes+mmd_title_block+tex_math_double_backslash-latex_macros-auto_identifiers -t markdown+raw_tex-native_spans-simple_tables-multiline_tables-grid_tables-latex_macros -s --wrap=none --column=999 --atx-headers --reference-location=block --file-scope
## TeX/PDF
### LaTeX workflow
latexmkArg = -$(latexmkEngine)
pandocArgFragment = $(pandocArgCommon) --filter=$(pantable)
### pandoc workflow
pandocArgStandalone = $(pandocArgFragment) --toc-depth=1 -s -N
## HTML/ePub
pandocArgHTML = $(pandocArgFragment) -t $(HTMLVersion) --toc-depth=2 -s -N -c $(CSSURL)/css/common.min.css -c $(CSSURL)/fonts/fonts.min.css
pandocArgePub = $(pandocArgHTML) -t $(ePubVersion) --epub-chapter-level=2
# GitHub README
pandocArgReadmeGitHub = $(pandocArgFragment) --toc-depth=2 -s -t markdown_github --reference-location=block
pandocArgReadmePypi = $(pandocArgFragment) -s -t rst --reference-location=block -f markdown+autolink_bare_uris-fancy_lists-implicit_header_references

docs = $(wildcard *.md)
docsPdf = $(patsubst %.md,%.pdf,$(docs))
docsAll = $(docsPdf) index.html ../README.md ../README.rst ../README.html

# Main Targets ########################################################################################################################################################################################

all: $(docsAll)

clean:
	rm -f $(docsAll)

# Making dependancies #################################################################################################################################################################################

%.pdf: %.md
	pandoc $(pandocArgStandalone) -o $@ $<
%.html: %.md
	pandoc $(pandocArgHTML) $< -o $@

# readme
## index.html
index.html: badges.markdown README.md
	pandoc $(pandocArgHTML) $^ -o $@
## GitHub README
../README.md: badges.markdown README.md
	printf "%s\n\n" "<!--This README is auto-generated from \`docs/README.md\`. Do not edit this file directly.-->" > $@
	pandoc $(pandocArgReadmeGitHub) $^ >> $@
## PyPI README
../README.rst: badges.markdown README.md
	printf "%s\n\n" ".. This README is auto-generated from \`docs/README.md\`. Do not edit this file directly." > $@
	pandoc $(pandocArgReadmePypi) $^ >> $@
../README.html: ../README.rst
	rst2html.py $< > $@

print-%:
	$(info $* = $($*))
