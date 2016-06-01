mainfiles=main_plos 
suppfiles=S1_Text S2_Text S3_Text S4_Text S1_Figure S1_Table
TEX=latex
TEXOPT=--interaction=batchmode -synctex=1
BIBTEX=bibtex

.PHONY: clean clean-all show zip plos supp all final
.SECONDARY:


plos:	main_plos.pdf
supp:	$(addsuffix .pdf, $(suppfiles))
all:	plos supp	
show:	main_plos.pdf		
	evince $< 

final-pdf: main_final.pdf

final: plos
	latexpand --keep-comments --expand-bbl main_plos.bbl \
	    main_plos.tex > main_final.tex
	sed -i 's/\\makeatletter{}//g' main_final.tex	
clean:
	rm -f *.{aux,bak,out,bbl,blg,synctex.gz,idx,toc,log}

clean-all:	clean
	rm -f *.{dvi,ps,pdf}

tar:
	git archive --format=tar.gz HEAD > /tmp/Article_Plos.tar.gz

zip:
	git archive --format=zip HEAD > /tmp/Article_Plos.zip

%.pdf:	%.dvi
	dvipdf $<

main_plos.dvi:	main_plos.tex main_plos.aux \
    		$(addsuffix .aux, $(suppfiles))
	$(BIBTEX) $(basename $<) || true
	$(TEX) $(TEXOPT) $(basename $<)
	$(TEX) $(TEXOPT) $(basename $<)

supp_S%.dvi:	supp_S%.tex main_plos.aux \
    		$(addsuffix .aux, $(suppfiles))
	$(BIBTEX) $(basename $<) || true
	$(TEX) $(TEXOPT) $(basename $<)
	$(TEX) $(TEXOPT) $(basename $<)

%.aux:	%.tex
	$(TEX) $(TEXOPT) $*

