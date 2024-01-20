# Maintainer: bigshans <wo199710@hotmail.com>

pkgname=beaver-notes
pkgver=2.5.0
pkgrel=1
epoch=
pkgdesc="A privacy-focused, cross-platform note-taking application."
_electron=electron
arch=('x86_64')
url="https://www.beavernotes.com/"
license=('MIT')
depends=(electron)
makedepends=('asar' 'npm' 'yarn' 'nodejs' 'imagemagick' 'libxcrypt-compat')
provides=('beaver-notes')
source=("$pkgname-$pkgver.tar.gz::https://github.com/Daniele-rolli/Beaver-Notes/archive/refs/tags/$pkgver.tar.gz"
        "beaver-notes.desktop"
        "no-font.patch")
sha256sums=('33c419603fe88a164660adb8a37abca66433260e98008e030615e8bc615a407f'
            '4475ac27a250fd89667e0c7130863e666725c7f41a605df5a05889515b29cfb3'
            'faf1f0ca0b1f99116d722edf437f94502a2cc1e8fe64ff22bfec209716535bab')

build() {
	cd "Beaver-Notes-$pkgver"

  patch -Np1 -i ../no-font.patch
	# Build the application
	yarn install
	yarn build
	yarn electron-builder build --config electron-builder.config.cjs --linux dir --x64 --config.asar=true
	
	# Convert icon to standard conforming png format
	convert buildResources/icon.ico buildResources/icon.png
}

package() {
    cd "Beaver-Notes-$pkgver"
	install -dm 755 "$pkgdir"/usr/lib/$pkgname
  asar extract ./dist/linux-unpacked/resources/app.asar ./dist/linux-unpacked/resources/app
  rm -rf ./dist/linux-unpacked/resources/app/node_modules
	# Copy full application to destiation directory
	cp -r --no-preserve=ownership --preserve=mode dist/linux-unpacked/resources/app "$pkgdir"/usr/lib/$pkgname
	install -dm 755 "$pkgdir"/usr/bin
    echo '#!/bin/sh
exec electron /usr/lib/beaver-notes/app "$@"' >> "$pkgdir"/usr/bin//$pkgname
  chmod +x "$pkgdir"/usr/bin/$pkgname
	
	# Install desktop file
	install -Dm 644 ../beaver-notes.desktop "$pkgdir"/usr/share/applications/beaver-notes.desktop
	
	# Install icon
	install -Dm 644 buildResources/icon-7.png "$pkgdir"/usr/share/icons/hicolor/256x256/apps/beaver-notes.png

	# Install license
	install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
