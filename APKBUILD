# Contributor: Patrick Wu <wotingwu@live.com>
# Maintainer: Natanael Copa <ncopa@alpinelinux.org>
pkgname=fakeroot
pkgver=1.23
pkgrel=2
pkgdesc="Gives a fake root environment using TCP method, useful for building packages as a non-privileged user"
arch="all"
license="GPL-3.0-or-later"
url="https://packages.debian.org/fakeroot"
depends=
options="!checkroot"
checkdepends="bash"
makedepends_build="libtool autoconf automake po4a"
makedepends_host="libcap-dev acl-dev linux-headers"
makedepends="$makedepends_build $makedepends_host"
subpackages="$pkgname-doc"
source="http://ftp.debian.org/debian/pool/main/f/fakeroot/fakeroot_${pkgver}.orig.tar.xz
	fakeroot-hide-dlsym-errors.patch
	fakeroot-no64.patch
	fakeroot-stdint.patch
	fakeroot-no-ldlibrarypath.patch
	xstatjunk.patch
	"
builddir="$srcdir"/fakeroot-$pkgver

check() {
	cd "$builddir"
	make check
}

build() {
	cd "$builddir"

	if [ "$CLIBC" = "musl" ]; then
		# musl does not have _STAT_VER, it's really not used for
		# anything, so define it as zero (just like uclibc does)
		export CFLAGS="-D_STAT_VER=0 $CFLAGS"
	fi

	CONFIG_SHELL=/bin/sh ./bootstrap
	CONFIG_SHELL=/bin/sh ./configure \
		--build=$CBUILD \
		--host=$CHOST \
		--prefix=/usr \
		--disable-static \
		--with-ipc=tcp

	make
	cd doc
	po4a -k 0 --rm-backups --variable "srcdir=../doc/" po4a/po4a.cfg
}

package() {
	cd "$builddir"
	make DESTDIR="$pkgdir" install
}

sha512sums="0984679207e6e340abf715d4b26a213f85420cd8c58f21e65eb069337a3bd67436c6f80168412c10b28701689ec63290f122a5ff5d44a57b2b166aa72799d036  fakeroot_1.23.orig.tar.xz
666f41d6adc5e65eba419e08d5bbc4f561e40b0fc7bfa82090eb87962a7f3193bf319754e04aca289e865c66df2ecced1dbb45c9aa9f093657f22193dda25354  fakeroot-hide-dlsym-errors.patch
7a832e6bed3838c7c488e0e12ba84b8d256e84bbb06d6020247452a991de505fa5c6bd7bcb84dce8753eb242e0fcab863b5461301cd56695f2b003fe8d6ff209  fakeroot-no64.patch
ed7a58b0d201139545420f9e5429f503c00e00f36dea84473e77ea99b23bb8d421da1a8a8ce98ff90e72e378dff4cb9ea3c1a863a969899a5f50dfac3b9c5fac  fakeroot-stdint.patch
acfc1e5efce132279adddf9e11c28d65602059d5cd723ad98b67cb9183e1de68445f3bba7ac54ee60265b85f25141fcc9b2156f551aa5c624a92631320f5b743  fakeroot-no-ldlibrarypath.patch
5efd33fd778bd94a529ed7e439fb8fea25ff865dda3f6f9e431264e942b37f3b5d7a0ad14107b55c5fa81b86efd5a82aedb3803cfab08ec57f27f5b229d2fe88  xstatjunk.patch"
