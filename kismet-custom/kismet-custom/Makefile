PKG_NAME:=kismet-custom

PKG_BUILD_PARALLEL:=1
HOST_BUILD_PARALLEL:=1

include ../kismet.mk

define Package/kismet-custom
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Kismet (26 January 2021)
  URL:=https://www.kismetwireless.net/
  DEPENDS:=+libpthread +libpcap +libpcre +libstdcpp +libncurses +libsqlite3 +zlib +protobuf-lite +libprotobuf-c
  SUBMENU:=Kismet (Custom)
endef

define Package/kismet-custom/description
  Modern Kismet wireless capture and IDS, with the web-ui based Kismet
endef

define Build/Configure
  $(call Build/Configure/Default,--with-linux-headers=$(LINUX_DIR))
endef

CONFIGURE_ARGS += \
	--sysconfdir=/etc/kismet \
	--bindir=/usr/bin \
	--disable-python-tools \
	--with-protoc=$(STAGING_DIR_HOSTPKG)/bin/protoc \
	--enable-protobuflite \
	--disable-element-typesafety \
	--disable-libusb \
	--disable-debuglibs \
	--disable-libcap

# Override libs to force lpthread because we don't detect it properly on OpenWRT
CONFIGURE_VARS += \
	CXXFLAGS="$$$$CXXFLAGS"  \
	LIBS="-lm -lpthread"

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
	LD="$(TARGET_CXX)" \
	kismet
endef

define Package/kismet-custom/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/kismet $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/kismet/
	$(INSTALL_CONF) ./files/kismet_package.conf $(1)/etc/kismet/
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_80211.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_alerts.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_filter.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_httpd.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_logging.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_memory.conf $(1)/etc/kismet
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/conf/kismet_uav.conf $(1)/etc/kismet
	cp -Rv $(PKG_BUILD_DIR)/http_data $(1)/etc/kismet/
endef

$(eval $(call BuildPackage,kismet-custom))
