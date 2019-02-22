
# R-MKL: Introduction

Substantial improvements in `R` performance can be achieved by using fast, optimised Basic Linear Algebra Subprogram Libraries. In particular MKL seems to reach _70% improvement over the standard libblas+lapack version, and a 25% improvement over the ATLAS/OpenBLAS version":_

http://blog.nguyenvq.com/blog/2014/11/10/optimized-r-and-python-standard-blas-vs-atlas-vs-openblas-vs-mkl/

This brief guide explains how to compile R from source and link it to Intel MKL. It is based on the following technical article:

https://software.intel.com/en-us/articles/using-intel-mkl-with-r

# Howto

Download and install Intel Math Kernel Library:

```bash
[After registration]
$ wget http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/14895/l_mkl_2019.1.144.tgz

$ tar xvzf "l_mkl_2019.1.144.tgz"

$ sudo l_mkl_2019.1.144/install.sh

$ sudo ldconfig
```

Get R dependencies and download R source:

```bash
$ sudo apt-get build-dep r-base-core

  Depends: zip
  Depends: unzip
  Depends: libpaper-utils
  Depends: xdg-utils
  Depends: libbz2-1.0
  Depends: libc6
  Depends: libcairo2
  Depends: libcurl4
  Depends: libgfortran5
  Depends: libglib2.0-0
  Depends: libgomp1
  Depends: libice6
  Depends: libicu63
  Depends: libjpeg62-turbo
  Depends: liblzma5
  Depends: libpango-1.0-0
  Depends: libpangocairo-1.0-0
  Depends: libpcre3
  Depends: libpng16-16
  Depends: libquadmath0
  Depends: libreadline7
  Depends: libsm6
  Depends: libtcl8.6
  Depends: libtiff5
  Depends: libtk8.6
  Depends: libx11-6
  Depends: libxext6
  Depends: libxss1
  Depends: libxt6
  Depends: zlib1g
  Depends: ucf
  Depends: ca-certificates

$ wget https://cran.ma.imperial.ac.uk/src/base/R-3/R-3.5.2.tar.gz

$ tar xvzf R-3.5.2.tar.gz

$ cd R-3.5.2
```

Load MKL linking configurations:

```bash
$ source /opt/intel/mkl/bin/mklvars.sh intel64

$ MKL="-Wl,--no-as-needed -lmkl_gf_lp64 -Wl,--start-group -lmkl_gnu_thread  -lmkl_core  -Wl,--end-group -fopenmp  -ldl -lpthread -lm"

$ ./configure --with-blas="$MKL" --with-lapack

```

This is how the output of `./configure` should appear:

```
R is now configured for x86_64-pc-linux-gnu

  Source directory:          .
  Installation directory:    /usr/local

  C compiler:                gcc  -g -O2
  Fortran 77 compiler:       f95  -g -O2

  Default C++ compiler:      g++   -g -O2
  C++98 compiler:            g++ -std=gnu++98 -g -O2
  C++11 compiler:            g++ -std=gnu++11 -g -O2
  C++14 compiler:            g++ -std=gnu++14 -g -O2
  C++17 compiler:            g++ -std=gnu++17 -g -O2
  Fortran 90/95 compiler:    gfortran -g -O2
  Obj-C compiler:	      

  Interfaces supported:      X11
  External libraries:        readline, BLAS(MKL), LAPACK(in blas), curl
  Additional capabilities:   PNG, JPEG, TIFF, NLS, cairo, ICU
  Options enabled:           R profiling

  Capabilities skipped:      
  Options not enabled:       shared BLAS, memory profiling

  Recommended packages:      yes

```

Compile base R:

```bash
$ make
$ make docs
$ make pdf
```

Check whether the linking has been successful:

```bash
$ make check

$ ldd  ./bin/R

	linux-vdso.so.1 (0x00007fff9aec3000)
	libmkl_gf_lp64.so => /opt/intel/compilers_and_libraries_2019.1.144/linux/mkl/lib/intel64_lin/libmkl_gf_lp64.so (0x00007f9edc74f000)
	libmkl_gnu_thread.so => /opt/intel/compilers_and_libraries_2019.1.144/linux/mkl/lib/intel64_lin/libmkl_gnu_thread.so (0x00007f9edaefb000)
	libmkl_core.so => /opt/intel/compilers_and_libraries_2019.1.144/linux/mkl/lib/intel64_lin/libmkl_core.so (0x00007f9ed6d6f000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f9ed6d4d000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f9ed6d2c000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007f9ed6ba7000)
	libgfortran.so.5 => /lib/x86_64-linux-gnu/libgfortran.so.5 (0x00007f9ed693a000)
	libquadmath.so.0 => /lib/x86_64-linux-gnu/libquadmath.so.0 (0x00007f9ed68f9000)
	libreadline.so.7 => /lib/x86_64-linux-gnu/libreadline.so.7 (0x00007f9ed66ac000)
	libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007f9ed6638000)
	liblzma.so.5 => /lib/x86_64-linux-gnu/liblzma.so.5 (0x00007f9ed6412000)
	libbz2.so.1.0 => /lib/x86_64-linux-gnu/libbz2.so.1.0 (0x00007f9ed63fd000)
	libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007f9ed61df000)
	librt.so.1 => /lib/x86_64-linux-gnu/librt.so.1 (0x00007f9ed61d5000)
	libicuuc.so.63 => /lib/x86_64-linux-gnu/libicuuc.so.63 (0x00007f9ed6006000)
	libicui18n.so.63 => /lib/x86_64-linux-gnu/libicui18n.so.63 (0x00007f9ed5d2b000)
	libgomp.so.1 => /lib/x86_64-linux-gnu/libgomp.so.1 (0x00007f9ed5cfa000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f9ed5b37000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f9edd703000)
	libgcc_s.so.1 => /lib/x86_64-linux-gnu/libgcc_s.so.1 (0x00007f9ed5b1d000)
	libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007f9ed5aef000)
	libicudata.so.63 => /lib/x86_64-linux-gnu/libicudata.so.63 (0x00007f9ed40ff000)
	libstdc++.so.6 => /lib/x86_64-linux-gnu/libstdc++.so.6 (0x00007f9ed3f7c000)

```

Install:

```basj
$ make install
```

If `ldd` struggles to find the libraries and you don't want to export all the MKL paths globally, add alias to `.bashrc` to source MKL path:

```
alias R='source /opt/intel/mkl/bin/mklvars.sh intel64 && R'
```
