# Maintainer: Hunter Wittenborn <hunter@hunterwittenborn.com>
pkgname=chezmoi
pkgver=2.33.6
pkgrel=1
pkgdesc='Manage your dotfiles across multiple diverse machines, securely.'
arch=('any')
makedepends=('git' 'golang-go>=2:1.17')
license=('MIT')
url='https://chezmoi.io'

source=("${pkgname}-${pkgver}::git+https://github.com/twpayne/chezmoi/#tag=v${pkgver}")
sha256sums=('SKIP')

build() {
    cd "${pkgname}-${pkgver}/"
    make build-in-git-working-copy
}

package() {
    cd "${pkgname}-${pkgver}/"
    install -Dm 755 "./${pkgname}" "${pkgdir}/usr/bin/${pkgname}"
    "./${pkgname}" completion bash | install -Dm 644 /dev/stdin "${pkgdir}/usr/share/bash-completion/completions/${pkgname}"
}

# vim: set sw=4 expandtab: