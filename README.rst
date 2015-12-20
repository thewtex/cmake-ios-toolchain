cmake-ios-toolchain
===================
CMake Toolchain files for the iOS SDK
-------------------------------------

.. image:: https://travis-ci.org/thewtex/cmake-ios-toolchain.svg
    :target: https://travis-ci.org/thewtex/cmake-ios-toolchain

Example
-------

::

  cmake \
    -DCMAKE_TOOLCHAIN_FILE=../cmake-ios-toolchain/ios.simulator.toolchain.cmake \
      /path/to/project/source
  make

Dependencies
------------

- `CMake <http://cmake.org>`_

Status
-------

Supported
^^^^^^^^^

- Unix Makefiles and Ninja generators
- iOS Simulator Toolchain


Todo
^^^^

- `CMAKE_CROSSCOMPILING_EMULATOR` support (try_run and test execution)
- iOS Device Toolchains
