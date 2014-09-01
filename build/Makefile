ifneq ($(findstring i686,$(TARGET)),)
CFLAGS=-m32
else
CFLAGS=-m64
endif

ifneq ($(findstring mingw,$(TARGET)),)
STATIC_SUFFIX=lib
else
STATIC_SUFFIX=a
endif

OPTS = \
	--enable-shared=no \
	--disable-examples-build \
	--prefix="$(OUT_DIR)" \

export CFLAGS := $(CFLAGS) -fPIC
PWD := $(CURDIR)
LIBSSH2 := libssh2-1.4.4-20140901

all:
	rm -rf "$(OUT_DIR)/include" "$(OUT_DIR)/lib"
	mkdir -p "$(OUT_DIR)"
	(cd "$(OUT_DIR)" && $(PWD)/$(LIBSSH2)/configure $(OPTS))
	make -C "$(OUT_DIR)"
	make -C "$(OUT_DIR)" install
	cp "$(OUT_DIR)/lib/libssh2.$(STATIC_SUFFIX)" "$(OUT_DIR)"
	cp "$(OUT_DIR)/lib/pkgconfig/libssh2.pc" "$(OUT_DIR)"

