# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NPM_NAME:=xmpp
PKG_NAME:=node-$(PKG_NPM_NAME)
PKG_VERSION:=1.1.0
PKG_RELEASE:=7

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/xmppjs/xmpp.js.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=b439f45824cfff079c823f6f2c8c67f14237731e
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_MIRROR_HASH:=deccde6e02580d90907c4e4dd20a233290b22fa7c28124bfd60385669b98bc5a

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=

PKG_BUILD_DEPENDS:=node/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

include $(INCLUDE_DIR)/package.mk

define Package/node-xmpp/default
  SUBMENU:=Node.js
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=XMPP $(1) for JavaScript
  URL:=https://github.com/xmppjs/xmpp.js/tree/node-xmpp
  DEPENDS:=+node
endef

define Package/node-xmpp
  $(call Package/node-xmpp/default,modules)
endef

define Package/node-xmpp-client
  $(call Package/node-xmpp/default,client)
endef

define Package/node-xmpp-component
  $(call Package/node-xmpp/default,component)
endef

define Package/node-xmpp-server
  $(call Package/node-xmpp/default,server)
endef

define Package/node-xmpp/description
 node-xmpp is a set of JavaScript Node.js, io.js and browser modules to work with XMPP and/or XML
endef

NODEJS_CPU:=$(subst powerpc,ppc,$(subst aarch64,arm64,$(subst x86_64,x64,$(subst i386,ia32,$(ARCH)))))
TMPNPM:=$(shell mktemp -u XXXXXXXXXX)

TARGET_CFLAGS+=$(FPIC)
TARGET_CPPFLAGS+=$(FPIC)

define Build/Compile/module
	$(MAKE_VARS) \
	$(MAKE_FLAGS) \
	npm_config_arch=$(NODEJS_CPU) \
	npm_config_target_arch=$(NODEJS_CPU) \
	npm_config_build_from_source=true \
	npm_config_nodedir=$(STAGING_DIR)/usr/ \
	npm_config_prefix=$(PKG_INSTALL_DIR)/usr/ \
	npm_config_cache=$(TMP_DIR)/npm-cache-$(TMPNPM) \
	npm_config_tmp=$(TMP_DIR)/npm-tmp-$(TMPNPM) \
	NODE_PATH="$(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NAME)/node_modules:$(PKG_INSTALL_DIR)/usr/lib/node_modules/$(PKG_NAME)/node_modules/$(1)/node_modules" \
	npm install -g $(PKG_BUILD_DIR)/packages/$(1)/
	rm -rf $(TMP_DIR)/npm-tmp-$(TMPNPM)
	rm -rf $(TMP_DIR)/npm-cache-$(TMPNPM)
endef

define Build/Compile
	$(call Build/Compile/module,node-xmpp)
	$(call Build/Compile/module,node-xmpp-client)
	$(call Build/Compile/module,node-xmpp-component)
	$(call Build/Compile/module,node-xmpp-server)
endef

define Package/node-xmpp/install
	$(INSTALL_DIR) $(1)/usr/lib/node/node-xmpp
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/node-xmpp/* $(1)/usr/lib/node/node-xmpp/
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(LN) ../node/node-xmpp $(1)/usr/lib/node_modules/node-xmpp
endef

define Package/node-xmpp/postrm
#!/bin/sh
rm /usr/lib/node_modules/node-xmpp || true
rm -rf /usr/lib/node/node-xmpp || true
endef

define Package/node-xmpp-client/install
	$(INSTALL_DIR) $(1)/usr/lib/node/node-xmpp-client
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/node-xmpp-client/* $(1)/usr/lib/node/node-xmpp-client/
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(LN) ../node/node-xmpp-client $(1)/usr/lib/node_modules/node-xmpp-client
endef

define Package/node-xmpp-client/postrm
#!/bin/sh
rm /usr/lib/node_modules/node-xmpp-client || true
rm -rf /usr/lib/node/node-xmpp-client || true
endef

define Package/node-xmpp-component/install
	$(INSTALL_DIR) $(1)/usr/lib/node/node-xmpp-component
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/node-xmpp-component/* $(1)/usr/lib/node/node-xmpp-component/
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(LN) ../node/node-xmpp-component $(1)/usr/lib/node_modules/node-xmpp-component
endef

define Package/node-xmpp-component/postrm
#!/bin/sh
rm /usr/lib/node_modules/node-xmpp-component || true
rm -rf /usr/lib/node/node-xmpp-component || true
endef

define Package/node-xmpp-server/install
	$(INSTALL_DIR) $(1)/usr/lib/node/node-xmpp-server
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/node_modules/node-xmpp-server/* $(1)/usr/lib/node/node-xmpp-server/
	$(INSTALL_DIR) $(1)/usr/lib/node_modules
	$(LN) ../node/node-xmpp-server $(1)/usr/lib/node_modules/node-xmpp-server
endef

define Package/node-xmpp-server/postrm
#!/bin/sh
rm /usr/lib/node_modules/node-xmpp-server || true
rm -rf /usr/lib/node/node-xmpp-server || true
endef

$(eval $(call BuildPackage,node-xmpp))
$(eval $(call BuildPackage,node-xmpp-client))
$(eval $(call BuildPackage,node-xmpp-component))
$(eval $(call BuildPackage,node-xmpp-server))
