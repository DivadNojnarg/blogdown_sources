+++
  title = "Save your R packages when updating R version"
  date = "2018-04-24"
+++

This post is **a copy and paste** of an [r-blogger article](https://www.r-bloggers.com/upgrade-r-without-losing-your-packages/).

However, it is **so useful** that I keep it here to remember about the process to
save and restore all R packages when updating to a newer version of R.

- Before you upgrade, build a temp file with all of your old packages.

{{< highlight r >}}
tmp <- installed.packages()
installedpkgs <- as.vector(tmp[is.na(tmp[,"Priority"]), 1])
save(installedpkgs, file="installed_old.rda")
{{< /highlight >}}

- Install the new version of R and let it do it’s thing.

- Once you’ve got the new version up and running, reload the saved packages and re-install them from CRAN.

{{< highlight r >}}
tmp <- installed.packages()
installedpkgs.new <- as.vector(tmp[is.na(tmp[,"Priority"]), 1])
missing <- setdiff(installedpkgs, installedpkgs.new)
install.packages(missing)
update.packages()
{{< /highlight >}}

Enjoy!