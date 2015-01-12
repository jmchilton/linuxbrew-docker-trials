Trying to build samtools with `sjackman/linuxbrew-standalone`. Dockerfile in attempt1/.

    % cd attempt1; sudo docker build --no-cache .
    Sending build context to Docker daemon 2.048 kB
    Sending build context to Docker daemon 
    Step 0 : FROM sjackman/linuxbrew-standalone
     ---> b64b9670f58e
    Step 1 : RUN brew tap homebrew/science
     ---> Running in 30b92e68e0e7
    Cloning into '/home/linuxbrew/.linuxbrew/Library/Taps/homebrew/homebrew-science'...
    Warning: Could not create link for homebrew/science/newick-utils, as it
    conflicts with Homebrew/homebrew/newick-utils. You will need to use the
    fully-qualified name when referring this formula, e.g.
      brew install homebrew/science/newick-utils
    Tapped 412 formulae
     ---> 225d3da343ae
    Removing intermediate container 30b92e68e0e7
    Step 2 : RUN brew install samtools
     ---> Running in 43f6d274a32a
    ==> Installing samtools from homebrew/homebrew-science
    ==> Installing samtools dependency: htslib
    ==> Downloading https://github.com/samtools/htslib/archive/1.1.tar.gz
    ==> Patching
    ==> echo '#define HTS_VERSION "1.1"' > version.h
    ==> make
    ==> make install prefix=/home/linuxbrew/.linuxbrew/Cellar/htslib/1.1
    /home/linuxbrew/.linuxbrew/Cellar/htslib/1.1: 31 files, 7.8M, built in 19 seconds
    ==> Installing samtools
    ==> Downloading https://github.com/samtools/samtools/archive/1.1.tar.gz
    ==> Patching
    ==> make HTSDIR=/home/linuxbrew/.linuxbrew/opt/htslib/include HTSLIB=/home/linuxbrew/.linuxbrew/opt/htslib/lib/libhts.a
    /home/linuxbrew/.linuxbrew/bin/ld: cannot find -lcurses
    collect2: error: ld returned 1 exit status
    Makefile:125: recipe for target 'samtools' failed
    make: *** [samtools] Error 1
    make: *** Waiting for unfinished jobs....
    
    READ THIS: https://github.com/Homebrew/linuxbrew/blob/master/share/doc/homebrew/Troubleshooting.md#troubleshooting
    If reporting this issue please do so at (not Homebrew/homebrew):
      https://github.com/homebrew/homebrew-science/issues
    
    INFO[0038] The command [/bin/sh -c brew install samtools] returned a non-zero code: 1 

Noticed `sjackman/linuxbrew-bottle` is newer, tried to base a newer variant of `linuxbrew-standalone` on that. In attempt2/

    % cd ../attempt2
    % sudo docker pull sjackman/linuxbrew-core
    Pulling repository sjackman/linuxbrew-core
    1f52a8b07c4d: Download complete 
    511136ea3c5a: Download complete 
    3b363fd9d7da: Download complete 
    607c5d1cca71: Download complete 
    f62feddc05dc: Download complete 
    8eaa4ff06b53: Download complete 
    416cfd2acfe5: Download complete 
    f513afd5edd0: Download complete 
    9ded40f0f868: Download complete 
    e2ffda256acb: Download complete 
    dbd22227b8f4: Download complete 
    fca42a944e05: Download complete 
    c730d5c4ee84: Download complete 
    d00178a12883: Download complete 
    3ef624661dbb: Download complete 
    Status: Image is up to date for sjackman/linuxbrew-core:latest
    % sudo docker build .
    Sending build context to Docker daemon  2.56 kB
    Sending build context to Docker daemon 
    Step 0 : FROM sjackman/linuxbrew-core
     ---> 1f52a8b07c4d
    Step 1 : MAINTAINER Shaun Jackman <sjackman@gmail.com>
     ---> Using cache
     ---> 8b05209a10e4
    Step 2 : USER root
     ---> Using cache
     ---> e81e0e926839
    Step 3 : RUN ln -s /usr/lib/x86_64-linux-gnu /usr/lib64
     ---> Using cache
     ---> 621a7d0f0a94
    Step 4 : USER linuxbrew
     ---> Using cache
     ---> 129842e6a193
    Step 5 : RUN mkdir $HOME/.linuxbrew/lib     && ln -s lib $HOME/.linuxbrew/lib64
     ---> Using cache
     ---> 3c0b265bbcf7
    Step 6 : ENV HOMEBREW_BUILD_BOTTLE 1
     ---> Using cache
     ---> 1ca711a587d2
    Step 7 : ENV HOMEBREW_DEVELOPER 1
     ---> Using cache
     ---> 7175d5e2b646
    Step 8 : RUN brew install glibc
     ---> Using cache
     ---> 8ceabf81cb1e
    Step 9 : RUN brew postinstall glibc
     ---> Using cache
     ---> 3a47cbc3f3b5
    Step 10 : RUN brew unlink glibc
     ---> Using cache
     ---> d7dbe58904c4
    Step 11 : RUN brew install https://raw.githubusercontent.com/Homebrew/homebrew-dupes/master/zlib.rb
     ---> Using cache
     ---> 78b116e49f35
    Step 12 : RUN brew reinstall binutils
     ---> Running in 5da40db91209
    ==> Reinstalling binutils
    Error: Invalid cross-device link - (/home/linuxbrew/.linuxbrew/Cellar/binutils/2.25, /home/linuxbrew/.linuxbrew/Cellar/    binutils/2.25.reinstall)
    INFO[0004] The command [/bin/sh -c brew reinstall binutils] returned a non-zero code: 1 

