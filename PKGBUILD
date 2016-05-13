# Maintainer: Christopher Bero <bigbero@gmail.com>
# Taken from https://aur.archlinux.org/packages/mspds
# PKGBUILD copied from https://github.com/greigdp/msp430-mspds
# Contributor: Alexei Colin <ac@alexeicolin.com>
pkgname=mspds
pkgver=3.7.0.0012
pkgrel=1
pkgdesc="MSP430 Debug Stack. Contains a dynamic link library as well as embedded firmware that runs on the MSP-FET430UIF or the eZ430 emulators."
arch=('i686' 'x86_64')
url="http://processors.wiki.ti.com/index.php/MSPDS_Open_Source_Package"
# Licenses were found in "Manifest MSPDebugStack OS Package.pdf" from the mspds source archive.
license=('custom:TI BSD' 'custom:IAR BSD' 'custom: TI TSPA')
group=('msp430')
depends=('hidapi' 'boost')
makedepends=('unzip' 'dos2unix')
optdepends=('mspdebug')
noextract=('slac460p.zip')
source=('http://www.ti.com/lit/sw/slac460p/slac460p.zip'
        'fix.patch')

sha256sums=('2a61b01c688ea4b73431d6b9f42131e83a9391bfe5465a978d39543996bb433f'
            'c6124c52f57516250de8a8621d2d58442c3af3c219eaf2e3556fc8270fc80f7e')

prepare() {
    unzip -o slac460p.zip MSPDSOS-3.07.000.012-linux-installer.run
    chmod +x MSPDSOS-3.07.000.012-linux-installer.run
    ./MSPDSOS-3.07.000.012-linux-installer.run --mode unattended --prefix mspds
    find ./mspds/slac460/ -type f -exec dos2unix -q '{}' \;
    # This hidapi patch allows us to build mspds from the hidapi Archlinux package rather than the v0.7 source.
    patch -p1 -d ./mspds/slac460/ < ../fix.patch

    # This patch fixes a compile error related to namespaces
    #patch -p1 -d MSPDebugStack_OS_Package < ../ambiguous-ifstream.patch
}

build() {
    cd "$srcdir/mspds/slac460"
    # The -j flag is the number of parallel jobs to run, adjust accordningly.
    make
}

package() {
    install -Dm644 "$srcdir/mspds/slac460/libmsp430.so" "$pkgdir/usr/lib/libmsp430.so"
    mkdir -p "$pkgdir/usr/include/libmsp430"
    install -Dm644 "$srcdir/mspds/slac460/DLL430_v3/include/"* "$pkgdir/usr/include/libmsp430"
}
