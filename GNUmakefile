FOREIGN_MAKE = 1
TOP	= ../..
include $(TOP)/mk/common.make
include $(TOP)/mk/foreign.make

ifdef WIN32
EXE	= al2co.exe
RSYNC	= cp -uv
endif
ifdef UNIX
EXE	= al2co
endif

all: $(AL2CO_INSTALL)

install-for-chimera-distribution: $(AL2CO_INSTALL)

install-for-chimera-build: install-for-chimera-distribution
	$(RSYNC) $(AL2CO_INSTALL)/$(EXE) $(bindir)

$(AL2CO_INSTALL): $(AL2CO_SOURCE)
ifneq ($(WIN32),msvs)
	cd $(AL2CO_SOURCE) ; $(CC) $(TARGET_ARCH) $(CFLAGS) $(LDFLAGS) al2co.c -o $(EXE) -lm
else
	cd $(AL2CO_SOURCE) ; $(CC) $(TARGET_ARCH) $(CFLAGS) $(LDFLAGS) al2co.c /Fe$(EXE) msvcrt.lib
endif
	-mkdir $(AL2CO_INSTALL)
ifeq ($(WIN32),msvs)
	mt -nologo -manifest `cygpath -m "$(AL2CO_SOURCE)/$(EXE)"`.manifest \
		-outputresource:`cygpath -m "$(AL2CO_SOURCE)/$(EXE)"`\;1
endif
	cd $(AL2CO_SOURCE) ; $(RSYNC) $(EXE) $(AL2CO_INSTALL)

$(AL2CO_SOURCE):
	-mkdir $(AL2CO_SOURCE)
	$(RSYNC) al2co.c $(AL2CO_SOURCE)
ifneq ($(PATCHES),)
	for p in $(PATCHES); do \
		(cd $(AL2CO_SOURCE) ; patch -p0) < $$p ; \
	done
endif
