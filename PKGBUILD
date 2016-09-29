# $Id$
# Maintainer: Eric Bélanger <eric@archlinux.org>

pkgname=syslog-ng
pkgver=3.8.1
pkgrel=2
pkgdesc="Next-generation syslogd with advanced networking and filtering capabilities"
arch=('i686' 'x86_64')
url="http://www.balabit.com/network-security/syslog-ng/"
license=('GPL2' 'LGPL2.1')
depends=('awk' 'eventlog' 'systemd' 'glib2' 'libdbi')
makedepends=('python2' 'libxslt' 'json-c')
optdepends=('logrotate: for rotating log files'
	    'json-c: for json-plugin')
backup=('etc/syslog-ng/scl.conf'
        'etc/syslog-ng/syslog-ng.conf'
        'etc/logrotate.d/syslog-ng')
source=(https://github.com/balabit/syslog-ng/releases/download/${pkgname}-${pkgver}/${pkgname}-${pkgver}.tar.gz
        syslog-ng.conf syslog-ng.logrotate afsocket-tcp-Include-missing-setsockopt-header.patch)
sha256sums=('84b081f6e5f98cbc52052e342bcfdc5de5fe0ebe9f5ec32fe9eaec5759224cc5'
            '680c3498a973ee2635c8a28086a39079071bd2fcfd499fc38bf1f1b184f4c6c4'
            '93c935eca56854011ea9e353b7a1da662ad40b2e8452954c5b4b5a1d5b2d5317'
            'f391eb5e1d60928b96b04ed7cba4590b2782ff8d19d83bffa1c691fe5e26142e')

prepare() {
  cd $pkgname-$pkgver
  sed -i -e 's,/bin/,/usr/bin/,' -e 's,/sbin/,/bin/,' contrib/systemd/syslog-ng.service
  patch -p1 < $startdir/afsocket-tcp-Include-missing-setsockopt-header.patch
}

build() {
  cd $pkgname-$pkgver
  ./configure --prefix=/usr --sysconfdir=/etc/syslog-ng --libexecdir=/usr/lib \
    --sbindir=/usr/bin --localstatedir=/var/lib/syslog-ng --datadir=/usr/share \
    --with-pidfile-dir=/run --disable-spoof-source --enable-ipv6 --enable-sql \
    --enable-systemd --with-systemdsystemunitdir=/usr/lib/systemd/system \
    --enable-manpages --with-jsonc=system
  make
}

check() {
  cd $pkgname-$pkgver
  make check
}

package() {
  make -C $pkgname-$pkgver DESTDIR="$pkgdir" install
  install -dm755 "$pkgdir/var/lib/syslog-ng" "$pkgdir/etc/syslog-ng/patterndb.d"
  install -Dm644 "$srcdir/syslog-ng.conf" "$pkgdir/etc/syslog-ng/syslog-ng.conf"
  install -Dm644 "$srcdir/syslog-ng.logrotate" "$pkgdir/etc/logrotate.d/syslog-ng"
}
