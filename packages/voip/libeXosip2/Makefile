NAME = libeXosip2
VERSION = 3.6.0
DEPENDS = common/openssl voip/libosip2

SRC_TGZ = http://mirror.lihnidos.org/GNU/savannah/exosip/libeXosip2-3.6.0.tar.gz

include ../../../support/download.mk

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked: ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	rm -rf build
	mkdir -p build
	tar -C build -z -x -f $<
	mv build/libeXosip2-3.6.0 build/src
	touch $@

include ../../../support/staging.mk

#include ../../../support/stage-rootfs.mk

include ../../../support/cross-compile.mk

stage-local:: build/${ARCH}.built

build/%.built: build/.unpacked
	rm -f $@
	mkdir -p build/$*
# cp is notorious for compromising last-modification time...
# so rely on tar instead until autoconf/automake weirdness is fixed!
#	cp -R build/src/* build/$*
	(cd build/src; tar cf - .) | (cd build/$*; tar xf -)
	( cd build/$* ; ${SB2} export CFLAGS=-I${INSTALL_PREFIX}/include && \
		export LDFLAGS="-L${INSTALL_PREFIX}/lib -Wl,-rpath,${INSTALL_PREFIX}/lib" && \
		./configure --prefix=${INSTALL_PREFIX} --disable-static )
	( cd build/$*/src ; ${SB2} $(MAKE) install )
	( cd build/$*/include ; ${SB2} $(MAKE) install )
	touch $@

clobber::
	rm -rf build
