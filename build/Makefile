ifneq ($(findstring i686,$(TARGET)),)
CFLAGS=-m32
else
CFLAGS=-m64
endif

ifneq ($(findstring mingw,$(TARGET)),)
# Otherwise apparently the makefiles get corrupted and aren't valid
export LD=$(shell which ld)
endif

OPTS = \
	--enable-shared=no \
	--disable-examples-build \
	--prefix="$(OUT_DIR)"

export CFLAGS := $(CFLAGS) -fPIC
PWD := $(CURDIR)
LIBSSH2 := libssh2-1.4.4-20140901

all:
	rm -rf "$(OUT_DIR)/include" "$(OUT_DIR)/lib"
	mkdir -p "$(OUT_DIR)/build"
	[ -f "$(OUT_DIR)/build/Makefile" ] && \
		make -C "$(OUT_DIR)/build" distclean || true
	(cd "$(OUT_DIR)/build" && "$(PWD)/$(LIBSSH2)/configure" $(OPTS))
	make -C "$(OUT_DIR)/build"
	rm -rf "$(OUT_DIR)/include" "$(OUT_DIR)/lib"
	cp -r "$(PWD)/$(LIBSSH2)/include" "$(OUT_DIR)"
	mkdir -p "$(OUT_DIR)/lib/pkgconfig"
	cp "$(OUT_DIR)/build/src/.libs/libssh2.a" "$(OUT_DIR)/lib"
	cp "$(OUT_DIR)/build/src/.libs/libssh2.a" "$(OUT_DIR)"
	cp "$(OUT_DIR)/build/libssh2.pc" "$(OUT_DIR)/lib/pkgconfig"
	cp "$(OUT_DIR)/build/libssh2.pc" "$(OUT_DIR)"

