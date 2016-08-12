
# How to Install an Old Version of R/Bioconductor

Recently I upgraded R and Bioconductor and discovered that an old package no longer worked. Usually in this situation it is best to work with the developers to fix whatever is broken. However since I had already processed a large number of samples using the old package, I needed to revert to the previous version so that all the data would be processed in the same fashion. 

## Installing an older version of R alongside the current version

Installing older packages of R follows the typical make workflow used for other unix software. The steps are outlined below. The trick is to pass along the `--prefix` option with directory where you want this version to go, so that you don't overwrite your current version of R.

The source for older R packages can be found here:

<https://cran.r-project.org/src/base/>

In the example below, I am downgrading to R version 3.2.3 which is compatible with the Bioconductor package I was using.

	wget https://cran.r-project.org/src/base/R-3/R-3.2.3.tar.gz
	tar xcvf R-3.2.3.tar.gz
	cd R-3.2.3
	./configure --prefix=$HOME/R-3.2.3
	make
	make install

The older version of R can be run by typing:

	~/R-3.2.3/bin/R

Packages should be automatically installed in the R-3.2.3 folder.

	install.packages(c("dplyr", "tidyr", "ggplot2", "doSNOW", "parallel", "foreach"))


## Installing an old version of Bioconductor

The trick to making this work is to install an old version of the `BiocInstaller`. Basically this is where the `biocLite()` function lives, which is used to install Bioconductor packages. Any packages you install with an old copy of BiocInstaller will be fetched from the older repository. For example the Bioconductor package I wanted to revert to was released under Bioconductor 3.2. So I needed to downgrade from version 3.3 to 3.2. A list of old versions along with release dates and the version of R required can be found here:

<http://bioconductor.org/about/release-announcements/#release-versions>

	install.packages("BiocInstaller", repos="http://bioconductor.org/packages/3.2/bioc")
	library(BiocInstaller)
	biocLite()
	biocLite(c("name of package"))


