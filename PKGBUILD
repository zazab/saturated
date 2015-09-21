pkgname=saturated
pkgver="autogenerated"
pkgrel=1
pkgdesc="Dead simple makepkg builder server"
arch=('i686' 'x86_64')
license=('GPL')
makedepends=('go' 'git')

source=(
	"saturated::git://github.com/seletskiy/saturated"
	"saturated.service"
)

md5sums=(
	'SKIP'
     'adcc03f700810dadf2f3e30cf801694d'
)

backup=(
)

pkgver() {
	cd "$srcdir/$pkgname"
	local date=$(git log -1 --format="%cd" --date=short | sed s/-//g)
	local count=$(git rev-list --count HEAD)
	local commit=$(git rev-parse --short HEAD)
	echo "$date.${count}_$commit"
}

build() {
	cd "$srcdir/$pkgname"

	if [ -L "$srcdir/$pkgname" ]; then
		rm "$srcdir/$pkgname" -rf
		mv "$srcdir/.go/src/$pkgname/" "$srcdir/$pkgname"
	fi

	rm -rf "$srcdir/.go/src"

	mkdir -p "$srcdir/.go/src"

	export GOPATH="$srcdir/.go"

	mv "$srcdir/$pkgname" "$srcdir/.go/src/"

	cd "$srcdir/.go/src/$pkgname/"
	ln -sf "$srcdir/.go/src/$pkgname/" "$srcdir/$pkgname"

	echo "Running 'go get'..."
	go get
}

package() {
	install -DT "$srcdir/.go/bin/$pkgname" "$pkgdir/usr/bin/$pkgname"
	install -DT -m0755 "$srcdir/saturated.service" "$pkgdir/usr/lib/systemd/system/saturated.service"
}