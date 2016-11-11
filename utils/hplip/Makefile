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
PKG_RELEASE:=2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/$(PKG_NAME)
PKG_MD5SUM:=d4982fd71549f45856b5ad76e1ae8809

PKG_BUILD_DEPENDS:=python
PKG_FIXUP:=libtool

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/hplip
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS+=+libjpeg +libusb-1.0 +cups +libcupsimage
endef

define Package/hplip/description
	HPLIP is an HP developed solution for printing, scanning, and faxing with HP inkjet and laser based printers in Linux.
endef

define Package/hplip/configfiles
/etc/hp/hplip.conf
endef

define Package/hplip-scan
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing - Scanning libraries
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS+=+hplip +libsane
endef

define Package/hplip-scan/description
	Libraries to use HP Printers/Scanners with Sane.
endef

define Package/hplip-ppds
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing - PPD files
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS+=+hplip
endef

define Package/hplip-ppds/description
	PPD files for CUPS.
endef

define Package/hplip-tools
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=HP Linux Imaging and Printing - Command line tools
  URL:=http://sourceforge.net/projects/hplip/
  DEPENDS+=+hplip +hplip-scan
endef

define Package/hplip-tools/description
	Command line tools for managing your printer.
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

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		$(TARGET_CONFIGURE_OPTS) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		all install
endef

define Package/hplip/install
	$(INSTALL_DIR) $(1)/etc/hp
	$(CP) $(PKG_INSTALL_DIR)/etc/hp/hplip.conf $(1)/etc/hp/hplip.conf

	$(INSTALL_DIR) $(1)/usr/share/hplip/data/models
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/data/models/models.dat $(1)/usr/share/hplip/data/models

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib

	$(INSTALL_DIR) $(1)/usr/lib/cups/backend
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cups/backend/hp $(1)/usr/lib/cups/backend

	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/cups/filter/* $(1)/usr/lib/cups/filter

	$(INSTALL_DIR) $(1)/usr/share/cups/drv/hp
	$(CP) $(PKG_INSTALL_DIR)/usr/share/cups/drv/hp/hpcups.drv $(1)/usr/share/cups/drv/hp
endef

define Package/hplip-scan/install
	$(INSTALL_DIR) $(1)/usr/lib/sane
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/sane/*.so* $(1)/usr/lib/sane
endef

define Package/hplip-ppds/install
	$(INSTALL_DIR) $(1)/usr/share/ppd/HP
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ppd/HP/* $(1)/usr/share/ppd/HP
	$(CP) $(PKG_BUILD_DIR)/ppd/hpcups/* $(1)/usr/share/ppd/HP
endef

define Package/hplip-tools/install
	$(INSTALL_DIR) $(1)/usr/lib/python2.7/site-packages
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/python2.7/site-packages/*.so $(1)/usr/lib/python2.7/site-packages

	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin

	$(INSTALL_DIR) $(1)/usr/share/hplip
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/share/hplip/*.py $(1)/usr/share/hplip
	$(CP) $(PKG_INSTALL_DIR)/usr/share/hplip/{base,copier,pcard,prnt,scan,check-plugin.py,hplip_clean.sh} $(1)/usr/share/hplip
endef

$(eval $(call BuildPackage,hplip))
$(eval $(call BuildPackage,hplip-scan))
$(eval $(call BuildPackage,hplip-ppds))
$(eval $(call BuildPackage,hplip-tools))
