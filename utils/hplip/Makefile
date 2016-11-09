#
# Copyright (C) 2006-2010 OpenWrt.org
# Copyright (C) 2016 Aaron Bulmahn
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hplip
PKG_VERSION:=3.16.7
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/$(PKG_NAME)
PKG_MD5SUM:=d4982fd71549f45856b5ad76e1ae8809

PKG_BUILD_DEPENDS := python
PKG_FIXUP:=libtool

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/hplip
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS+=+libjpeg +libsane +cups +libcupsimage +libusb-1.0
endef

define Package/hplip/description
	HPLIP is an HP developed solution for printing, scanning, and faxing with HP inkjet and laser based printers in Linux.
endef

CONFIGURE_ARGS += \
	--disable-gui-build \
	--disable-network-build \
	--disable-fax-build \
	--disable-pp-build \
	--disable-doc-build \
	--disable-foomatic-xml-install \
	--disable-dbus-build

define Build/Configure
	$(call Build/Configure/Default,\
		$(CONFIGURE_ARGS),\
		ac_cv_lib_cups_cupsDoFileRequest=yes \
		LIBS="-ljpeg -lusb-1.0" \
	)
endef

define Package/hplip/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpip.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpipp.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/.libs/libhpmud.so* $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/cups/backend
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/.libs/hp $(1)/usr/lib/cups/backend

	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/hpcups $(1)/usr/lib/cups/filter
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/prnt/filters/hpps $(1)/usr/lib/cups/filter
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/fax/filters/pstotiff $(1)/usr/lib/cups/filter

	$(INSTALL_DIR) $(1)/usr/lib/sane
	$(CP) $(PKG_BUILD_DIR)/.libs/libsane-hpaio.so* $(1)/usr/lib/sane

	$(INSTALL_DIR) $(1)/etc/hp
	$(CP) $(PKG_BUILD_DIR)/hplip.conf $(1)/etc/hp/hplip.conf

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/models
	$(CP) $(PKG_BUILD_DIR)/data/models/models.dat $(1)/usr/share/hplip/data/models

	$(INSTALL_DIR) $(1)/usr/share/cups/drv/hp
	$(CP) $(PKG_BUILD_DIR)/prnt/drv/hpcups.drv $(1)/usr/share/cups/drv/hp

	$(INSTALL_DIR) $(1)/usr/share/ppd/HP
	$(CP) $(PKG_BUILD_DIR)/prnt/ps/* $(1)/usr/share/ppd/HP
endef

$(eval $(call BuildPackage,hplip))
