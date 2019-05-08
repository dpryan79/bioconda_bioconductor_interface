# cytolib

This package is marked in `SystemRequirements` as needing `GNU make` and `C++11`. We ignore `C++11` regardless, but as this package only contains headers it has no actual requirements.

# GLAD

This package is almost never able to find its source code.

# HilbertVisGUI

`gtkmm 2.4` is not likely to be installable in docker containers in the foreseeable future. It's assumed that it will be present on systems already. Consequently, this package will likely always be skipped.

# hiReadsProcessor

Allegedly needs blat and a 2bit of hg18, which may be true for some example, but seems unlikely for normal use. Perhaps just blat?

# Rhtslib

This requires bz2, lzma and zlib and should therefore have `bzip2`, `xz` and `zlib` or something like that in `SystemRequirements`. This does not respect the CFLAGS or LDFLAGS environment variables or when they're put in Makevars.

# Rbowtie2

This requires zlib, so should specify `zlib` in the `SystemRequirements`.
