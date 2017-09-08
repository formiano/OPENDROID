dist_noinst_SCRIPTS = xml2po.py

LANGS = ar bg ca cs da de el en en_GB es et fa fi fr fy he hr hu is it ku lt lv nl no nb pl pt pt_BR ro ru sv sk sl sr th tr uk
LANGMO = $(LANGS:=.mo)
LANGPO = $(LANGS:=.po)

EXTRA_DIST = $(LANGPO)

# the TRANSLATORS: allows putting translation comments before the to-be-translated line.
opendroid-py.pot: $(top_srcdir)/src/*.py
	$(XGETTEXT) --no-wrap -L Python --from-code=UTF-8 --add-comments="TRANSLATORS:" -d @PACKAGE_NAME@ -s -o $@ $^

opendroid-xml.pot: $(srcdir)/xml2po.py $(top_srcdir)/src/*.xml
	$(PYTHON) $^ > $@

opendroid.pot: opendroid-py.pot opendroid-xml.pot
	sed --in-place opendroid-py.pot --expression=s/CHARSET/UTF-8/
	sed --in-place opendroid-xml.pot --expression=s/CHARSET/UTF-8/
	cat $^ | $(MSGUNIQ) --no-wrap --no-location -o $@ -

%.po: opendroid.pot
	if [ -f $@ ]; then \
		$(MSGMERGE) --backup=none --no-wrap --no-location -s -N -U $@ $< && touch $@; \
	else \
		$(MSGINIT) -l $@ -o $@ -i $< --no-translator; \
	fi

.po.mo:
	$(MSGFMT) -o $@ $<

BUILT_SOURCES = $(LANGMO)
CLEANFILES = $(LANGMO) opendroid-py.pot opendroid.pot

dist-hook: $(LANGPO)

install-data-local: $(LANGMO)
	for lang in $(LANGS); do \
		$(mkinstalldirs) $(DESTDIR)/usr/lib/enigma2/python/OPENDROID/locale/$$lang/LC_MESSAGES; \
		$(INSTALL_DATA) $$lang.po $(DESTDIR)/usr/lib/enigma2/python/OPENDROID/locale/opendroid-$$lang.po; \
		$(INSTALL_DATA) $$lang.mo $(DESTDIR)/usr/lib/enigma2/python/OPENDROID/locale/$$lang/LC_MESSAGES/opendroid.mo; \
	done

uninstall-local:
	for lang in $(LANGS); do \
		$(RM) $(DESTDIR)/usr/lib/enigma2/python/OPENDROID/locale/opendroid-$$lang.po; \
		$(RM) $(DESTDIR)/usr/lib/enigma2/python/OPENDROID/locale/$$lang/LC_MESSAGES/opendroid.mo; \
	done
