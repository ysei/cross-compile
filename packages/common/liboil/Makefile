NAME = liboil
VERSION = 0.3.14
DEPENDS = common/glib-2.0

SRC_TGZ = http://palm.cdnetworks.net/opensource/1.4.1.1/${NAME}-${VERSION}.tar.gz

include ../../../support/download.mk

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked: ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	rm -rf build
	mkdir -p build
	tar -C build -z -x -f $<
	mv build/${NAME}-${VERSION} build/src
	patch -d build/src -p0 < ${NAME}-webos-patch
	touch $@

include ../../../support/staging.mk

include ../../../support/stage-rootfs.mk

include ../../../support/cross-compile.mk

stage-local:: build/${ARCH}.built

build/%.built: build/.unpacked
	rm -f $@
	mkdir -p build/$*
	cp -R build/src/* build/$*
	( cd build/$* ; \
		LDFLAGS="-Wl,-rpath,/usr/local/lib -L/usr/local/lib" ${SB2} \
		./configure --disable-static --prefix=${INSTALL_PREFIX} )
	( cd build/$* ; ${SB2} $(MAKE) install-pkgconfigDATA )
	( cd build/$*/liboil ; ${SB2} $(MAKE) install-pkgincludeHEADERS )
	( cd build/$* ; ${SB2} $(MAKE) install )
	touch $@

clobber::
	rm -rf build
