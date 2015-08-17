# $Id$
# Maintainer: Balló György <ballogyor+arch at gmail dot com>
# Contributor: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

pkgbase=totem-gst
_pkgbase=totem
pkgname=totem-gst
true && pkgname=('totem-gst' 'totem-plugin-gst')
pkgver=3.4.3
pkgrel=2
pkgdesc="A GNOME3 integrated movie player based on Gstreamer (uses the old GStreamer backend without Clutter)."
url="http://www.gnome.org"
arch=('i686' 'x86_64')
license=('GPL2' 'custom')
depends=('gstreamer0.10-base-plugins' 'gstreamer0.10-good-plugins' 'totem-plparser' 'libxxf86vm'
         'libxtst' 'desktop-file-utils' 'iso-codes' 'python2' 'libpeas' 'hicolor-icon-theme'
         'gnome-icon-theme-symbolic' 'gsettings-desktop-schemas' 'dconf'
         'python2-gobject')
makedepends=('intltool' 'gtk-doc' 'nautilus' 'libgdata' 'lirc-utils'
             'libepc' 'bluez' 'vala' 'grilo' 'libzeitgeist' 'pylint' 'gnome-common')
options=('!libtool' '!emptydirs')
source=(http://ftp.gnome.org/pub/gnome/sources/$_pkgbase/${pkgver%.*}/$_pkgbase-$pkgver.tar.xz
        browser-plugins.ini
        grilo-0.2.patch
        revert-clutter.patch)
sha256sums=('fce76e3c924d0ffd3d4eef2e69af4c44b3537b26c2df86745e900b4c829b38db'
            'a50a3bbf35f0535f7e8e20af1893446a2e5711015484f9ae6d1ff91af3b23c4e'
            'c4b280199c6ab039f094f9c7ba67909c356c0d0b577032bc265b41e35ba580c3'
            '6fd15c43300ad2f3ff9d4aea6ea3f80a9ac1f1169ba07394ac762371903d3769')

build() {
  cd "$_pkgbase-$pkgver"
  patch -Np1 -i "$srcdir/revert-clutter.patch"
  patch -Np1 -i "$srcdir/grilo-0.2.patch"
  autoreconf -fi
  ./configure --prefix=/usr \
    --sysconfdir=/etc \
    --libexecdir=/usr/lib/totem \
    --localstatedir=/var \
    --disable-static \
    --enable-python \
    --enable-nautilus   
  make
}

package_totem-gst() {
  groups=('gnome-extra')
  install=totem.install
  optdepends=('gstreamer0.10-ugly-plugins: Extra media codecs'
              'gstreamer0.10-bad-plugins: Extra media codecs'
              'gstreamer0.10-ffmpeg: Extra media codecs'
              'lirc-utils: Infrared Remote Control plugin'
              'libepc: Publish Playlist plugin'
              'libgdata: YouTube Browser plugin'
              'bluez: Bemused plugin'
              'grilo-plugins: Browse sidebar (remote media)'
              'pyxdg: opensubtitles plugin'
              'libzeitgeist: Zeitgeist plugin')
  conflicts=('totem')

  cd "$_pkgbase-$pkgver"
  make DESTDIR="${pkgdir}" install

  rm -r "$pkgdir/usr/lib/mozilla"
  rm "$pkgdir/usr/lib/totem/totem-plugin-viewer"

  sed -i "s|#!/usr/bin/python$|#!/usr/bin/python2|" \
    $pkgdir/usr/lib/totem/{totem/totem-bugreport.py,plugins/iplayer/iplayer2.py}

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/totem-gst/COPYING"
}

package_totem-plugin-gst() {
  pkgdesc="Totem plugin for web browsers (uses the old GStreamer backend without Clutter)"
  depends=("totem-gst=$pkgver")
  conflicts=('totem-plugin')
  backup=(etc/totem/browser-plugins.ini)

  cd "$_pkgbase-$pkgver"
  make -C browser-plugin \
    plugindir=/usr/lib/mozilla/plugins \
    xptdir=/usr/lib/mozilla/plugins \
    DESTDIR="$pkgdir" install

  install -Dm644 ../browser-plugins.ini "$pkgdir/etc/totem/browser-plugins.ini"

  install -Dm644 COPYING "$pkgdir/usr/share/licenses/totem-plugin-gst/COPYING"
}

# AUR workaround
pkgdesc="A GNOME3 integrated movie player based on Gstreamer (uses the old GStreamer backend without Clutter)."
depends=('gstreamer0.10-base-plugins' 'gstreamer0.10-good-plugins' 'totem-plparser' 'libxxf86vm'
         'libxtst' 'desktop-file-utils' 'iso-codes' 'python2' 'libpeas' 'hicolor-icon-theme'
         'gnome-icon-theme-symbolic' 'gsettings-desktop-schemas' 'dconf'
         'python2-gobject')
