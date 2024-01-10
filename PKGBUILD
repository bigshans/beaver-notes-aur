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
makedepends=('npm' 'yarn' 'nodejs' 'imagemagick' 'libxcrypt-compat')
provides=('beaver-notes')
source=("$pkgname-$pkgver.tar.gz::https://github.com/Daniele-rolli/Beaver-Notes/archive/refs/tags/$pkgver.tar.gz"
        "beaver-notes.desktop")
sha256sums=("SKIP"
            "988dc1020793d118dd2dc20745881ee6f4db221c61be503dc762f0d43318dfee")

build() {
	cd "Beaver-Notes-$pkgver"

	# Build the application
	yarn install
	yarn build
	yarn electron-builder build --config electron-builder.config.cjs --linux dir --x64 --config.asar=true
	
	# Convert icon to standard conforming png format
	convert buildResources/icon.ico buildResources/icon.png
}

package() {
    cd "Beaver-Notes-$pkgver"
	# Copy full application to destiation directory
	install -dm 755 "$pkgdir"/opt/$pkgname
    echo '#!/bin/env bash
electron /opt/beaver-notes/resources/app.asar' >> "$pkgdir"/opt/$pkgname/$pkgname
  chmod +x "$pkgdir"/opt/$pkgname/$pkgname
	cp -r --no-preserve=ownership --preserve=mode dist/linux-unpacked/resources "$pkgdir"/opt/$pkgname/resources
	
	# Install desktop file
	install -Dm 644 ../beaver-notes.desktop "$pkgdir"/usr/share/applications/beaver-notes.desktop
	
	# Install icon
	install -Dm 644 buildResources/icon-7.png "$pkgdir"/usr/share/icons/hicolor/256x256/apps/beaver-notes.png

	# Install license
	install -Dm 644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
