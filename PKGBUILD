# Contributor: Kali Prasad <kprasadvnsi@protonmail.com>
# Maintainer: Furkan Kardame <f.kardame@manjaro.org>

pkgname=uboot-orangepi5
pkgver=2017.09
pkgrel=2
pkgdesc='U-Boot for Orange Pi 5'
arch=('aarch64')
url='https://github.com/orangepi-xunlong/u-boot-orangepi'
license=('GPL')
makedepends=('wget' 'python' 'dtc' 'git')
install=${pkgname}.install
_binsite="https://github.com/armbian/rkbin/raw"
_bincommit="1f2bd43aa12942849ec0f0e7ce26930865076bf3"
source=(
  "git+https://github.com/orangepi-xunlong/u-boot-orangepi.git#branch=v2017.09-rk3588"
  "src/rk3588_ddr.bin::$_binsite/$_bincommit/rk35/rk3588_ddr_lp4_2112MHz_lp5_2736MHz_v1.08.bin"
  "src/rk3588_bl31.elf::$_binsite/$_bincommit/rk35/rk3588_bl31_v1.28.elf"
)
md5sums=(SKIP SKIP SKIP)

build() {
  cd "${srcdir}/u-boot-orangepi"
  
  sed -i '1c#!/usr/bin/env python3' arch/arm/mach-rockchip/decode_bl31.py

  unset CFLAGS CXXFLAGS CPPFLAGS LDFLAGS

  make orangepi_5_defconfig
  export KCFLAGS='-Wno-error=address'
  make BL31="${srcdir}/rk3588_bl31.elf" spl/u-boot-spl.bin u-boot.dtb u-boot.itb

  tools/mkimage -n rk3588 -T rksd -d ../rk3588_ddr.bin:spl/u-boot-spl.bin idbloader.img
}

package() {
  cd "${srcdir}/u-boot-orangepi"
  mkdir -p "${pkgdir}/boot/extlinux"
  install -D -m 0644 idbloader.img u-boot.itb -t "${pkgdir}/boot"
}
