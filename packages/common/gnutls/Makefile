NAME = gnutls
VERSION = 2.8.5
DEPENDS = common/libgpg-error common/libgcrypt

SRC_TGZ = http://ftp.gnu.org/pub/gnu/gnutls/gnutls-2.8.5.tar.bz2

include ../../../support/download.mk

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked: ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	rm -rf build
	mkdir -p build
	tar -C build -j -x -f $<
	mv build/${NAME}-${VERSION} build/src
	patch -d build/src -p1 < ./${NAME}-${VERSION}.patch
	touch $@

include ../../../support/staging.mk

include ../../../support/stage-rootfs.mk

include ../../../support/cross-compile.mk

stage-local:: build/${ARCH}.built

build/%.built: build/.unpacked
	rm -f $@
	mkdir -p build/$*
	cp -R build/src/* build/$*
	( cd build/$* ; ${SB2} ./configure --disable-static --prefix=${INSTALL_PREFIX} )
	( cd build/$*/lib/includes ; ${SB2} $(MAKE) install )
	( cd build/$*/lib ; ${SB2} $(MAKE) install-pkgconfigDATA )
	( ${SB2} ln -f -s libgnutls.so.13.2.2 ${INSTALL_PREFIX}/lib/libgnutls.so )
	touch $@

clobber::
	rm -rf build
