ifneq ($(findstring i686,$(TARGET)),)
CFLAGS=-m32
else
CFLAGS=-m64
endif

OPTS = \
	--enable-shared=no \
	--disable-examples-build \
	--prefix=$(OUT_DIR) \

export CFLAGS := $(CFLAGS) -fPIC
PWD := $(CURDIR)

all:
	mkdir -p "$(OUT_DIR)"
	(cd "$(OUT_DIR)" && $(PWD)/libssh2-1.4.3/configure $(OPTS))
	make -C "$(OUT_DIR)" -j10
	make -C "$(OUT_DIR)" -j10 install
	cp "$(OUT_DIR)/lib/libssh2.a" "$(OUT_DIR)"
	cp "$(OUT_DIR)/lib/pkgconfig/libssh2.pc" "$(OUT_DIR)"

