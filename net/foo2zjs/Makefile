include $(TOPDIR)/rules.mk

PKG_NAME:=foo2zjs
PKG_VERSION:=git-e04290d
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/koenkooi/foo2zjs.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=e04290de6b7a30d588f3411fd9834618e09f7b9b

PKG_LICENSE:=GPL-2
PKG_LICENSE_FILES:=COPYING
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/foo2zjs
	SECTION:=net
	CATEGORY:=Network
	TITLE:=A linux printer drivers for ZjStream protocol
	DEPENDS:=+openprinting-cups-filters
	URL:=http://foo2zjs.rkkda.com/
	SUBMENU:=Printing
endef

define Package/foo2zjs/description
	This is a driver originally developed for the Minolta magicolor 2300 DL network color laser printer which uses the Zenographics protocol.\
	This driver will also work for the HP LaserJet 1000, 1005, 1018, 1020, 1022, P1102, P1566, P1606dn, P2035, and other Zenographics-based printers.
endef

define Build/Configure
endef

define Build/Compile
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS)"
	rm -f $(PKG_BUILD_DIR)/arm2hpdl
	$(HOSTCC) $(PKG_BUILD_DIR)/arm2hpdl.c -o $(PKG_BUILD_DIR)/arm2hpdl
endef

define Build/Install
	DESTDIR=$(PKG_INSTALL_DIR) $(MAKE) -C $(PKG_BUILD_DIR) install-extra
endef

define Package/foo2zjs/install
	$(INSTALL_DIR) $(1)/usr/lib/cups/filter
	$(INSTALL_DIR) $(1)/usr/share/foomatic/db/source
	$(INSTALL_DIR) $(1)/usr/share/cups/model
	$(INSTALL_DIR) $(1)/usr/share/ppd
	$(INSTALL_DIR) $(1)/etc/hotplug.d/usb
	$(MAKE_VARS) DESTDIR=$(1) $(MAKE) -C $(PKG_BUILD_DIR) \
		USBDIR=$(1)/etc/hotplug.d/usb \
		UDEVDIR=$(1)/etc/hotplug.d/usb \
		VARPPD=$(1)/var/lp/ppd \
		install-prog install-icc2ps install-crd install-hotplug-prog \
		install-extra install-ppd install-foo
	rm -f $(1)/usr/bin/arm2hpdl
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hplj1005
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hplj1018
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hplj1020
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hpljP1005
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hpljP1006
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hpljP1007
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hpljP1008
	ln -fs /etc/hotplug.d/usb/hplj1000 $(1)/etc/hotplug.d/usb/hpljP1505
endef

define Package/foo2zjs/postinst
#!/bin/sh
# install-foo:
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-Media.xml
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-PaperSize.xml
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-Source.xml
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-DitherPPI.xml
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-Copies.xml
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-Nup.xml
rm -f /usr/share/foomatic/db/source/opt/foo2zjs-NupOrient.xml
rm -f /usr/share/foomatic/db/source/opt/foo2*-Quality.xml
rm -f /usr/share/foomatic/db/source/opt/foo2hp-AlignCMYK.xml
rm -f /usr/share/foomatic/db/source/printer/KonicaMinolta*.xml
rm -rf /var/cache/foomatic/*/*
rm -f /var/cache/foomatic/printconf.pickle

# install-ppd:
/etc/init.d/cupsd restart

# install-hotplug-prog:
/etc/hotplug.d/usb/hplj1000 install-usblp

# install-filter:
ln -sf /usr/bin/command2foo2lava-pjl /usr/lib/cups/filter/
endef

define Package/foo2zjs/prerm
#!/bin/sh
rm /usr/lib/cups/filter/command2foo2lava-pjl
rm /etc/hotplug.d/usb/hpl*
endef

$(eval $(call BuildPackage,foo2zjs))
