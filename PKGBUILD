# $Id: PKGBUILD 168060 2012-10-05 14:07:35Z bisson $
# Maintainer: Gaetan Bisson <bisson@archlinux.org>
# Contributor: Angel Velasquez <angvp@archlinux.org>
# Contributor: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Damir Perisa <damir.perisa@bluewin.ch>
# Contributor: Ben <ben@benmazer.net>

pkgname=mpd
pkgver=0.17.2
pkgrel=1
pkgdesc='Flexible, powerful, server-side application for playing music'
url='http://mpd.wikia.com/wiki/Music_Player_Daemon_Wiki'
license=('GPL')
arch=('arm')
depends=('libao' 'ffmpeg' 'libmodplug' 'audiofile' 'libshout' 'libmad' 'curl' 'faad2'
         'sqlite' 'jack' 'libmms' 'wavpack' 'avahi' 'libid3tag' 'yajl')
makedepends=('doxygen')
source=("http://downloads.sourceforge.net/musicpd/${pkgname}-${pkgver}.tar.bz2"
        'tmpfiles.d'
        'rc.d') 
sha1sums=('5e7ccf39f44e51240f181c2e1d9af5a7dafb1f02'
          'f4d5922abb69abb739542d8e93f4dfd748acdad7'
          '3470d489565f0ed479f1665dd2876f66acb5a585')

backup=('etc/mpd.conf')
install=install

build() {
	cd "${srcdir}/${pkgname}-${pkgver}"

	./configure \
		--prefix=/usr \
		--sysconfdir=/etc \
		--enable-jack \
		--enable-lastfm \
		--enable-soundcloud \
		--enable-pulse \
		--enable-shine \
		--disable-sidplay \
		--with-systemdsystemunitdir=/usr/lib/systemd/system

	make
}

package() {
	cd "${srcdir}/${pkgname}-${pkgver}"

	make DESTDIR="${pkgdir}" install

	sed \
		-e '/^#playlist_directory/c playlist_directory "/var/lib/mpd/playlists"' \
		-e '/^#db_file/c db_file "/var/lib/mpd/mpd.db"' \
		-e '/^#pid_file/c pid_file "/run/mpd/mpd.pid"' \
		-e '/^#state_file/c state_file "/var/lib/mpd/mpdstate"' \
		-e '/^#user/c user "mpd"' \
		-i doc/mpdconf.example

	install -Dm755 ../rc.d "${pkgdir}"/etc/rc.d/mpd
	install -d -g 45 -o 45 "${pkgdir}"/var/lib/mpd/playlists
	install -Dm644 doc/mpdconf.example "${pkgdir}"/etc/mpd.conf
	install -Dm644 ../tmpfiles.d "${pkgdir}"/usr/lib/tmpfiles.d/mpd.conf
}
