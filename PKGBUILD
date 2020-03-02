# Maintainer: Lily Wilson <hotaru@thinkindifferent.net>
pkgname=raspberrypi-userland-aarch64-git
pkgver=r764.6e6a2c8
pkgrel=1
pkgdesc="aarch64-compatible bits of /opt/vc for Raspberry Pi (vcgencmd, tvservice, etc.)" 
arch=('aarch64')
url="https://github.com/raspberrypi/userland"
license=('custom')
makedepends=('git' 'cmake' 'ed')
provides=('raspberrypi-firmware' 'raspberrypi-userland-aarch64')
source=('git+https://github.com/raspberrypi/userland.git'
        'raspberrypi-userland.conf'
        'raspberrypi-userland.sh')
md5sums=('SKIP'
         '72e0d5818fc513ece1b964f25f7e7882'
         '734f220923521411450b7cb48333c142')

pkgver() {
	cd "$srcdir/userland"

	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
	cd "$srcdir/userland"
	echo -e "1,\$s/sudo /true sudo /\nw\nq"|ed buildme
}

build() {
	cd "$srcdir/userland"
	./buildme --aarch64
}

package() {
	cd "$srcdir/userland"
	install -Dm755 -t "$pkgdir/opt/vc/bin" build/bin/*
	install -Dm644 -t "$pkgdir/opt/vc/lib" build/lib/*
	cd build/inc
        find . -type f -printf "install -Dm644 -t \"$pkgdir/opt/vc/include/%h\" %h/%f\n" | sh
	cd "$srcdir/userland"
	install -Dm644 -t "$pkgdir/etc/ld.so.conf.d" "$srcdir/raspberrypi-userland.conf"
        install -Dm644 -t "$pkgdir/etc/profile.d" "$srcdir/raspberrypi-userland.sh"
        install -Dm644 LICENCE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"    
}
