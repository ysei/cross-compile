NAME = matchbox-window-manager
VERSION = 1.2

DEPENDS = x/matchbox-common \
		  x/libmatchbox \
			x/libXext


SRC_BZ2 = http://matchbox-project.org/sources/${NAME}/${VERSION}/${NAME}-${VERSION}.tar.bz2

include ../../../support/download.mk

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked: ${DL_DIR}/${NAME}-${VERSION}.tar.bz2
	rm -rf build
	mkdir -p build
	tar -C build -j -x -f $<
	mv build/${NAME}-${VERSION} build/src
	touch $@

include ../../../support/staging.mk

stage-local:: build

include ../../../support/cross-compile.mk

build: build/${ARCH}.built

build/%.built: build/.unpacked
	rm -f $@
	mkdir -p build/$*
	cp -R build/src/* build/$*
	( cd build/$* ; ${SB2} \
	LDFLAGS="\"-L/usr/local/lib -Wl,-rpath-link,/usr/local/lib -Wl,-rpath,/usr/local/lib\" PATH=/usr/local/bin:${PATH}" \
	./configure \
		--prefix=${INSTALL_PREFIX} \
	)	
	( cd build/$* ; ${SB2} $(MAKE))
	( cd build/$* ; ${SB2} $(MAKE) install)
	touch $@

clobber::
	rm -rf build
