# cytolib

This package is marked in `SystemRequirements` as needing `GNU make` and `C++11`. We ignore `C++11` regardless, but as this package only contains headers it has no actual requirements.

# GLAD

This package is almost never able to find its source code.

# HilbertVisGUI

`gtkmm 2.4` is not likely to be installable in docker containers in the foreseeable future. It's assumed that it will be present on systems already. Consequently, this package will likely always be skipped.

# Rhtslib

This requires lzma headers and should therefore have `xz` or something like that in `SystemRequirements`.
