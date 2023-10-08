#
# diStorm3 (NonStop Port)
#

# Set this for the build RVU to use
RVU ?= L17.08
# Set this on the command line to load custom environment parameters.
HOSTNAME ?= $(shell uname -n)

# Include custom system overrides for cross-compilation, if any
-include environment.$(HOSTNAME)

TARGET_BASE	= libdistorm3.so
COBJS	= ../../src/mnemonics.o ../../src/textdefs.o ../../src/prefix.o ../../src/operands.o ../../src/insts.o ../../src/instructions.o ../../src/distorm.o ../../src/decoder.o
CC	= c99
CFLAGS	+= -O2 -WRVU=$(RVU) -Wnowarn=1506,226 -DSUPPORT_64BIT_OFFSET -DDISTORM_STATIC
LDFLAGS	+= -WRVU=$(RVU) -Wshared
PREFIX	= /usr/local
# The lib SONAME version (ignored on NonStop):
LIB_S_VERSION = 3
# The lib real version (ignored on NonStop):
LIB_R_VERSION = 3.4.0
#LDFLAGS += -Wl,-soname,${TARGET_BASE}
LDFLAGS += 
DESTDIR	=
TARGET_NAME = ${TARGET_BASE}

all:
	PATH=$(BUILD_PATH) $(MAKE) -C . HOSTNAME=$(HOSTNAME) RVU=$(RVU) clib

clean:
	/bin/rm -rf ../../src/*.o ${TARGET_NAME} ../../distorm3.a ./../*.o

clib: ${COBJS}
	${CC} ${CFLAGS} ${VERSION} ${COBJS} ${LDFLAGS} -o ${TARGET_NAME}
	ar rs ../../distorm3.a ${COBJS}

install: ${TARGET_NAME}
	install -D -s ${TARGET_NAME} ${DESTDIR}${PREFIX}/lib/${TARGET_NAME}
	ln -sf ${DESTDIR}${PREFIX}/lib/${TARGET_NAME} ${DESTDIR}${PREFIX}/lib/${TARGET_BASE}
	@echo "... running ldconfig might be smart ..."

.c.o:
	${CC} ${CFLAGS} ${VERSION} -c $< -o $@

