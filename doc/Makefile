git.pdf: git.adoc
	docker run --rm -v `pwd`:/documents/:z docker.io/asciidoctor/docker-asciidoctor asciidoctor-pdf -a lang=es git.adoc

git.html: git.adoc git-basico.adoc
	podman run -it -v `pwd`:/documents/:z docker.io/asciidoctor/docker-asciidoctor asciidoctor -a lang=es git.adoc

clean:
	rm -f *.html *.pdf
