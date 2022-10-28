# GoogleTest

## 安装步骤linux

## build && install

```shell
git clone https://github.com/google/googletest.git

mkdir build 
cmake -DBUILD_SHARED_LIBS=ON .
make && make install
```

## check

```shell
sudo ldconfig -v | grep gtest
```

Before you start make sure your have read and understood [this note from Google](https://github.com/google/googletest/blob/36066cfecf79267bdf46ff82ca6c3b052f8f633c/googletest/docs/faq.md#why-is-it-not-recommended-to-install-a-pre-compiled-copy-of-google-test-for-example-into-usrlocal)! This tutorial makes using gtest easy, but may introduce [nasty bugs](https://stackoverflow.com/questions/9183540/linking-error-when-building-google-test-on-mac-commandline/20974890#20974890).

## 1. Get the googletest framework

```cpp
wget https://github.com/google/googletest/archive/release-1.8.0.tar.gz
```

Or get it by [hand](https://github.com/google/googletest/releases). I won't maintain this little How-to, so if you stumbled upon it and the links are outdated, feel free to edit it.

## 2. Unpack and build google test

```cpp
tar xf release-1.8.0.tar.gz
cd googletest-release-1.8.0
cmake -DBUILD_SHARED_LIBS=ON .
make
```

## 3. "Install" the headers and libs on your system.

This step might differ from distro to distro, so make sure you copy the headers and libs in the correct directory. I accomplished this by checking where [Debians former gtest libs](http://packages.debian.org/squeeze/amd64/libgtest-dev/filelist) were located. But I'm sure there are better ways to do this.

```cpp
sudo cp -a googletest/include/gtest /usr/include
sudo cp -a googlemock/gtest/libgtest_main.so googlemock/gtest/libgtest.so /usr/lib/

# The easiest/best way:
make install  # Note: before v1.11 this can be dangerous and is not supported
```

## 4. Update the cache of the linker

... and check if the GNU Linker knows the libs

```cpp
sudo ldconfig -v | grep gtest
```

If the output looks like this:

```cpp
libgtest.so.0 -> libgtest.so.0.0.0
libgtest_main.so.0 -> libgtest_main.so.0.0.0
```

then everything is fine.

gTestframework is now ready to use. Just don't forget to link your project against the library by setting `-lgtest` as linker flag and optionally, if you did not write your own test mainroutine, the explicit `-lgtest_main` flag.