Odd that happens to me locally but not on Dockerhub - to demonstrate my ignorance I just tried to replace `brew reinstall` with separate `uninstall` and `install` (in attempt3/) - but then I get very cryptic errors when trying to build gcc dependencies.

    % sudo docker build .
    Sending build context to Docker daemon  2.56 kB
    Sending build context to Docker daemon 
    Step 0 : FROM sjackman/linuxbrew-core
     ---> 1f52a8b07c4d
    Step 1 : MAINTAINER Shaun Jackman <sjackman@gmail.com>
     ---> Using cache
     ---> 8b05209a10e4
    Step 2 : USER root
     ---> Using cache
     ---> e81e0e926839
    Step 3 : RUN ln -s /usr/lib/x86_64-linux-gnu /usr/lib64
     ---> Using cache
     ---> 621a7d0f0a94
    Step 4 : USER linuxbrew
     ---> Using cache
     ---> 129842e6a193
    Step 5 : RUN mkdir $HOME/.linuxbrew/lib     && ln -s lib $HOME/.linuxbrew/lib64
     ---> Using cache
     ---> 3c0b265bbcf7
    Step 6 : ENV HOMEBREW_BUILD_BOTTLE 1
     ---> Using cache
     ---> 1ca711a587d2
    Step 7 : ENV HOMEBREW_DEVELOPER 1
     ---> Using cache
     ---> 7175d5e2b646
    Step 8 : RUN brew install glibc
     ---> Using cache
     ---> 8ceabf81cb1e
    Step 9 : RUN brew postinstall glibc
     ---> Using cache
     ---> 3a47cbc3f3b5
    Step 10 : RUN brew unlink glibc
     ---> Using cache
     ---> d7dbe58904c4
    Step 11 : RUN brew install https://raw.githubusercontent.com/Homebrew/homebrew-dupes/master/zlib.rb
     ---> Using cache
     ---> 78b116e49f35
    Step 12 : RUN brew uninstall binutils
     ---> Using cache
     ---> 0aa18779dc2a
    Step 13 : RUN brew install binutils
     ---> Using cache
     ---> 1f02efeacd52
    Step 14 : RUN brew link glibc
     ---> Using cache
     ---> 6eac194c77a0
    Step 15 : RUN brew install patchelf
     ---> Using cache
     ---> 025f89e087c8
    Step 16 : RUN brew install gcc --with-glibc --only-dependencies
     ---> Running in 07e5b4f4dbe6
    ==> Installing dependencies for gcc: gmp, mpfr, libmpc, isl, cloog
    ==> Installing gcc dependency: gmp
    ==> Downloading https://downloads.sf.net/project/linuxbrew/Bottles/gmp-6.0.0a.x86_64_linux.bottle.tar.gz
    ==> Pouring gmp-6.0.0a.x86_64_linux.bottle.tar.gz
    /home/linuxbrew/.linuxbrew/Cellar/gmp/6.0.0a: 15 files, 3.6M
    ==> Installing gcc dependency: mpfr
    ==> Downloading https://downloads.sf.net/project/linuxbrew/Bottles/mpfr-3.1.2-p10.x86_64_linux.bottle.1.tar.gz
    ==> Pouring mpfr-3.1.2-p10.x86_64_linux.bottle.1.tar.gz
    /home/linuxbrew/.linuxbrew/Cellar/mpfr/3.1.2-p10: 24 files, 3.8M
    ==> Installing gcc dependency: libmpc
    ==> Downloading https://downloads.sf.net/project/linuxbrew/Bottles/libmpc-1.0.2.x86_64_linux.bottle.3.tar.gz
    ==> Pouring libmpc-1.0.2.x86_64_linux.bottle.3.tar.gz
    /home/linuxbrew/.linuxbrew/Cellar/libmpc/1.0.2: 10 files, 472K
    ==> Installing gcc dependency: isl
    ==> Downloading https://downloads.sf.net/project/linuxbrew/Bottles/isl-0.12.2.x86_64_linux.bottle.2.tar.gz
    ==> Pouring isl-0.12.2.x86_64_linux.bottle.2.tar.gz
    /home/linuxbrew/.linuxbrew/Cellar/isl/0.12.2: 55 files, 3.7M
    ==> Installing gcc dependency: cloog
    ==> Downloading https://downloads.sf.net/project/linuxbrew/Bottles/cloog-0.18.1.x86_64_linux.bottle.2.tar.gz
    ==> Pouring cloog-0.18.1.x86_64_linux.bottle.2.tar.gz
    /home/linuxbrew/.linuxbrew/Cellar/cloog/0.18.1: 33 files, 656K
     ---> 474b577e9ddc
    Removing intermediate container 07e5b4f4dbe6
    Step 17 : RUN brew install gcc --with-glibc
     ---> Running in 8909835830a2
    ==> Downloading http://ftpmirror.gnu.org/gcc/gcc-4.9.2/gcc-4.9.2.tar.bz2
    ==> Downloading https://gist.githubusercontent.com/sjackman/34fa1081982bda781862/raw/738349d49f4f094cced7cfe287cdcdfcd7207265/52fd2e1.diff
    ==> Patching
    patching file config-ml.in
    ==> ../configure --with-native-system-header-dir=/home/linuxbrew/.linuxbrew/include --with-build-time-tools=/home/linuxbrew/.linuxbrew/Cellar/binutils/2.25/x86_64-unknown-linux-gnu/bin --with-boot-ldflags=-static-libstdc++ -static-libgcc -L/home/linuxbrew/.linuxbrew/lib -Wl,--dynamic-linker=/home/linuxbrew/.linuxbrew/opt/glibc/lib/ld-linux-x86-64.so.2 -Wl,-rpath,/home/linuxbrew/.linuxbrew/lib --prefix=/home/linuxbrew/.linuxbrew/Cellar/gcc/4.9.2_1  --enable-languages=c,c++,objc,obj-c++,fortran --program-suffix=-4.9 --with-gmp=/home/linuxbrew/.linuxbrew/opt/gmp --with-mpfr=/home/linuxbrew/.linuxbrew/opt/mpfr --with-mpc=/home/linuxbrew/.linuxbrew/opt/libmpc --with-cloog=/home/linuxbrew/.linuxbrew/opt/cloog --with-isl=/home/linuxbrew/.linuxbrew/opt/isl --with-system-zlib --enable-libstdcxx-time=yes --enable-stage1-checking --enable-checking=release --enable-lto --disable-werror --with-pkgversion=Homebrew gcc 4.9.2_1 --with-glibc --with-bugurl=https://github.com/Homebrew/homebrew/issues --enable-plugin --disable-nls --disable-multilib
    ==> make bootstrap
    make[2]: *** [all-stage1-gcc] Error 2
    make[2]: Leaving directory `/tmp/gcc-BWfNl8/gcc-4.9.2/build'
    make[1]: *** [stage1-bubble] Error 2
    make[1]: Leaving directory `/tmp/gcc-BWfNl8/gcc-4.9.2/build'
    make: *** [bootstrap] Error 2
    
    READ THIS: https://github.com/Homebrew/linuxbrew/blob/master/share/doc/homebrew/Troubleshooting.md#troubleshooting
    
    These open issues may also help:
    gcc fails on not finding ISL (https://github.com/Homebrew/homebrew/issues/35691)
    Object files deleted during build of gcc needed by gdb (https://github.com/Homebrew/homebrew/issues/35734)
    gcc 4.9.2 fails to produce debugging information (https://github.com/Homebrew/homebrew/issues/34976)
    --cc=gcc-4.9 gets me in an infinite loop (https://github.com/Homebrew/homebrew/issues/33731)
    MacOS.(gcc|clang|llvm)_version can return nil (https://github.com/Homebrew/homebrew/issues/18781)
