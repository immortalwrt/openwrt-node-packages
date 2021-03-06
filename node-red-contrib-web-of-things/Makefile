# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=node-red-contrib-web-of-things
PKG_NAME:=$(PKG_NPM_NAME)
PKG_VERSION:=1.2.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NPM_NAME)-$(PKG_VERSION).tgz
PKG_SOURCE_URL:=https://registry.npmjs.org/$(PKG_NPM_NAME)/-/
PKG_HASH:=de7d82f3827229d0e03ffcf8c852b3ae4558d38a7730fb9c3d51aedf8ea97bbc

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/node-red-contrib-web-of-things
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=Web of Things node for node-RED
  URL:=https://flows.nodered.org/node/node-red-contrib-web-of-things
  DEPENDS:=+node +node-red +node-wot-core +node-wot-binding-http +node-wot-binding-coap \
           +node-wot-binding-mqtt +node-wot-binding-opcua +node-wot-binding-modbus
endef

define Package/node-red-contrib-web-of-things/description
 A Node-RED package of nodes for the Web of Things.
endef

TAR_OPTIONS+= --strip-components 1
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)

define Build/Compile
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_target_arch=$(NODEJS_CPU) \
	npm_config_build_from_source=true \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_prefix=$(PKG_INSTALL_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	npm install -g $(PKG_BUILD_DIR)
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
endef

define Package/node-red-contrib-web-of-things/install
	$(INSTALL_DIR) $(1)/usr/lib/node/$(PKG_NPM_NAME)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/{package.json,*.md} \
		$(1)/usr/lib/node/$(PKG_NPM_NAME)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NPM_NAME)/src \
		$(1)/usr/lib/node/$(PKG_NPM_NAME)/
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(LN) ../node/$(PKG_NPM_NAME) $(1)/usr/lib/node_modules/$(PKG_NPM_NAME)
endef

define Package/node-red-contrib-web-of-things/postrm
#!/bin/sh
rm /usr/lib/node_modules/node-red-contrib-web-of-things || true
rm -rf /usr/lib/node/node-red-contrib-web-of-things || true
endef

$(eval $(call BuildPackage,node-red-contrib-web-of-things))
