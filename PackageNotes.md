# cytolib

This package is marked in `SystemRequirements` as needing `GNU make` and `C++11`. We ignore `C++11` regardless, but as this package only contains headers it has no actual requirements.

# CoGAPS

Like GLAD, this can't find its source code if you run `R CMD INSTALL` inside the source directory. So a simple `touch CoGAPS` before installing should fix things.

# GLAD

This package isn't able to find its source code, since it's looking for a file called `GLAD`. In bioconda it's also getting the wrong flags for gsl for some reason. The solution is to use the following in the build script:

    sed -i.bak 's/^ac_unique_file="GLAD"/ac_unique_file="NAMESPACE"/' configure
    $R CMD INSTALL --build --configure-vars='GSL_LIBS="-L$PREFIX/lib -lgsl -lgslcblas -lm"' .

# gmapR

This is being caught up by the change on OSX to .dylib as a shared library extension. 

# graph

This is currently the lone conda-specific issue. The recipe renames a dynamic library and that seems to confuse conda. The fix is the following patch:

    diff --git a/src/Makevars b/src/Makevars
    index 5ddfd10..77759f9 100644
    --- src/Makevars
    +++ src/Makevars
    @@ -1,6 +1,6 @@
     after: $(SHLIB)
    -       mv $(SHLIB) BioC_graph$(SHLIB_EXT)
    -
    +       cp $(SHLIB) BioC_graph$(SHLIB_EXT)
    +#
     # By default, 'R CMD build' won't remove that file so it will end up in the
     # source tarball (observed with R 2.12.0).
     clean:

This is likely an obscure conda bug, so there's no issue that needs reporting upstream.

# HilbertVisGUI

`gtkmm 2.4` is not likely to be installable in docker containers in the foreseeable future. It's assumed that it will be present on systems already. Consequently, this package will likely always be skipped.

# hiReadsProcessor

Allegedly needs blat and a 2bit of hg18, which may be true for some example, but seems unlikely for normal use. Perhaps just blat?

# lpsymphony

For some reason, the package enforces using `-TP` while linking, which breaks things on OSX with clang. Adding the following to build.sh fixes that:

    sed -i.bak "s/-TP//" src/SYMPHONY/SYMPHONY/configure.ac
    sed -i.bak "s/-TP//" src/SYMPHONY/SYMPHONY/configure

# PureCN

There are a number of R scripts that need to have the R path manually set. This can be done in `build.sh` as follows:

    extdata=$PREFIX/lib/R/library/PureCN/extdata
    mkdir -p $PREFIX/bin

    for f in Coverage.R Dx.R FilterCallableLoci.R IntervalFile.R NormalDB.R
    do
      perl -pi -e 'print "#!/opt/anaconda1anaconda2anaconda3/bin/Rscript\n" if $. == 1' $extdata/$f
      ln -s $extdata/$S $PREFIX/bin/PureCN_${f}
    done

    perl -pi -e 'print "#!/opt/anaconda1anaconda2anaconda3/bin/Rscript\n" if $. == 1' $extdata/PureCN.R
    ln -s $extdata/PureCN.R $PREFIX/bin/PureCN.R

This could likely be done in `meta.yaml` instead.

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

# tofsims

It's not listed as a system requirement, but this requirs liblapack, which comes from (open)blas.
