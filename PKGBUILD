# Maintainer: hiddeninthesand <hiddeninthesand at pm dot me>
# Contributor:         steiner <daimarstein@pm.me>

## AUR Maintainer:     barfin <barfin@protonmail.com>
## AUR Co-Maintainer:  Jaja <jaja@mailbox.org>
## AUR Co-Maintainer:  floriplum <floriplum@mailbox.org>

## pkginfo
pkgdesc='A fancy custom distribution of Valves Proton with various patches'
pkgname='proton-ge-custom-bin'
pkgver='8.3'
pkgrel='1'
arch=('x86_64')
license=('BSD' 'LGPL' 'zlib' 'MIT' 'MPL' 'custom')
changelog='changelog.md'
provides=('proton' "proton-ge-custom=${pkgver/_/.}")
conflicts=('proton-ge-custom')

## dependencies
makedepends=('patch')
depends=('python3' 'libvulkan1' 'flac' 'speex' 'libgstreamer1.0-0' 'libgudev-1.0-0' 'mpg123' 'libtheora0' 'ffmpeg' 'libopenal1' 'va-driver-all' 'libvdpau1' 'libturbojpeg')
optdepends=('kdialog: KDE splash dialog support'
	'zenity: GNOME splash dialog support'
	'steam: use proton with steam like intended'
	'vulkan-driver: driver to be used by DXVK'
	'winetricks: protonfixes backend - highly recommended'
	'wine: support for 32bit prefixes'
	'xboxdrv: gamepad driver service')
## makepkg options
options=(!strip emptydirs)

## fix naming conventions, matching upstream
_pkgname="${pkgname//-bin/}"
_srcdir="GE-Proton${pkgver//./-}"

## paths and files
_protondir="usr/share/steam/compatibilitytools.d/${_pkgname}"
_licensedir="usr/share/licenses/${_pkgname}"
_execfile='usr/bin/proton'
_protoncfg="${_protondir}/user_settings.py"

## user edited files to backup
backup=("/${_protoncfg}")

## sources
url='https://github.com/GloriousEggroll/proton-ge-custom'
source=("${_pkgname}-${pkgver}.tar.gz::${url}/releases/download/${_srcdir}/${_srcdir}.tar.gz"
	'user_settings.py'
	'launcher.sh')
b2sums=('26dc7cb707ae9fe9d43795b3da1a966522da838ac79532f40bc3e7e75709b16e9f06d8327ca33d880864ab739b25a46c8627a99f6a23d0892dae52f82d3d9bb9'
        'd39460c4d5aada5a30c9fb7d9d7bd14c59a5c71d5af0d9ab7f3a6d2ee4e3dcc80026cf4bfc3682318ae5aeea5d318270bad21b8a6b88c978df5f4a325da39f84'
        '1c7f252e2f1229140bb628e820266137fd16b503bdf80680db8bd463ad60327217023b65c8080829d6354f8ce0bc673eabb95b7fd409c65650cf163dd16f8884')

build() {
	## patches
	sed -i "s|_proton=echo|_proton=/${_protondir}/proton|" "${srcdir}"/launcher.sh
	sed -i -r 's|"GE-Proton.*"|"Proton-GE"|' "${_srcdir}"/compatibilitytool.vdf
	## remove artifacts
	rm "${_srcdir}"/protonfixes/*.tar.xz
	rm -rf "${_srcdir}"/protonfixes/.git*
	## fixes from namcap inspection
	strip --preserve-dates --strip-unneeded "${_srcdir}"/files/bin/wine*
}

package() {
	## create paths
	install -d "${pkgdir}/${_protondir}/"
	install -d "${pkgdir}/${_licensedir}/"
	install -d "${pkgdir}/$(dirname ${_execfile})/"
	## licenses
	mv "${_srcdir}/LICENSE" "${pkgdir}/${_licensedir}/license"
	mv "${_srcdir}/LICENSE.OFL" "${pkgdir}/${_licensedir}/license_OFL"
	mv "${_srcdir}/PATENTS.AV1" "${pkgdir}/${_licensedir}/license_AV1"
	mv "${_srcdir}/protonfixes/LICENSE" "${pkgdir}/${_licensedir}/license_protonfixes"
	## config files
	install --mode=0775 --group=50 "${srcdir}"/user_settings.py "${pkgdir}/${_protoncfg}"
	## executables
	mv "${_srcdir}"/* "${pkgdir}/${_protondir}"
	install --mode=0755 "${srcdir}"/launcher.sh "${pkgdir}/${_execfile}"
}
