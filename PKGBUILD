pkgname=node-gyp
pkgver=11.4.2
pkgrel=1
pkgdesc="Node.js native addon build tool"
arch=('x86_64')
url="https://github.com/nodejs/node-gyp"
license=('MIT')
depends=(
    'nodejs'
    'nodejs-nopt'
    'semver'
)
makedepends=(
    'git'
    'jq'
    'npm'
)
options=('!emptydirs')
source=(git+ssh://git@github.com/nodejs/node-gyp.git#tag=v${pkgver})
sha256sums=(5cf28d952ace6dcefb85536bfbcc2d38f10493b5d5455d5ed0a26e503a9aca1b)

build() {
    cd ${pkgname}

    npm install
}

package() {
    cd ${pkgname}

    local mod_dir=/usr/lib64/node_modules/${pkgname}

    install -vdm755 ${pkgdir}/{usr/bin,${mod_dir}}

    ln -s ${mod_dir}/bin/${pkgname}.js ${pkgdir}/usr/bin/${pkgname}

    npm prune --omit=dev

    mapfile -t mod_files < <(npm pack --dry-run --json | jq -r .[].files.[].path)

    cp --parents -a ${mod_files[@]} node_modules ${pkgdir}${mod_dir}

    # Experimental dedup
    rm -r ${pkgdir}${mod_dir}/node_modules/{,.bin/}{nopt,semver}
}
