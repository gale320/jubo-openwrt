
include $(TOPDIR)/rules.mk

PKG_NAME:=jubo-meteor
PKG_RELEASE:=r0.8.2
PKG_SOURCE:=$(PKG_NAME)-$(PKG_RELEASE).tar.gz
PKG_SOURCE_URL:
PKG_MD5SUM:

include $(INCLUDE_DIR)/package.mk

define Package/jubo
  DEPENDS:=+libstdcpp
  SECTION:=base
  CATEGORY:=Base system
  TITLE:=Base file for JuBo.
  URL:=http://www.meteor.com/
endef

define Package/jubo/description

endef

define Build/Prepare
	$(call Build/Prepare/Default)
	ls $(PKG_BUILD_DIR)
endef

define Build/Compile/Default

endef
Build/Compile = $(Build/Compile/Default)

define Package/jubo/install
	mkdir -p $(1)/usr/bin
	mkdir -p $(1)/jubo
	mkdir -p $(1)/jubo/db
	mkdir -p $(1)/jubo/apps
	$(CP) $(PKG_BUILD_DIR)/node $(1)/usr/bin
	$(CP) $(PKG_BUILD_DIR)/packages $(1)/jubo
	$(CP) $(PKG_BUILD_DIR)/node_modules $(1)/jubo
	ln -s $(1)/jubo/node_modules/jubo/bin/jubo $(1)/usr/bin/jubo
endef

$(eval $(call BuildPackage,jubo))
