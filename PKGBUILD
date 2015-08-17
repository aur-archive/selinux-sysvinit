# $Id: PKGBUILD 168539 2012-10-13 09:28:52Z thomas $
# Maintainer: Nicky726 <Nicky726@gmail.com>
# Contributor: Eric Belanger <eric@archlinux.org>

pkgbase=selinux-sysvinit
pkgname=('selinux-sysvinit')
true && pkgname=('selinux-sysvinit-tools' 'selinux-sysvinit')
_origname=sysvinit
pkgver=2.88
pkgrel=9
arch=('i686' 'x86_64')
url="http://savannah.nongnu.org/projects/sysvinit"
license=('GPL')
groups=('selinux' 'selinux-system-utilities')
depends=('selinux-util-linux' 'selinux-coreutils' 'glibc' 'awk')
source=(http://download.savannah.gnu.org/releases/sysvinit/${_origname}-${pkgver}dsf.tar.bz2
        "0001-simplify-writelog.patch"
        "0002-remove-ansi-escape-codes-from-log-file.patch"
        ${_origname}-2.88-selinux.patch)
sha1sums=('f2ca149df1314a91f3007cccd7a0aa47d990de26'
          '326112c8a9bd24cb45bd4bb2f958a25f0ac4773d'
          'bbecfa7dfa45ac7c37ed8ac59fb53f6a85064b32'
          '9ed8746e61860c11c1152c6727e31d29c3d81d4f')

build() {
  cd "${srcdir}/${_origname}-${pkgver}dsf"

  # FS#30005
  patch -p1 -d "src" -i "${srcdir}/0001-simplify-writelog.patch"
  patch -p1 -d "src" -i "${srcdir}/0002-remove-ansi-escape-codes-from-log-file.patch"

  patch -Np1 -i "${srcdir}/${_origname}-2.88-selinux.patch"
  make WITH_SELINUX=yes
}

package_selinux-sysvinit-tools() {
  pkgdesc="SELinux aware Linux System V Init Tools"
	conflicts=('sysvinit-tools')
	provides=("sysvinit-tools=${pkgver}-${pkgrel}")

  cd "${srcdir}/${_origname}-${pkgver}dsf"
  make ROOT="${pkgdir}" install

  # provided by util-linux
  cd "${pkgdir}"
  rm bin/mountpoint
  rm usr/share/man/man1/mountpoint.1
  rm usr/bin/{mesg,utmpdump,wall}
  rm usr/share/man/man1/{mesg,utmpdump,wall}.1
  rm sbin/sulogin
  rm usr/share/man/man8/sulogin.8

  ### split out sysvinit
  rm -rf ${srcdir}/_sysvinit
  install -dm755 \
        ${srcdir}/_sysvinit/sbin \
        ${srcdir}/_sysvinit/usr/share/man/man8
  cd ${srcdir}/_sysvinit
  mv ${pkgdir}/sbin/{halt,init,poweroff,reboot,runlevel,shutdown,telinit} sbin/
  mv ${pkgdir}/usr/share/man/man5 usr/share/man/
  mv ${pkgdir}/usr/share/man/man8/{halt,init,poweroff,reboot,runlevel,shutdown,telinit}.8 usr/share/man/man8/
}

package_selinux-sysvinit() {
  pkgdesc="SELinux aware Linux System V Init"
  depends=('selinux-sysvinit-tools')
  conflicts=("${_origname}")
  provides=("${_origname}=${pkgver}-${pkgrel}")
  install=sysvinit.install

  mv "${srcdir}"/_sysvinit/* $pkgdir
}
