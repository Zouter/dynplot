
<!-- README.md is generated from README.Rmd. Please edit that file -->

<a href="https://travis-ci.org/dynverse/dynplot"><img src="https://travis-ci.org/dynverse/dynplot.svg" align="left"></a>
<a href="https://codecov.io/gh/dynverse/dynplot">
<img src="https://codecov.io/gh/dynverse/dynplot/branch/master/graph/badge.svg" align="left" /></a>
[**ℹ️ Tutorials**](https://dynverse.org)     [**ℹ️ Reference
documentation**](https://dynverse.org/reference/dynplot)
<br><img src="man/figures/logo.png" align="right" />

# Common visualisation tools for single-cell trajectories

## Installation

``` r
devtools::install_github("dynverse/dynplot")
```

On linux, udunits2 has to be installed:

  - Debian / Ubuntu / Linux mint: `sudo apt-get install libudunits2-dev`
  - Fedora / CentOS: `sudo dnf install udunits2-devel`

## Usage

The package provides different ways to plot both the topology and
cellular properties of a trajectory:

![](.readme_files/cells-1.png)<!-- -->

And to plot the expression and feature importances of many genes along
the trajectory

![](.readme_files/heatmap-1.png)<!-- -->

## Latest changes

Check out `news(package = "dynwrap")` or [NEWS.md](inst/NEWS.md) for a
full list of
changes.

<!-- This section gets automatically generated from inst/NEWS.md, and also generates inst/NEWS -->

### Recent changes in dynplot 1.0.2 (04-07-2019)

  - BUG FIX: Fix weird ceiling warning.

  - BUG FIX: Fix for new select waypoints

  - MINOR CHANGE: Added parameters `size_cells`, `alpha_cells` and
    `border_radius_percentage` to plotting functions that plot cells.

### Recent changes in dynplot 1.0.1 (07-05-2019)

  - BUG FIX: Remove dependency on shades, fixing rgb2hsv
bug

## Dynverse dependencies

<!-- Generated by "update_dependency_graphs.R" in the main dynverse repo -->

![](man/figures/dependencies.png)
