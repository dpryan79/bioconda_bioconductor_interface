# cytolib

This package is marked in `SystemRequirements` as needing `GNU make` and `C++11`. We ignore `C++11` regardless, but as this package only contains headers it has no actual requirements.

# GLAD

This package is almost never able to find its source code.

# HilbertVisGUI

`gtkmm 2.4` is not likely to be installable in docker containers in the foreseeable future. It's assumed that it will be present on systems already. Consequently, this package will likely always be skipped.

# hiReadsProcessor

Allegedly needs blat and a 2bit of hg18, which may be true for some example, but seems unlikely for normal use. Perhaps just blat?

# lpsymphony

For some reason, the package enforces using `-T` while linking, which breaks things on OSX with clang.

# Rbowtie2

This requires zlib, so should specify `zlib` in the `SystemRequirements`.

This does not pass CXXFLAGS through to the make file, so one needs to modify the CXX variable in Makevars:

    echo -e "CC=$CC
    FC=$FC
    CFLAGS=$CFLAGS
    CXXFLAGS=$CXXFLAGS
    CXX=$CXX -I${PREFIX}/include -L${PREFIX}/lib
    CXX98=$CXX
    CXX11=$CXX
    CXX14=$CXX" > ~/.R/Makevars

# Rhtslib

This requires bz2, lzma and zlib and should therefore have `bzip2`, `xz` and `zlib` or something like that in `SystemRequirements`. This does not respect the CFLAGS or LDFLAGS environment variables or when they're put in Makevars. The solution to that is to do the following before calling `R CMD INSTALL`:

    pushd src/htslib-1.7
    make CC="${CC}" CFLAGS="${CFLAGS}" LDFLAGS="-L${PREFIX}/lib"
    popd
