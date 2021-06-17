dynplot: Plotting Single-Cell Trajectories
================

<!-- README.md is generated from README.Rmd. Please edit that file -->

<a href="https://github.com/dynverse/dynplot/actions?query=workflow%3AR-CMD-check"><img src="https://github.com/dynverse/dynplot/workflows/R-CMD-check/badge.svg" align="left"></a>
<a href="https://codecov.io/gh/dynverse/dynplot"><img src="https://codecov.io/gh/dynverse/dynplot/branch/master/graph/badge.svg" align="left" /></a>  
[**ℹ️ Tutorials**](https://dynverse.org)     [**ℹ️ Reference
documentation**](https://dynverse.org/reference/dynplot)
<br><img src="man/figures/logo.png" align="right" />

Visualise a single-cell trajectory as a graph or dendrogram, as a
dimensionality reduction or heatmap of the expression data, or a
comparison between two trajectories as a pairwise scatterplot or
dimensionality reduction projection.

Here’s a summary of the different plotting functions for visualising
single-cell trajectories.

``` r
library(tidyverse)
library(dyno)

# get trajectory
data(example_bifurcating)

trajectory <- example_bifurcating %>% add_root()

# gather some prior information
grouping <- trajectory$prior_information$groups_id

groups <- tibble(
  group_id = trajectory$milestone_ids,
  color = dynplot:::milestone_palette_list$auto(length(group_id))
)
features_oi <- apply(as.matrix(trajectory$counts), 2, sd) %>% sort() %>% names() %>% tail(10)
feature_oi <- features_oi[[10]]
```

## `plot_dendro()`: Plot a trajectory as a dendrogram

``` r
patchwork::wrap_plots(
  plot_dendro(trajectory) + labs(title = "Topology"),
  plot_dendro(trajectory, "milestone") + labs(title = "Ordering"),
  plot_dendro(trajectory, grouping=grouping, groups=groups) + labs(title = "Grouping/clustering"),
  plot_dendro(trajectory, feature_oi=feature_oi) + labs(title = "Expression of\na single gene"),
  plot_dendro(trajectory, "pseudotime") + labs(title = "Pseudotime"),
  byrow = TRUE,
  ncol = 3
) & theme(legend.position = "none")
```

<img src="man/figures/dendro-1.png" width="100%" />

## `plot_onedim()`: Plot a trajectory as a one-dimensional set of connected segments

``` r
patchwork::wrap_plots(
  plot_onedim(trajectory) + labs(title = "Topology"),
  plot_onedim(trajectory, "milestone") + labs(title = "Ordering"),
  plot_onedim(trajectory, grouping=grouping, groups=groups) + labs(title = "Grouping/clustering"),
  plot_onedim(trajectory, feature_oi=feature_oi) + labs(title = "Expression of\na single gene"),
  plot_onedim(trajectory, "pseudotime") + labs(title = "Pseudotime"),
  byrow = TRUE,
  ncol = 2
) & theme(legend.position = "none")
```

<img src="man/figures/onedim-1.png" width="100%" />

## `plot_graph()`: Plot a trajectory and cellular positions as a graph

``` r
patchwork::wrap_plots(
  plot_graph(trajectory) + labs(title = "Topology"),
  plot_graph(trajectory, "milestone") + labs(title = "Ordering"),
  plot_graph(trajectory, grouping=grouping, groups=groups) + labs(title = "Grouping/clustering"),
  plot_graph(trajectory, feature_oi=feature_oi) + labs(title = "Expression of\na single gene"),
  plot_graph(trajectory, "pseudotime") + labs(title = "Pseudotime"),
  byrow = TRUE,
  ncol = 3
) & theme(legend.position = "none")
```

<img src="man/figures/graph-1.png" width="100%" />

## `plot_dimred()`: Plot a trajectory in a (given) dimensionality reduction

``` r
patchwork::wrap_plots(
  plot_dimred(trajectory) + labs(title = "Topology"),
  plot_dimred(trajectory, "milestone") + labs(title = "Ordering"),
  plot_dimred(trajectory, grouping=grouping, groups=groups) + labs(title = "Grouping/clustering"),
  plot_dimred(trajectory, feature_oi=feature_oi) + labs(title = "Expression of\na single gene"),
  plot_dimred(trajectory, "pseudotime") + labs(title = "Pseudotime"),
  byrow = TRUE,
  ncol = 3
) & theme(legend.position = "none")
```

<img src="man/figures/dimred-1.png" width="100%" />

## `plot_heatmap()`: Plot expression data along a trajectory

In addition, you can also plot the expression of genes along the
trajectory as a heatmap.

``` r
plot_heatmap(trajectory, grouping = trajectory$prior_information$grouping_assignment)
```

<img src="man/figures/heatmap-1.png" width="100%" />

## `plot_linearised_comparison()`: Compare two trajectories as a pseudotime scatterplot

You can compare multiple trajectories (for the same cells) by creating a
scatterplot between the two trajectories.

``` r
prediction <- infer_trajectory(trajectory, ti_comp1())

trajectory$id <- "Bifurcating"
prediction$id <- "Linear"
plot_linearised_comparison(trajectory, prediction)
#> Warning: Unknown or uninitialised column: `directed`.

#> Warning: Unknown or uninitialised column: `directed`.

#> Warning: Unknown or uninitialised column: `directed`.
```

<img src="man/figures/scatterplot-1.png" width="100%" />

<!--

TODO: create dedicated function for this
## Alternative: Compare two trajectories by projecting dimensionality reductions

You can also use the colouring of the cells in order to compare two trajectories.

{r dimredcompare}
traj1 <- trajectory %>% dynwrap::add_dimred(dimred = dyndimred::dimred_landmark_mds)
traj2 <- prediction %>% dynwrap::add_dimred(dimred = dyndimred::dimred_landmark_mds, expression_source = trajectory)

plot_dimred(traj1, milestone_percentages = traj2$milestone_percentages, milestones = traj2$milestone_ids)
plot_dimred(
  traj2,
  expression_source = traj1,
  color_cells = "milestone",
  milestones = traj1$milestone_ids,
  milestone_percentages = traj1$milestone_percentages
)



-->

## Latest changes

Check out `news(package = "dynwrap")` or [NEWS.md](NEWS.md) for a full
list of changes.

<!-- This section gets automatically generated from inst/NEWS.md -->

### Recent changes in dynplot 1.1.0

Initial release on CRAN.

-   MINOR CHANGE: Add `arrow` parameter to all plot functions.

-   BUG FIX: Apply fixes for new versions of tibble, tidyr, and ggraph.

-   BUG FIX `optimise_order()`: Fix problem where GA::ga() wouldn’t run
    on milestone networks with 1 or 4 edges.

-   BUG FIX `linearise_cells()`: Fix ordering issue when
    `equal_cell_width` is `TRUE`.

-   MINOR CHANGE: Clean imports and supposed undefined variables.

-   MINOR CHANGE `plot_dendro()`: Allow plotting of disconnected graphs
    (\#32). This assumes that
    `dynwrap::add_root(traj, root_milestone_id = c(...))` has been
    called properly.

### Recent changes in dynplot 1.0.2 (04-07-2019)

-   BUG FIX: Fix weird ceiling warning.

-   BUG FIX: Fix for new select waypoints (\#41).

-   MINOR CHANGE: Added parameters `size_cells`, `alpha_cells` and
    `border_radius_percentage` to plotting functions that plot cells
    (\#40).
