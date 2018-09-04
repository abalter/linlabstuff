I did come up with a reproducible installation using conda. It requires an older version of R, but everything else is more or less up to date, and DADA2 is at the latest.

## 1. Set the channels
First of all, you need to set the channels like this, which is different than the order bioconda recommends:

_.condarc_
```
channels:
  - conda-forge
  - bioconda
  - defaults
```

Note, that if you don't want this as your default `.condarc`, you can put it inside the environment folder as an override.

## 2. Create the environment and activate it
```
conda create -n dada2
conda activate dada2
```

At this point, you can put the above `.condarc` in `<path to conda>/envs/dada2`.

## 3. Install the packages:

    conda install r-knitr r-rmarkdown r-ggplot2 r-gridextra r-nlme r-data.table r-dplyr krb5 curl libssh2 zlib bioconductor-dada2 bioconductor-biocinstaller

## 4. Reinstall `krb5`
    conda install krb5   
There seems to be a glitch in the way conda relates to `krb5`. You can create a circular situation where updating the environment downgrades `krb5`, and then `conda install krb5` upgrades it. For DADA2 to load correctly, `krb5` needs to be in the upgraded state.

## 5. Usage:
### RStudio Desktop loaded from command line.
For this I activate the `dada2` environment and then start RStudio at the command line. Seems to work seamlessly.

### RStudio Server
I had to do a little trickery still for this to work:
1. At the command line activate the `dada2` environment and then `library(dada2)`. You get a message that `Rcpp` is loading, and then a warning that it was built under a different version of R. Then you can close R.
2. In RStudio Server set `.libPaths('<path to conda>/envs/dada2/lib/R/library')`. I tried putting this in my `.Rprofile`, but it did not seem to work.
3. Then you can `library(dada2)` without `Rcpp` loading or the warning message and proceed happily onward.

### RStudio Desktop not loaded from command line
I have actually not tested this. However, I would guess you would follow the same procedure as for RStudio Server.

I hope this is all helpful

I also hope down the road this becomes easier!
