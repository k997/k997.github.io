---
title: "Archlinux  Build Bochs Debugger Gui"
date: 2022-09-24T12:29:20Z
draft: false
tags: ["archlinux","bochs","os"]

---


## 背景

archlinux 默认安装的 bochs 不带 debug gui，调试程序时候需要在 bochs 手动打印寄存器及其他信息，通过 bochs 的 debug gui 可以方便查看程序相关信息。

## 创建 PKGBUILD 文件

```bash
mkdir bochs-enhanced-debugger
touch PKGBUILD
```

参考[AUR (en) - bochs-gdb-stub (archlinux.org)](https://aur.archlinux.org/packages/bochs-gdb-stub)

```
pkgname=bochs-enhanced-debugger
pkgver=2.7
pkgrel=1
pkgdesc="A portable x86 PC emulation software package with enhanced debugger"
arch=('x86_64')
url="http://bochs.sourceforge.net/"
license=('LGPL')
depends=('gcc-libs' 'libxrandr' 'libxpm' 'gtk2')
replaces=("bochs-enhanced-debugger")
conflicts=("bochs-enhanced-debugger")
source=("http://downloads.sourceforge.net/sourceforge/bochs/bochs-$pkgver.tar.gz")
sha256sums=('a010ab1bfdc72ac5a08d2e2412cd471c0febd66af1d9349bc0d796879de5b17a')

prepare() {
    cd "$srcdir/bochs-$pkgver"
    # 4.X kernel is basically 3.20
    sed -i 's/2\.6\*|3\.\*)/2.6*|3.*|4.*)/' configure*
}

build() {
    cd "$srcdir/bochs-$pkgver"

    ./configure \
        --prefix=/usr \
        --without-wx \
        --with-x11 \
        --with-x \
        --with-term \
        --disable-docbook \
        --enable-cpu-level=6 \
        --enable-fpu \
        --enable-3dnow \
        --enable-disasm \
        --enable-long-phy-address \
        --enable-disasm \
        --enable-pcidev \
        --enable-usb \
        --enable-iodebug \
        --enable-smp \
        --enable-x86-64 \
        --enable-avx \
        --enable-evex \
        --enable-debugger \
        --enable-x86-debugger \
        --enable-debugger-gui
        #--with-sdl
        #--enable-all-optimizations
        #--enable-plugins

    sed -i 's/^LIBS = /LIBS = -lpthread/g' Makefile
    make -j $(nproc)
}

package() {
    cd "$srcdir/bochs-$pkgver"
    make DESTDIR="$pkgdir" install
    install -Dm644 .bochsrc "$pkgdir/etc/bochsrc-sample.txt"

    cd "$pkgdir/usr/bin/"
    mv bochs bochs-enhanced-debugger
    rm bximage
    cd "$pkgdir/usr/"
    rm -rfv share
    cd "$pkgdir"
    rm -rfv etc
}
```

## 打包安装 bochs debugger gui

```bash
makepkg

sudo pacman -U bochs-enhanced-debugger*.pkg.tar.zst
```

## bochs 配置文件

配置文件末尾添加 `display_library:  x, options="gui_debug"`

## 启动 bochs 

```
bochs -f bochs.bxrc
```

![image-20220924201953643](https://pic.kongwg.top/image-20220924201953643.png)