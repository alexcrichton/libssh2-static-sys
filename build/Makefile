ifneq ($(findstring i686,$(TARGET)),)
export CFLAGS := $(CFLAGS) -m32 -ffunction-sections -fdata-sections
else ifneq ($(findstring x86_64,$(TARGET)),)
export CFLAGS := $(CFLAGS) -fPIC -m64 -ffunction-sections -fdata-sections
else
export CFLAGS := $(CFLAGS) -fPIC -ffunction-sections -fdata-sections
endif

ifneq ($(findstring windows,$(TARGET)),)
# Otherwise apparently the makefiles get corrupted and aren't valid
export LD=$(shell which ld)
OPTS := --without-openssl --with-wincng
endif

OPTS := $(OPTS) \
	--enable-shared=no \
	--disable-examples-build \
	--prefix="$(OUT_DIR)"

PWD := $(CURDIR)
LIBSSH2 := libssh2-1.4.4-20140901

# If libcurl exists locally, don't do anything. Otherwise go and build curl
all:
	pkg-config --exists libssh2 || $(MAKE) build

build:
	rm -rf "$(OUT_DIR)/include" "$(OUT_DIR)/lib" "$(OUT_DIR)/build"
	mkdir -p "$(OUT_DIR)/build" "$(OUT_DIR)/lib/pkgconfig"
	[ -f "$(OUT_DIR)/build/Makefile" ] && \
		make -C "$(OUT_DIR)/build" distclean || true
	(cd "$(OUT_DIR)/build" && "$(PWD)/$(LIBSSH2)/configure" $(OPTS))
	make -C "$(OUT_DIR)/build/src" -j$(NUM_JOBS)
	cp -r "$(PWD)/$(LIBSSH2)/include" "$(OUT_DIR)"
	cp "$(OUT_DIR)/build/src/.libs/libssh2.lib" "$(OUT_DIR)/lib/libssh2.a" || exit 0
	cp "$(OUT_DIR)/build/src/.libs/libssh2.a" "$(OUT_DIR)/lib/libssh2.a" || exit 0
	cp "$(OUT_DIR)/build/src/.libs/libssh2.lib" "$(OUT_DIR)/libssh2.a" || exit 0
	cp "$(OUT_DIR)/build/src/.libs/libssh2.a" "$(OUT_DIR)/libssh2.a" || exit 0
	cp "$(OUT_DIR)/build/libssh2.pc" "$(OUT_DIR)/lib/pkgconfig"
	cp "$(OUT_DIR)/build/libssh2.pc" "$(OUT_DIR)"

