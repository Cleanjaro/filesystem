# Maintainer: David Spink <yorper_protonmail.com>
# Contributor: Philip Müller <philm@manjaro.org>
# Contributor: Sébastien Luttringer
# Contributor: Tom Gundersen <teg@jklm.no>

pkgname=filesystem
pkgver=2019.11
pkgrel=1
pkgdesc='Base Cleanjaro Linux files'
arch=('x86_64')
license=('GPL')
url='http://cleanjaro.org'
groups=('base')
depends=('iana-etc')
backup=('etc/crypttab' 'etc/fstab' 'etc/group' 'etc/gshadow' 'etc/host.conf'
        'etc/hosts' 'etc/hostname' 'etc/issue' 'etc/ld.so.conf' 'etc/locale.conf'
        'etc/motd' 'etc/nsswitch.conf' 'etc/passwd' 'etc/profile' 'etc/resolv.conf'
        'etc/securetty' 'etc/shadow' 'etc/shells' 'etc/vconsole.conf'
        'etc/modules-load.d/modules.conf')
source=('crypttab' 'env-generator' 'fstab' 'group' 'gshadow' 'home-local-bin.sh' 'host.conf'
        'hosts' 'hostname' 'issue' 'ld.so.conf' 'locale.conf' 'locale.sh' 'modules.conf' 'motd'
        'nsswitch.conf' 'os-release' 'passwd' 'profile' 'resolv.conf' 'securetty' 'shadow'
        'shells' 'sysctl' 'sysusers' 'tmpfiles' 'vconsole.conf')
sha256sums=('e03bede3d258d680548696623d5979c6edf03272e801a813c81ba5a5c64f4f82'
            'ed0cb4f1db4021f8c3b5ce78fdf91d2c0624708f58f36c9cf867f4d93c3bc6da'
            'e54626e74ed8fee4173b62a545ab1c3a3a069e4217a0ee8fc398d9933e9c1696'
            '244f0718ee2a9d6862ae59d6c18c1dd1568651eada91a704574fa527fbac2b3a'
            '90d879374f77bac47f132164c1e7fc4892e994ff1d1ac376efa0c1c26ea37273'
            'e6653e2b468339323f47d760bff67d91b2ce89437968d6debd9c86a97fa0fa79'
            '4d7b647169063dfedbff5e1e22cee77bd1a4183dbcfd5e802e68939da4bbf733'
            'd9cd8a77d9e0aa5e90d7f4ed74c8745c17b525e720e28e4c44364150003c35f9'
            'c6418587f646523343ace3990f01b527cdc8dfd89598a13a6fd8cd6d5750b18e'
            '786a237e748ea6c3e705e468db8204c2bc838b8e6bf938b54b10cd8db9e63fd8'
            'dad04a370e488aa85fb0a813a5c83cf6fd981ce01883fc59685447b092de84b5'
            'bc538cf42f96e24b8ec94c32242b6891de245e0207fab6937bfce3e23942742b'
            '8ca2d8eef6fb5143c9ef7e9174ccfef59ac7ad2deee243574cd10c763156cc10'
            'a8a1cd5c81b11498d43ba0e0b5de53de6f154a395d54171f44d2874b4f659053'
            'a7f7ec38a2374269b3e4e974c302bbff0ea2715806b4c4739ae2cb544205fa99'
            '3e17088d6141e3ef676528ac8ef4abd9c4584f8b65547c1236b55310e725e1bf'
            'b2151c12d7859fc3dee2ec537c3c36ae7a6b3ca0521c9a043262625c64f95904'
            '5e06477834f51abf42ea4e8dc199632afc6afbfd8c44354685a271e9a48d2c0a'
            '5da078777cda24e4df697e2928451723f2303bfdbb2ce9551c822188c7945d25'
            '5557d8e601b17a80d1ea7de78a9869be69637cb6a02fbfe334e22fdf64e61d4c'
            'd88be2b45b43605ff31dd83d6a138069b6c2e92bc8989b7b9ab9eba8da5f8c7b'
            '8ce994663d7588143ad7ed4441b07f468f4f7d3590164dd73ddfa3ea307ece8e'
            'c390b31fffc4a2b5d78ae8c89f5317aadef1f71baac09cfb467b675db1406d61'
            '89e43a0b7028f52d5c8e7fb961d962c4b4f4e9595880a6157274ddb2c7c0b6b4'
            'b5b28f395583d141d88c0b955cd05124f9b8cdf003feab01e55885b8e8c1303e'
            '618ac097441c1f2daffc9967e5c3cd18ea8866f776db62d04bf401c53907b1c9'
            'cd4a55177020a436254bb4baf84e068b98b3b0f6644173a7c853d58d236e00f1')

