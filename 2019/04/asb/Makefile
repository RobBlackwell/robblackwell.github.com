index.html : index.md references.bib Makefile
	pandoc --standalone \
		--mathjax \
		--from=markdown+yaml_metadata_block+smart \
		--filter pandoc-eqnos \
		--filter pandoc-citeproc \
		--csl elsevier-harvard.csl \
		--bibliography references.bib \
		index.md \
		-o index.html
