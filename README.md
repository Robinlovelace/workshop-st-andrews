# Origin destination datasets and spatial interaction models


``` r
if (!requireNamespace("pak", quietly = TRUE)) {
  install.packages("pak")
}
pkgs = c(
  "sf",
  "osmextract",
  "tidyverse",
#   "spanishoddata",
  "od",
  "simodels",
  "zonebuilder",
  "tmap"
)
```

Install the packages as follows:

``` r
pak::pak(pkgs, upgrade = FALSE)
```

We’ll load the packages as follows:

``` r
purrr::walk(pkgs, library, character.only = TRUE)
```

    Linking to GEOS 3.12.1, GDAL 3.8.4, PROJ 9.3.1; sf_use_s2() is TRUE

    Data (c) OpenStreetMap contributors, ODbL 1.0. https://www.openstreetmap.org/copyright.
    Check the package website, https://docs.ropensci.org/osmextract/, for more details.

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ✔ purrr     1.0.4     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

And let’s switch tmap to interactive plotting mode:

``` r
tmap_mode("view")
```

    ℹ tmap mode set to "view".

We’ll use the `osmextract` package to download OpenStreetMap data for St
Andrews. Set the `osmextract` download directory if it’s not already
set.

``` r
# ?oe_download_directory
# usethis::edit_r_environ()
Sys.getenv("OSMEXT_DOWNLOAD_DIRECTORY")
```

    [1] "/data/bronze/osm"

Let’s get a polygon representing St Andrews and its surroundings

``` r
# Radius of circles is 1, 3, 6, 10 km
st_andrews_zones = zb_zone("St Andrews", n_circles = 4)
names(st_andrews_zones)
```

    [1] "label"      "circle_id"  "segment_id" "geometry"   "centroid"  

``` r
st_andrews_zones = st_andrews_zones |>
  select(label, circle_id, segment_id)
plot(st_andrews_zones)
```

![](README_files/figure-commonmark/st-andrews-polygon-1.png)

Let’s check the zones are in the right place with an interactive plot:

``` r
tm_shape(st_andrews_zones) +
  tm_polygons("circle_id", alpha = 0.3)
```

![](images/clipboard-855988838.png)