package() {
  cd "$pkgdir"

  # setup root filesystem
  for d in boot dev etc home mnt usr var opt srv/http run; do
    install -d -m755 $d
  done
  install -d -m555 proc
  install -d -m555 sys
  install -d -m0750 root
  install -d -m1777 tmp
  # vsftpd won't run with write perms on /srv/ftp
  # ftp (uid 14/gid 11)
  install -d -m555 -g 11 srv/ftp

  # setup /etc and /usr/share/factory/etc
  install -d etc/{ld.so.conf.d,skel,profile.d} usr/share/factory/etc
  for f in fstab group hostname host.conf hosts issue ld.so.conf locale.conf motd \
  nsswitch.conf passwd resolv.conf securetty shells profile vconsole.conf; do
    install -m644 "$srcdir"/$f etc/
    install -m644 "$srcdir"/$f usr/share/factory/etc/
  done
  ln -s ../proc/self/mounts etc/mtab
  for f in gshadow shadow crypttab; do
    install -m600 "$srcdir"/$f etc/
    install -m600 "$srcdir"/$f usr/share/factory/etc/
  done

  # add modules-load.d/modules
  install -D -m644 $srcdir/modules.conf etc/modules-load.d/modules.conf

  # Create the cleanjaro-release file
  echo "Cleanjaro Linux" > $pkgdir/etc/cleanjaro-release
	ln -s cleanjaro-release $pkgdir/etc/arch-release
	ln -s cleanjaro-release $pkgdir/etc/manjaro-release
  install "$srcdir"/home-local-bin.sh etc/profile.d/home-local-bin.sh
  install -m755 "$srcdir"/locale.sh etc/profile.d/locale.sh
  install -Dm644 "$srcdir"/os-release usr/lib/os-release

  # setup /var
  for d in cache local opt log/old lib/misc empty; do
    install -d -m755 var/$d
  done
  install -d -m1777 var/{tmp,spool/mail}

  # allow setgid games (gid 50) to write scores
  install -d -m775 -g 50 var/games
  ln -s spool/mail var/mail
  ln -s ../run var/run
  ln -s ../run/lock var/lock

  # setup /usr hierarchy
  for d in bin include lib share/misc src; do
    install -d -m755 usr/$d
  done
  for d in {1..8}; do
    install -d -m755 usr/share/man/man$d
  done

  # add lib symlinks
  ln -s usr/lib lib
  [[ $CARCH = 'x86_64' ]] && {
    ln -s usr/lib lib64
    ln -s lib usr/lib64
  }

  # add bin symlinks
  ln -s usr/bin bin
  ln -s usr/bin sbin
  ln -s bin usr/sbin

  # setup /usr/local hierarchy
  for d in bin etc games include lib man sbin share src; do
    install -d -m755 usr/local/$d
  done
  ln -s ../man usr/local/share/man

  # setup systemd-sysctl
  install -D -m644 "$srcdir"/sysctl usr/lib/sysctl.d/10-manjaro.conf

  # setup systemd-sysusers
  install -D -m644 "$srcdir"/sysusers usr/lib/sysusers.d/manjaro.conf

  # setup systemd-tmpfiles
  install -D -m644 "$srcdir"/tmpfiles usr/lib/tmpfiles.d/manjaro.conf

  # setup systemd.environment-generator
  install -D -m755 "$srcdir"/env-generator usr/lib/systemd/system-environment-generators/10-manjaro
}

# vim:set ts=2 sw=2 et:

