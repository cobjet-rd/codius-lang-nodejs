Evented I/O for V8 javascript (Port for Google Native Client).
===

### To build for Native Client (NaCl):

Prerequisites (Unix only):

    * GCC 4.2 or newer
    * G++ 4.2 or newer
    * Python 2.6 or 2.7
    * GNU Make 3.81 or newer
    * libexecinfo (FreeBSD and OpenBSD only)
    * [Native Client SDK](https://developer.chrome.com/native-client/sdk/download)

Unix/Macintosh:

```sh
export NACL_SDK_ROOT=<your-path>/nacl_sdk/pepper_<version>
source ./nacl-configure $NACL_SDK_ROOT
make

$NACL_SDK_ROOT/tools/sel_ldr_x86_32 -B $NACL_SDK_ROOT/tools/irt_core_x86_32.nexe -a \
-- $NACL_SDK_ROOT/toolchain/linux_x86_glibc/x86_64-nacl/lib32/runnable-ld.so \
--library-path $NACL_SDK_ROOT/toolchain/linux_x86_glibc/x86_64-nacl/lib32:\
$NACL_SDK_ROOT/toolchain/linux_x86_glibc/i686-nacl/usr/lib:\
$NACL_SDK_ROOT/lib/glibc_x86_32/Release ./out/Release/bin/node.nexe <your_nodejs_file>.js
```

Debug build:

```sh
make BUILDTYPE=Debug 

$NACL_SDK_ROOT/tools/sel_ldr_x86_32 -h 3:3 -B $NACL_SDK_ROOT/tools/irt_core_x86_32.nexe \
-a -- $NACL_SDK_ROOT/toolchain/linux_x86_glibc/x86_64-nacl/lib32/runnable-ld.so \
--library-path $NACL_SDK_ROOT/toolchain/linux_x86_glibc/x86_64-nacl/lib32:\
$NACL_SDK_ROOT/toolchain/linux_x86_glibc/i686-nacl/usr/lib:\
$NACL_SDK_ROOT/lib/glibc_x86_32/Debug ./out/Debug/node.nexe <your_nodejs_file>.js
```

GDB Debugging:

Change the "program" urls for runnable-ld.so based on your nacl_sdk directory 
relative to your node root.
```sh
$NACL_SDK_ROOT/tools/sel_ldr_x86_32 -g -h 3:3 -B $NACL_SDK_ROOT/tools/irt_core_x86_32.nexe \
-a -- $NACL_SDK_ROOT/toolchain/linux_x86_glibc/x86_64-nacl/lib32/runnable-ld.so \
--library-path $NACL_SDK_ROOT/toolchain/linux_x86_glibc/x86_64-nacl/lib32:\
$NACL_SDK_ROOT/toolchain/linux_x86_glibc/i686-nacl/usr/lib:\
$NACL_SDK_ROOT/lib/glibc_x86_32/Debug ./out/Debug/node.nexe <your_nodejs_file>.js

(new terminal)
$NACL_SDK_ROOT/toolchain/linux_x86_glibc/bin/x86_64-nacl-gdb

(gdb) nacl-manifest <your_manifest_file>.nmf
(gdb) target remote 127.0.0.1:4014
(gdb) remote get nexe test_node.nexe
(gdb) file test_node.nexe
(gdb) remote get irt test_irt.nexe 
(gdb) nacl-irt test_irt.nexe
```

## Issues
- libuv uses epoll which is not yet supported in nacl_io
  - Poll or select could be used as a fallback
  

### To build for non-Native Client:

Prerequisites (Unix only):

    * GCC 4.2 or newer
    * G++ 4.2 or newer
    * Python 2.6 or 2.7
    * GNU Make 3.81 or newer
    * libexecinfo (FreeBSD and OpenBSD only)

Unix/Macintosh:

```sh
./configure
make
make install
```

With libicu i18n support:

```sh
svn checkout --force --revision 214189 \
   http://src.chromium.org/svn/trunk/deps/third_party/icu46 \
   deps/v8/third_party/icu46
./configure --with-icu-path=deps/v8/third_party/icu46/icu.gyp
make
make install
```

If your python binary is in a non-standard location or has a
non-standard name, run the following instead:

```sh
export PYTHON=/path/to/python
$PYTHON ./configure
make
make install
```

Prerequisites (Windows only):

    * Python 2.6 or 2.7
    * Visual Studio 2010 or 2012

Windows:

    vcbuild nosign

You can download pre-built binaries for various operating systems from
[http://nodejs.org/download/](http://nodejs.org/download/).  The Windows
and OS X installers will prompt you for the location to install to.
The tarballs are self-contained; you can extract them to a local directory
with:

```sh
tar xzf /path/to/node-<version>-<platform>-<arch>.tar.gz
```

Or system-wide with:

```sh
cd /usr/local && tar --strip-components 1 -xzf \
                    /path/to/node-<version>-<platform>-<arch>.tar.gz
```

### To run the tests:

Unix/Macintosh:

```sh
make test
```

Windows:

```sh
vcbuild test
```

### To build the documentation:

```sh
make doc
```

### To read the documentation:

```sh
man doc/node.1
```

Resources for Newcomers
---
  - [The Wiki](https://github.com/joyent/node/wiki)
  - [nodejs.org](http://nodejs.org/)
  - [how to install node.js and npm (node package manager)](http://www.joyent.com/blog/installing-node-and-npm/)
  - [list of modules](https://github.com/joyent/node/wiki/modules)
  - [searching the npm registry](http://npmjs.org/)
  - [list of companies and projects using node](https://github.com/joyent/node/wiki/Projects,-Applications,-and-Companies-Using-Node)
  - [node.js mailing list](http://groups.google.com/group/nodejs)
  - irc chatroom, [#node.js on freenode.net](http://webchat.freenode.net?channels=node.js&uio=d4)
  - [community](https://github.com/joyent/node/wiki/Community)
  - [contributing](https://github.com/joyent/node/wiki/Contributing)
  - [big list of all the helpful wiki pages](https://github.com/joyent/node/wiki/_pages)
