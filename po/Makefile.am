PACKAGE = @PACKAGE@

POFILES = $(wildcard *.po)
MOFILES = $(patsubst %.po,%.mo,$(POFILES)) 

top_builddir = ..
localedir=$(datadir)/locale

MSGFMT = msgfmt


# Stripped down -- w/o xgettext, but with msgmerge
#
update-po:
	rm -f $(PACKAGE).pot
	for cat in "" $(POFILES); do \
	  if test -z "$$cat"; then continue; fi; \
	  msgmerge -o tmp-$$cat $$cat $(PACKAGE).pot && \
	  mv -f tmp-$$cat $$cat; \
	done

all: $(MOFILES)

%.mo: %.po
	@echo "$(MSGFMT) -o $@ $<"; $(MSGFMT) -o t-$@ $< && mv t-$@ $@

install-data-hook: $(MOFILES)
	@for cat in "" $(MOFILES); do \
	  if test -z "$$cat"; then continue; fi; \
	  cat=`basename $$cat`; \
	  lang=`echo $$cat | sed -e 's/\.mo$$//'`; \
	  dir=$(localedir)/$$lang/LC_MESSAGES; \
	  $(mkdir_p) $(DESTDIR)$$dir; \
	  if test -r $$cat; then realcat=$$cat; else realcat=$(srcdir)/$$cat; fi; \
	  $(INSTALL) $$realcat $(DESTDIR)$$dir/$(PACKAGE).mo; \
	 done
