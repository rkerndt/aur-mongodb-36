# Maintainer: Rickie Kerndt <kerndtr@kerndt.com>

pkgname=mongodb
pkgver=3.6.10
pkgrel=1
pkgdesc='A high-performance, open source, schema-free document-oriented database'
arch=('x86_64')
url='https://www.mongodb.com/'
license=('SSPL')

depends=('boost-libs' 'pcre' 'libsasl' 'yaml-cpp' 'libstemmer' 'snappy')

# python module requirements from buildscripts/requirements.txt
# will assume these are all make depends and not needed for running
#
# Jira integration (do not include)
#	cryptography >=2.0.0
#	pyjwt == 1.5.3
# Other
#	pyaml == 3.11
#	unittest-xml-reporting == 2.1.0
# Linters
#	yapf == 0.16.0
#	mypy == 0.501 python_version > "3"
# Typing in Python2 for mypy
#	typing == 3.6.1; python_version < "3"
#	pylint == 1.6.5
#	pydocstyle == 1.1.1
# resmoke.py
#	mock == 2.0.0 python_version <"3" (included with unittest-xml-reporting)
#	pymongo >= 3.0
#	pypiwin32 == 219 sys_platform == "win32"
#	PyYAML == 3.11
#	requests >= 2.16.1
#	subprocess32 >=3.2.7 os_name == "posix" and python_version < "3"	
#	cheetah3 == 3.0.0; python_version < "3" (needs cheetah from python2)

makedepends=('scons' 'boost' 'asio' 'python2-pyaml' 'python2-unittest-xml-reporting' 'python2-yapf'
             'python2-typing' 'python2-pylint' 'python2-pydocstyle' 'python2-cheetah' )
checkdepends=( )
optdepends=( )
conflicts=('wiredtiger' 'mongodb<=3.4' 'mongodb>=4')
backup=( 'etc/mongodb.conf' )

source=( "http://downloads.mongodb.org/src/mongodb-src-r${pkgver}.tar.gz"
         "mongodb.sysusers"
	 "mongodb.tmpfiles"
         "mongodb.conf"
         "mongodb.service" )

sha256sums=('b5972e7cbee1753e991bef54370f37e71ba5cbd6cbe32730ed8682ca02ebc110'
            '8a3d0f31ecee9db1024df4de7be19492c0899eeace8cd13f66c09e58f8d65aa7'
            '0efc533f1e637382cd78dc21bfc2fe6ee3634432d3f8cb0ef18fa93056ddc5cb'
            'fed0c3984f3847b3ab074b24075fc621a3229f149160438f342f8f0e45f5d84d'
            '513f05e59d5fdd34c31fb0641e23a41c7b2cf3e5d29fb23dfbb4ae1004ee46f5')

_scons_targets="core"
_scons_args="--use-system-pcre
             --use-system-snappy
             --use-system-yaml
             --use-system-zlib
             --use-system-stemmer
             --use-system-asio
	     --use-system-boost
             --use-sasl-client
             --ssl
             --disable-warnings-as-errors
             --link-mode=dynamic-strict
	     --release
             --opt=on"

build() {
	cd mongodb-src-r$pkgver
	scons $_scons_targets $_scons_args
}

package() {
	cd mongodb-src-r$pkgver
	scons install $_scons_targets  --prefix="${pkgdir}/usr" $_scons_args
	install -Dm644 "$srcdir/mongodb.conf" "$pkgdir/etc/mongodb.conf"
	install -Dm644 "$srcdir/mongodb.service" "$pkgdir/usr/lib/systemd/system/mongodb.service"
	install -Dm644 "$srcdir/mongodb.sysusers" "$pkgdir/usr/lib/sysusers.d/mongodb.conf"
	install -Dm644 "$srcdir/mongodb.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/mongodb.conf"
	install -Dm644 LICENSE-Community.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

#	install_compass does not support the Arch distro so remove it
	rm -f ${pkgdir}/usr/bin/install_compass
}
