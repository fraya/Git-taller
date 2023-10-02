all: git.pdf

%.pdf: %.adoc
	docker run --rm -v `pwd`:/documents/:z docker.io/asciidoctor/docker-asciidoctor asciidoctor-pdf -a lang=es $<

clean:
	rm -f *.html *.pdf
