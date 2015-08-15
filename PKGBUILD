# Maintainer: Robert Charlton <rectec [at] mail [dot] com>
# Contributor: Doug Newgard <scimmia22 at outlook dot com>

pkgname=gaminganywhere
pkgver=0.7.5
pkgrel=1
pkgdesc="Open-source cloud gaming platform"
arch=('i686' 'x86_64')
url="http://gaminganywhere.org"
license=('BSD')
depends=('ffmpeg' 'sdl2_ttf')
makedepends=('live-media')
install='gaminganywhere.install'
source=("http://gaminganywhere.org/dl/gaminganywhere-$pkgver-all-in-one.tar.bz2"
	'gaminganywhere.install')
md5sums=('b224b8111ae5752f4c3c1e7f7840a2dc'
	'c7993bd7a6575d92e5f9e925408d6b7c ')

prepare() {
  cd "$srcdir/$pkgname-$pkgver"

  rm -rf bin deps.* env-setup

# Include header for sleep function
  sed -i '/#include <stdarg.h>/a #include <unistd.h>' ga/client/ga-client.cpp

# Set right include dirs for live-media
  sed -i '/^L5CF/ s|=.*|= -I/usr/include/liveMedia -I/usr/include/BasicUsageEnvironment -I/usr/include/UsageEnvironment -I/usr/include/groupsock|' ga/Makefile.def

# Get rid of all of the GADEPS junk from the makefiles. Not stricly required, but cleans things up
  sed -i '/^GADEPS/d;s/\S*GADEPS\S*//g' ga/Makefile.def ga/module/Makefile.common ga/{client,core,server/event-posix,server/periodic}/Makefile

# Fix for build with live-media >= 2013.06.14
  sed -i '1210 s/)/, -1)/' ga/client/rtspclient.cpp
}

build() {
  cd "$srcdir/$pkgname-$pkgver/ga"

  make all
  make install
}

package() {
  cd "$srcdir/$pkgname-$pkgver"

  install -dm755 "$pkgdir/"{opt,usr/bin}
  cp -a bin "$pkgdir/opt/gaminganywhere"

  printf "#!/bin/bash\ncd /opt/gaminganywhere\n./ga-client \$@\n" > "$pkgdir/usr/bin/ga-client"
  printf "#!/bin/bash\ncd /opt/gaminganywhere\n./ga-server-periodic \$@\n" > "$pkgdir/usr/bin/ga-server-periodic"
  chmod 755 "$pkgdir/usr/bin/"{ga-client,ga-server-periodic}

  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
