#
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=node
PKG_VERSION:=v0.10.28
PKG_RELEASE:=2

PKG_SOURCE:=node-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://nodejs.org/dist/${PKG_VERSION}
PKG_MD5SUM:=87768be7065d2120e71619948ab4bb2d
#GIT_SOURCE:=https://github.com/brimstone/v8m-rb
#GIT_SOURCE:=/tmp/v8m-rb

GYP_DEFINES:=v8_use_mips_abi_hardfloat=false v8_can_use_fpu_instructions=false
LIBS:=-I$(TOOLCHAIN_DIR)/i486-openwrt-linux-uclibc/include/c++/4.6.3/ -I$(TOOLCHAIN_DIR)/i486-openwrt-linux-uclibc/include/c++/4.6.3/i486-openwrt-linux-uclibc/

include $(INCLUDE_DIR)/package.mk

define Package/node
  DEPENDS:=+libpthread +librt +libstdcpp
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Node.js is a platform built on Chrome's JavaScript runtime
  URL:=http://nodejs.org/
endef

define Package/node/description
Node.js is a platform built on Chrome's JavaScript runtime for easily building fast, scalable network applications. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient, perfect for data-intensive real-time applications that run across distributed devices.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) node.patch $(PKG_BUILD_DIR)/
	(cd $(PKG_BUILD_DIR); \
		echo $(TOOLCHAIN_DIR);\
	patch -p1 < node.patch; \
	);
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR); \
	export LIBS="$(LIBS)"; \
	export CFLAGS="$(TARGET_CFLAGS) $(LIBS) -g"; \
	export CXXFLAGS="$(TARGET_CXXFLAGS) $(LIBS) -g"; \
    export GYPFLAGS="$(GYPFLAGS)"; \
	./configure --without-snapshot; \
	);
endef

define Build/Compile
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) GYP_DEFINES="$(GYP_DEFINES)" CC="$(TARGET_CC)" CXX="$(TARGET_CXX)" CFLAGS="$(TARGET_CFLAGS) -g $(LIBS)" CXXFLAGS="$(TARGET_CXXFLAGS) $(LIBS) -nostdinc++" LDFLAGS="$(TARGET_LDFLAGS) -nodefaultlibs -lc -lgcc -lgcc_s -lpthread -lstdc++" || touch $(PKG_BUILD_DIR)/deps/v8/build/common.gypi
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) GYP_DEFINES="$(GYP_DEFINES)" CC="$(TARGET_CC)" CXX="$(TARGET_CXX)" CFLAGS="$(TARGET_CFLAGS) -g $(LIBS)" CXXFLAGS="$(TARGET_CXXFLAGS) $(LIBS) -nostdinc++" LDFLAGS="$(TARGET_LDFLAGS) -nodefaultlibs  -lc -lgcc -lgcc_s -lpthread -lstdc++"
endef


define Package/node/install
	mkdir -p $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/out/Release/node $(1)/usr/bin/
	ln -s /usr/bin/node $(1)/usr/bin/nodejs
endef

$(eval $(call BuildPackage,node))
