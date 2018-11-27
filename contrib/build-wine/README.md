Windows Binary Builds (PowerShell and WSL)
=====================

- install python in binaries folder to C:\python3.6.6
- install all other executables in binaries folder
- open PowerShell to electrum parent directory
- git clone https://github.com/ecdsa/pyinstaller.git
- cd pyinstaller
- python setup.py install
- cd to electrum
- git submodule update --init --recursive
- open WSL to electrum/contrib
- sudo apt-get install gettext
- ./make_locale
- mkdir -p ../gui/qt && cp deterministic-build/electrum-icons/icons_rc.py ../gui/qt/
- cd build-wine
- sudo apt-get install mingw-w64 autotools-dev autoconf libtool
- ./build-secp256k1.sh
- mkdir -p build && mv /tmp/electrum-build/secp256k1/libsecp256k1.dll build
- open PowerShell to electrum/contrib/build-wine
- pip install -r ..\requirements\requirements.txt
- pip install -r ..\requirements\requirements-binaries.txt
- pip install -r ..\requirements\requirements-hw.txt
- pip install -r ..\requirements\requirements-travis.txt
- pyinstaller.exe --noconfirm --ascii --name electrum-ray-$VERSION -w deterministic.spec
- & "C:/Program Files (x86)/NSIS/makensis.exe" /DPRODUCT_VERSION=$VERSION electrum.nsi
- generated binaries are in dist directory


Windows Binary Builds (Linux/Wine)
=====================

These scripts can be used for cross-compilation of Windows Electrum executables from Linux/Wine.

For reproducible builds, see the `docker` folder.


Usage:


1. Install the following dependencies:

 - dirmngr
 - gpg
 - 7Zip
 - Wine (>= v2)
 - (and, for building libsecp256k1)
   - mingw-w64
   - autotools-dev
   - autoconf
   - libtool


For example:

```
$ sudo apt-get install wine-development dirmngr gnupg2 p7zip-full
$ sudo apt-get install mingw-w64 autotools-dev autoconf libtool
```

The binaries are also built by Travis CI, so if you are having problems,
[that script](https://github.com/spesmilo/electrum/blob/master/.travis.yml) might help.

2. Make sure `/opt` is writable by the current user.
3. Run `build.sh`.
4. The generated binaries are in `./dist`.
