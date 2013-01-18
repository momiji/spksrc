PKG_NAME = python3
PKG_VERS = 3.3.0
PKG_EXT = tar.bz2
PKG_DIST_NAME = Python-$(PKG_VERS).$(PKG_EXT)
PKG_DIST_SITE = http://www.python.org/ftp/python/$(PKG_VERS)
PKG_DIR = Python-$(PKG_VERS)

DEPENDS  = cross/zlib cross/openssl cross/sqlite cross/readline cross/ncurses cross/bzip2
#DEPENDS += native/$(PKG_NAME)

HOMEPAGE = http://www.python.org/
COMMENT  = Python Programming Language
LICENSE  =

BUILD_ARCH = $(shell expr "$(TC_TARGET)" : '\([^-]*\)' )
HOST_ARCH = $(shell uname -m)
HOSTPYTHON_NATIVE = $(WORK_DIR)/$(PKG_DIR)/hostpython
HOSTPYTHON_LIB_NATIVE = $(WORK_DIR)/$(PKG_DIR)/build/lib.linux-$(HOST_ARCH)-3.3
PYTHON_LIB_CROSS = $(WORK_DIR)/$(PKG_DIR)/build/lib.linux-$(BUILD_ARCH)-3.3
HOSTPGEN_NATIVE = $(WORK_DIR)/$(PKG_DIR)/hostpgen
PYTHON_LIB_DIR = lib/python3.3

HOSTPYTHON = $(WORK_DIR)/$(PKG_DIR)/hostpython
HOSTPGEN = $(WORK_DIR)/$(PKG_DIR)/hostpgen

# Patch
POST_PATCH_TARGET = myPostPatch

# Configure
CONFIGURE_TARGET = myConfigure
GNU_CONFIGURE = 1
ADDITIONAL_CFLAGS = -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64
CONFIGURE_ARGS = --enable-ipv6 ac_cv_buggy_getaddrinfo=no ac_cv_file__dev_ptmx=no ac_cv_file__dev_ptc=no PYTHON_FOR_BUILD=./hostpython HOSTARCH=$(TC_TARGET) BUILDARCH=$(shell uname -m)

# Compile
COMPILE_TARGET = myCompile

# Install
INSTALL_TARGET = myInstall
POST_INSTALL_TARGET = $(WORK_DIR)/python-cc.mk

include ../../mk/spksrc.cross-cc.mk

myPostPatch:
	$(RUN) sed -e 's#@INSTALL_PREFIX@#$(INSTALL_PREFIX)#' -i Lib/mimetypes.py

myConfigure:
	cd $(WORK_DIR)/$(PKG_DIR) && CC= CPP= CXX= STRIP= RANLIB= READELF= AS= AR= CFLAGS= CPPFLAGS= CXXFLAGS= LDFLAGS= ./configure --prefix=$(WORK_DIR)/native
	cd $(WORK_DIR)/$(PKG_DIR) && CC= CPP= CXX= STRIP= RANLIB= READELF= AS= AR= CFLAGS= CPPFLAGS= CXXFLAGS= LDFLAGS= $(MAKE)
	cd $(WORK_DIR)/$(PKG_DIR) && tar zcf build.tgz build
	cd $(WORK_DIR)/$(PKG_DIR) && cp python hostpython
	cd $(WORK_DIR)/$(PKG_DIR) && cp config.log hostconfig.log
	cd $(WORK_DIR)/$(PKG_DIR) && make distclean
	cd $(WORK_DIR)/$(PKG_DIR) && tar zxf build.tgz
	cd $(WORK_DIR)/$(PKG_DIR) && rm build.tgz
	$(RUN) ./configure $(REAL_CONFIGURE_ARGS)

myCompile:
	$(RUN) _PYTHON_HOST_PLATFORM=$(TC_TARGET) $(MAKE)

myInstall:
	@install -m 755 -d $(STAGING_INSTALL_PREFIX)/etc
	@install -m 644 src/mime.types $(STAGING_INSTALL_PREFIX)/etc/
	$(RUN) _PYTHON_HOST_PLATFORM=$(TC_TARGET) $(MAKE) install prefix=$(STAGING_INSTALL_PREFIX)

$(WORK_DIR)/python-cc.mk: Makefile
	@echo HOSTPYTHON=$(HOSTPYTHON) > $@
	@echo HOSTPYTHON_LIB_NATIVE=$(HOSTPYTHON_LIB_NATIVE) >> $@
	@echo PYTHON_LIB_CROSS=$(PYTHON_LIB_CROSS) >> $@
	@echo PYTHON_LIB_DIR=$(PYTHON_LIB_DIR) >> $@

