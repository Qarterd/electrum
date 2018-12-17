Windows Binary Builds (PowerShell and WSL)
=====================

- install python in binaries to its default directory
- install all other executables in binaries folder
- open PowerShell to electrum-ray parent directory:
	- git clone https://github.com/ecdsa/pyinstaller.git
	- cd pyinstaller
	- python setup.py install
	- cd ../electrum-ray
	- git submodule update --init --recursive
- open WSL to electrum-ray:
	- cd contrib
	- cp deterministic-build/electrum-icons/icons_rc.py ../electrum/gui/qt/
	- sudo apt-get install gettext
	- ./make_locale
	- cd build-wine
	- sudo apt-get install mingw-w64 autotools-dev autoconf libtool
	- ./build-secp256k1.sh
	- mkdir -p build && mv /tmp/electrum-build/secp256k1/libsecp256k1.dll build
	- find ../.. -exec touch -d '2000-11-11T11:11:11+00:00' {} +
- in PowerShell:
	- pip install -r contrib\requirements\requirements.txt
	- pip install -r contrib\requirements\requirements-binaries.txt
	- pip install -r contrib\requirements\requirements-hw.txt
	- pip install -r contrib\requirements\requirements-travis.txt
	- cd contrib/build-wine
	- pyinstaller.exe --clean --noconfirm --ascii --name electrum-ray-$VERSION -w deterministic.spec
- in WSL:
	- find dist -exec touch -d '2000-11-11T11:11:11+00:00' {} +
- in PowerShell:
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


Binaries Sources
=====================
- https://www.python.org/ftp/python/3.6.6/python-3.6.6-amd64.exe
- https://github.com/mhammond/pywin32/releases/download/b224/pywin32-224.win-amd64-py3.6.exe
- https://prdownloads.sourceforge.net/nsis/nsis-3.03-setup.exe?download
- https://prdownloads.sourceforge.net/project/libusb/libusb-1.0/libusb-1.0.22/libusb-1.0.22.7z?download
