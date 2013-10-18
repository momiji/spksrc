PKG_NAME = findutils
PKG_VERS = 4.1.20
PKG_EXT = tar.gz
PKG_DIR = $(PKG_NAME)-$(PKG_VERS)
PKG_DIST_NAME = $(PKG_DIR).$(PKG_EXT)
PKG_DIST_SITE = http://ftp.gnu.org/pub/gnu/findutils

DEPENDS =

HOMEPAGE = http://www.gnu.org/software/findutils/
COMMENT  = Basic directory searching utilities of the GNU operating system
LICENSE  = GPL

GNU_CONFIGURE = 1
ADDITIONAL_CFLAGS = -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64
CONFIGURE_ARGS  = --enable-shared --enable-ipv6 PYTHON_FOR_BUILD=$(HOSTPYTHON) PGEN_FOR_BUILD=$(HOSTPGEN)
CONFIGURE_ARGS += ac_cv_buggy_getaddrinfo=no ac_cv_file__dev_ptmx=no ac_cv_file__dev_ptc=no ac_cv_have_long_long_format=yes
CONFIGURE_ARGS += ac_cv_x87_double_rounding=yes
ifneq ($(findstring $(ARCH), powerpc ppc824x),)
CONFIGURE_ARGS += ac_cv_func_clock_gettime=no ac_cv_lib_rt_clock_gettime=no
endif

CONFIGURE_TARGET = myConfigure
COMPILE_TARGET = myCompile
INSTALL_TARGET = myInstall

HOST_ARCH = $(shell uname -m)
BUILD_ARCH = $(shell expr "$(TC_TARGET)" : '\([^-]*\)' )

include ../../mk/spksrc.cross-cc.mk

.PHONY: myConfigure
myConfigure:
	$(RUN) ./configure --build=$(HOST_ARCH) --host=$(BUILD_ARCH) --prefix=/usr --cache-file=config.cache --libexecdir=/usr/lib/locate --localstatedir=/var/lib/locate

.PHONY: myCompile
myCompile:
	$(RUN) $(MAKE)

.PHONY: myInstall
myInstall:
	install -m 755 -d $(STAGING_INSTALL_PREFIX)/bin
	install -m 755 $(WORK_DIR)/$(PKG_DIR)/find/find $(STAGING_INSTALL_PREFIX)/bin
