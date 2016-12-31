STYLE=tango
TOCTITLE='Table des matières'

INPUT=DOCS.md
OUTPUT=index.html


all: 
	pandoc ${INPUT} -s -o ${OUTPUT} \
			 --filter ./tikz.py \
		    --highlight-style ${STYLE} \
		    --toc -V toctitle:${TOCTITLE}
			 

clean:
	rm -rf tikz-images
	rm ${OUTPUT}


open: all
	open ${OUTPUT}
