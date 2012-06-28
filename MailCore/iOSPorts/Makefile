#
#  iOS Ports Library
#  Copyright (C) 2011 Bindle Binaries
#  All rights reserved.
#
#  @BINDLE_BINARIES_BSD_LICENSE_START@
#
#  Redistribution and use in source and binary forms, with or without
#  modification, are permitted provided that the following conditions are
#  met:
#
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of Bindle Binaries nor the
#       names of its contributors may be used to endorse or promote products
#       derived from this software without specific prior written permission.
#
#  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS
#  IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO,
#  THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR
#  PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL BINDLE BINARIES BE LIABLE FOR
#  ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
#  DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
#  SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER
#  CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
#  LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
#  OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
#  SUCH DAMAGE.
#
#  @BINDLE_BINARIES_BSD_LICENSE_END@
#

SOURCES		= \
		  ports/database/openldap/pkgdata_openldap.m \
		  ports/devel/pcre/pkgdata_pcre.m \
		  ports/security/cyrus-sasl/pkgdata_cyrus-sasl.m \
		  ports/security/openssl/pkgdata_openssl.m \
		  ports/iOSPorts/pkgdata_iosports.m

LIBSOURCES	= \
		  ports/iOSPorts/other/iOSPorts-data.m \
		  ports/iOSPorts/other/iOSPorts-list.m

PKGINFOSOURCES	= \
		  $(LIBSOURCES) \
		  ports/iOSPorts/other/iOSPorts-pkginfo.m \
		  ports/iOSPorts/classes/iOSPortsCFuncs.m

PROGS		= \
		  build-aux/iOSPorts-geninfo \
		  build-aux/iOSPorts-genlist \
		  build-aux/iOSPorts-pkginfo

INCLUDES	= \
		  include/iOSPorts/iOSPorts.h \
		  include/iOSPorts/iOSPortsCFuncs.h \
		  include/iOSPorts/iOSPortsPackage.h \
		  include/iOSPorts/iOSPortsTypes.h \
		  include/iOSPorts/iOSPortsVersion.h \
		  include/iOSPorts/iOSPortsViewController.h

CFLAGS		= -W -Wall -Werror -Iinclude -framework Foundation

all: $(PROGS)

prog: $(PROGS)

include/iOSPorts:
	@mkdir -p ${@}

include/iOSPorts/iOSPorts.h: include/iOSPorts ports/iOSPorts/classes/iOSPorts.h
	cp ports/iOSPorts/classes/iOSPorts.h ${@};

include/iOSPorts/iOSPortsCFuncs.h: include/iOSPorts ports/iOSPorts/classes/iOSPortsCFuncs.h
	cp ports/iOSPorts/classes/iOSPortsCFuncs.h ${@};

include/iOSPorts/iOSPortsPackage.h: include/iOSPorts ports/iOSPorts/classes/iOSPortsPackage.h
	cp ports/iOSPorts/classes/iOSPortsPackage.h ${@};

include/iOSPorts/iOSPortsTypes.h: include/iOSPorts ports/iOSPorts/classes/iOSPortsTypes.h
	cp ports/iOSPorts/classes/iOSPortsTypes.h ${@};

include/iOSPorts/iOSPortsVersion.h: include/iOSPorts ports/iOSPorts/classes/iOSPortsVersion.h
	cp ports/iOSPorts/classes/iOSPortsVersion.h ${@};

include/iOSPorts/iOSPortsViewController.h: include/iOSPorts ports/iOSPorts/classes/iOSPortsViewController.h
	cp ports/iOSPorts/classes/iOSPortsViewController.h ${@};

$(SOURCES): build-aux/iOSPorts-geninfo build-aux/Makefile-package $(INCLUDES)
	$(MAKE) -C "`dirname ${@}`" license

ports/iOSPorts/other/iOSPorts-data.m: $(SOURCES)
	@rm -f ports/iOSPorts/other/iOSPorts-data.m
	cat $(SOURCES) > ports/iOSPorts/other/iOSPorts-data.m || \
	   { rm -f ports/iOSPorts/other/iOSPorts-data.m; exit 1; }

ports/iOSPorts/other/iOSPorts-list.m: build-aux/iOSPorts-genlist $(SOURCES)
	build-aux/iOSPorts-genlist -f -o ports/iOSPorts/other/iOSPorts-list.m $(SOURCES)

build-aux/iOSPorts-geninfo: ports/iOSPorts/other/iOSPorts-geninfo.m $(INCLUDES)
	$(CC) $(CFLAGS) -o ${@} ports/iOSPorts/other/iOSPorts-geninfo.m

build-aux/iOSPorts-genlist: ports/iOSPorts/other/iOSPorts-genlist.m $(INCLUDES)
	$(CC) $(CFLAGS) -o ${@} ports/iOSPorts/other/iOSPorts-genlist.m

build-aux/iOSPorts-pkginfo: $(PKGINFOSOURCES) $(INCLUDES)
	$(CC) $(CFLAGS) -o ${@} $(PKGINFOSOURCES)

clean:
	rm -Rf $(PROGS) $(SOURCES) $(LIBSOURCES)
	rm -Rf include/iOSPorts
	rm -Rf ports/iOSPorts/other/iOSPorts-data.c
	rm -Rf a.out *.o src/*.o ports/iOSPorts/other/*.o
	rm -Rf build/

distcleanall: clean
	for PKG in $(SOURCES);do \
           PKGDIR="`dirname $${PKG}`"; \
	   echo "cleaning $${PKGDIR}..."; \
	   $(MAKE) -C $${PKGDIR} distclean; \
	   rm -Rf $${PKGDIR}/build; \
	done

distclean: clean
	for PKG in $(SOURCES);do \
           PKGDIR="`dirname $${PKG}`"; \
	   echo "cleaning $${PKGDIR}..."; \
	   $(MAKE) -C $${PKGDIR} clean; \
	   rm -Rf $${PKGDIR}/build; \
	done
	@echo ' '
	@echo 'To distclean all ports, run "make distcleanall"'
	@echo ' '

