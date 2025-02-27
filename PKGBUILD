# Maintainer: Yauheni Kirylau <actionless dot loveless AT gmail.com>
# shellcheck disable=SC2034,SC2154

pkgname=pikaur-git
pkgver=1.6.16.2
pkgrel=1
pkgdesc="AUR helper which asks all questions before installing/building. Inspired by pacaur, yaourt and yay."
arch=('any')
url="https://github.com/actionless/pikaur"
license=('GPL3')
source=(
	"$pkgname::git+https://github.com/actionless/pikaur.git#branch=master"
)
md5sums=(
	"SKIP"
)
depends=(
	'pyalpm'
	'git'
)
makedepends=(
	'python-commonmark'
)
optdepends=(
	'asp: for ABS support in -G/--getpkgbuild operation'
	'python-pysocks: for socks5 proxy support'
)
conflicts=('pikaur')
provides=('pikaur')

pkgver() {
	cd "${srcdir}/${pkgname}" || exit 2
	set -o pipefail
	git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g' || echo 0.0.1
}

build() {
	cd "${srcdir}/${pkgname}" || exit 2
	sed -i -e "s/VERSION.*=.*/VERSION = '${pkgver}'/g" pikaur/config.py
	make
}

package() {
	cd "${srcdir}/${pkgname}" || exit 2
	/usr/bin/python3 setup.py install --prefix=/usr --root="$pkgdir/" --optimize=1
	for langmo in $(cd ./locale && ls ./*.mo); do
		lang=$(sed -e 's/.mo$//' <<< "${langmo}")
		install -Dm644 "locale/${langmo}" "$pkgdir/usr/share/locale/${lang}/LC_MESSAGES/pikaur.mo"
	done
	install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
	install -Dm644 pikaur.1 "$pkgdir/usr/share/man/man1/pikaur.1"
	cp -r ./packaging/* "${pkgdir}"
	cp -r ./dist/* "${pkgdir}"
}
