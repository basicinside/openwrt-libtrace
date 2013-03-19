# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id$

include $(TOPDIR)/rules.mk

PKG_NAME:=libtrace
PKG_VERSION:=3.0.16
PKG_RELEASE:=4

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://research.wand.net.nz/software/libtrace/
PKG_MD5SUM:=f873746341c94b85e4187a84969f2a17
PKG_BUILD_DEPENDS=zlib libpcap uclibcxx

include $(INCLUDE_DIR)/package.mk

define Package/libtrace
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS+= +zlib +uclibcxx +libpcap
  TITLE:=Trace file/capture device trace agnostic manipulation library
  URL:=http://research.wand.net.nz/software/libtrace.php
endef

define Package/libtrace/description
 This package contains a system-independent library for user-level network
 packet capture.
endef

TARGET_CFLAGS += $(FPIC)

define Build/Configure
	$(call Build/Configure/Default, \
		--enable-shared \
		--enable-static \
		, \
		BUILD_CC="$(TARGET_CC)" \
		HOSTCC="$(HOSTCC)" \
	)
endef


define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CCOPT="$(TARGET_CFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS) -lpcap -luClibc++ -lz" \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Build/InstallDev
	mkdir -p $(1)/usr/include
	$(CP)	$(PKG_INSTALL_DIR)/usr/include/dag{format,legacy}.h \
		$(1)/usr/include/
	$(CP)	$(PKG_INSTALL_DIR)/usr/include/lib{trace*,packetdump}.h \
		$(1)/usr/include/
	$(CP)	$(PKG_INSTALL_DIR)/usr/include/{erftypes,lt_inttypes,rt_protocol}.h \
		$(1)/usr/include/
	mkdir -p $(1)/usr/lib
	$(CP)	$(PKG_INSTALL_DIR)/usr/lib/libtrace.{a,la,so*} \
		$(1)/usr/lib/
	$(CP)	$(PKG_INSTALL_DIR)/usr/lib/libpacketdump.{a,la,so*} \
		$(1)/usr/lib/
	mkdir -p $(1)/usr/lib/libpacketdump
	$(CP) -r $(PKG_INSTALL_DIR)/usr/lib/libpacketdump/ \
		$(1)/usr/lib/libpacketdump

endef

define Package/libtrace/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libtrace.so.* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpacketdump.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/libpacketdump
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libpacketdump/*.so.* $(1)/usr/lib/libpacketdump
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/trace* $(1)/usr/bin/
endef

$(eval $(call BuildPackage,libtrace))
