NAME = fontconfig
VERSION = 2.3.2
DEPENDS = common/expat common/freetype

SRC_TGZ = http://fontconfig.org/release/fontconfig-2.3.2.tar.gz

include ../../../support/download.mk

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked: ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	rm -rf build
	mkdir -p build
	tar -C build -z -x -f $<
	mv build/${NAME}-${VERSION} build/src
	chmod a+rw -R .
	patch -d build/src -p1 < ./${NAME}-${VERSION}.patch
	touch $@

include ../../../support/staging.mk

include ../../../support/stage-rootfs.mk

include ../../../support/cross-compile.mk

stage-local:: build/${ARCH}.built

build/%.built: build/.unpacked
	rm -rf $@ build/$*
	mkdir -p build/$*
	cp -R build/src/* build/$*
	( cd build/$* ; ${SB2} /usr/bin/autoconf; \
		${SB2} ./configure --disable-static --prefix=${INSTALL_PREFIX} )
	( cd build/$*/fontconfig ; ${SB2} $(MAKE) install )
	( cd build/$* ; ${SB2} $(MAKE) install-pkgconfigDATA )
	touch $@

clobber::
	rm -rf build
