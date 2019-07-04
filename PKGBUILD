# Maintainer: Timon Engelke <aur@timonengelke.de>
pkgdesc="ROS - Timing utilities are very dependent on the system api provided for their use."
url='https://wiki.ros.org/ecl_time'

pkgname='ros-melodic-ecl-time'
pkgver='0.62.1'
_pkgver_patch=0
arch=('any')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-melodic-ecl-license
  ros-melodic-ecl-errors
  ros-melodic-ecl-config
  ros-melodic-ecl-time-lite
  ros-melodic-ecl-build
  ros-melodic-ecl-exceptions
  ros-melodic-catkin)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]})

ros_depends=(ros-melodic-ecl-license
  ros-melodic-ecl-errors
  ros-melodic-ecl-config
  ros-melodic-ecl-time-lite
  ros-melodic-ecl-build
  ros-melodic-ecl-exceptions)
depends=(${ros_depends[@]})

# Git version (e.g. for debugging)
# _tag=release/melodic/ecl_time/${pkgver}-${_pkgver_patch}
# _dir=${pkgname}
# source=("${_dir}"::"git+https://github.com/yujinrobot-release/ecl_core-release.git"#tag=${_tag})
# sha256sums=('SKIP')

# Tarball version (faster download)
_dir="ecl_core-release-release-melodic-ecl_time-${pkgver}-${_pkgver_patch}"
source=("${pkgname}-${pkgver}-${_pkgver_patch}.tar.gz"::"https://github.com/yujinrobot-release/ecl_core-release/archive/release/melodic/ecl_time/${pkgver}-${_pkgver_patch}.tar.gz")
sha256sums=('8c9fc10c3712cfbab03a994946b620d689f1fab6f22a69811a51052ca03b9e4d')

build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/melodic/setup.bash ] && source /opt/ros/melodic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  /usr/share/ros-build-tools/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/melodic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DPYTHON_INCLUDE_DIR=/usr/include/python3.7m \
        -DPYTHON_LIBRARY=/usr/lib/libpython3.7m.so \
        -DPYTHON_BASENAME=-python3.7m \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
