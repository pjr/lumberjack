# This build actually fails under Fedora 16. I looked at redhat and debian,
# both apply like 50 patches to openssl prior to packaging.
VERSION=1.0.1c
URL=ftp://ftp.openssl.org/source/openssl-$(VERSION).tar.gz
TARBALL=$(shell basename $(URL))
WORKDIR=openssl-$(VERSION)

default: install
include ../Makefile.ext

default: configure

#configure: #configure-autoconf
configure:
	(cd $(WORKDIR); ./config no-zlib no-krb5 enable-tlsext enable-camellia enable-mdc2 shared threads --prefix=$(PREFIX)--openssldir=$(PREFIX)/etc/ssl)


ifeq ($(UNAME),Darwin)
# Use --disable-dependency-tracking otherwise llvm/clang crashes
# on OS X 10.7.x
$(WORKDIR)/Makefile: | $(WORKDIR)
	cd $(WORKDIR); ./configure --prefix=$(PREFIX) --disable-dependency-tracking
endif

build: $(WORKDIR)/src/.libs/libopenssl.$(LIBEXT)
$(WORKDIR)/src/.libs/libopenssl.$(LIBEXT): configure
	$(MAKE) -C $(WORKDIR)

install: build
	$(MAKE) -C $(WORKDIR) install
