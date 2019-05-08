This repository is intended to track issues related to building bioconductor packages in bioconda. Its primary audience is the bioconda and bioconductor core teams.

# Packages with broken source code links

 - LRBase.Ath.eg.db
 - LRBase.Bta.eg.db
 - LRBase.Cel.eg.db
 - LRBase.Dme.eg.db
 - LRBase.Dre.eg.db
 - LRBase.Gga.eg.db
 - LRBase.Hsa.eg.db
 - LRBase.Mmu.eg.db
 - LRBase.Pab.eg.db
 - LRBase.Ssc.eg.db
 - LRBase.Xtr.eg.db

# Packages marked broken

We (bioconda) are not currently checking for `PackageStatus: Deprecated` in VIEWS, but should do so. However, not all of these are marked as deprecated.

 - birte
 - dSimer
 - flipflop
 - gpuMagic
 - flowQ (deprecated)
 - ProCoNA (deprecated)
 - PGPC (deprecated)

# Per-package notes

This are contained in [PackageNotes.md](PackageNotes.md).

# System Requirements

A running list of system requirements in tabular form is in [SystemRequirements.txt](SystemRequirements.txt). The first column contains the text from the `SystemRequirements` field in the VIEWS files. Unfortunately this is unstructured at the moment and also not always correct (see the per-package notes). In the future this should be replaced by something machine parsable. Additional fields are (1) the associated conda package and (2) any notes from me.


# Skeleton changes

Often CFLAGS and LDFLAGS aren't honored by packages, even when put into Makevars. We need to assess (A) how wide-spread this is and (B) come up with a mitigation solution together with bioconductor/core.
