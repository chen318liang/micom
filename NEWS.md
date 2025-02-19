# News and release notes for MICOM

This includes a list of major changes for each minor version starting from 0.19.0.

For information on how to use `micom` please refer to
[the documentation](https://micom-dev.github.io/micom).

### 0.32.0

Helper functions to save results were added.

### 0.31.7

Fixed a bug where minimal_medium would maximize with OSQP instead of minimize.

### 0.31.6

More adjustments to OSQP.

### 0.31.5

Fix a bug in the stabilization term used for OSQP. Try to avoid fragmentation warning.

### 0.31.4

Fix regression in T-SNE.

### 0.31.2 - 0.31.3

Some more fixes to the media completion.

### 0.31.1

Show added flux as fraction of total in `complete_db_medium`.

### 0.31.0

The medium completion functions now have a flag that allows adding additional flux
to components already in the candidate medium. The default is now changed to that
new non-strict mode which makes it much easier and more efficient to complete media.
In that case the `max_added_import` amount now specifies the maximum flux added on top
the limit specified in the candidate medium.

### 0.30.5

Fixed a bug where T-SNE would complain in low sample numbers.

### 0.30.3 - 0.30.4

Those were fixes to deployment and dependencies only.

### 0.30.2

Avoids a bug in COBRAPY that can yield to huge serialized versions of the models.

### 0.30.1

Fix compatibility with Python 3.7.

### 0.30.0

`build` will now detect existing models in the output folder and skip them. This now
allows to resume interrupted builds. Explicit rebuild requires you to delete the
folder now.

### 0.29.6

Fixes a bug where using more than 1 thread for workflows would not return results
in some cases.

### 0.29.5

Fixed some dependency errors on older Python versions.

### 0.29.4

Port tests to cobrapy 0.25 and above. Fix an import error in workflows.core.

### 0.29.3

Suppresses some warning in annotations from cobrapy.

### 0.29.2

Fixes some CI and build issues.

### 0.29.0

Renames the `n_jobs` arguments to `threads` across the code base for consistency and
updates the arguments for `workflow`.

`workflow` now does not spawn a process for single-threaded runs, reducing overhead.

Fixes an issue where `workflow` would deadlock with pytest-cov.

### 0.28.1

Fixes an issue where an accumulation in warnings in spawned processes trips up
jupyter.

### 0.28.0

Fixes a deployment issue with the previous build.

### 0.27.0

Annotations now include the molecular weight, number of carbon atoms, and the number of
nitrogen atoms for each compound.

### 0.26.0

Now adjusts active demands to be optional. This was an issue with some models
in AGORA that had forced biotin demands. This effectively prohibits zero growth
rates and is problematic for MICOM.

Fix the logger for multiprocessing.

### 0.25.1

Handle CARVEME models better in `workflows.complete_medium`.

Workflows now have a deterministic return error (same order as inputs).

### 0.25.0

`build_database` now allows to set the compression algorithm and level.

### 0.24.1

Fixed settings to get better convergence behavior with OSQP.

### 0.24.0

MICOM now works with OSQP and works out of the box without a commercial QP solver. See
the [installation docs]() for more information.

### 0.23.1

The sample heatmap in `plot_exchanges_per_sample` is now automatically rotated when
there are mores samples than reactions.

media workflows will use presolving by default since those are often numerically
problematic.

### 0.23.0

Fix the signature and add deprecation warnings for optimize_* methods.

`plot_exchanges_per_taxon` will now use normalized fluxes (per 1 gDW for each taxon)
which allows better comparability. The old behavior can be enabled with
`use_total_flux=True`.

Avoid negative growth rate constraints in `_apply_min_growth`.

Can now enable presolving/scaling in `grow` and `tradeoff`.

### 0.22.7

Fix some warnings from pandas.

Avoid a crash in `reularize_l2_norm` when all reactions for a taxon have been fixed to
zero.

Raise a better error if the minimal medium optimization fails in grow.

Use the right tolerance when setting atol and rtol automatically.

### 0.22.6

Fixed an error in `micom.workflows.build` if build was run without a model database
but with a `file` column in the taxonomy.

### 0.22.5

Fixed missing data files.

### 0.22.4

Improves version compatibility with cobrapy and QIIME 2.

### 0.22.3

Fixed a bug where incorrectly labeled biomass demand reactions were treated like an
exchange.

### 0.22.2

Lowered the required version for pandas to re-enable compatibility with Python 3.6.

Docs are now built on all pushes.

### 0.22.1

`atol` and `rtol` are now consistently exposed.

Remove six dependency.

### 0.22.0

Got a bit smarter in cleaning up compartment suffixes. This fixes the odd "_e_m" suffix
in CARVEME-derived models. This will change the names of exchange reactions compared
to version 0.21.x.

### 0.21.0

Stabilize minimal_medium a bit more.

The strategy to calculate fluxes in the grow workflow can now be set with the
`strategy` argument. For instance, you can now get fluxes via parsimonious FBA instead of
assuming minimal imports.

Fixed a bug where minimal media classification with `weights="mass"` would fail due to
invalid elements in the formula (for instance "X").

### 0.20.0

This version brings new functionality to design growth media from a skeleton medium.
This also allows for a quicker verification of media against the model databases.

Added workflows:

- `check_db_media` check if models in a model database can grow on a given medium
- `complete_db_media`completes a growth medium so all models in the db can grow on it

Together with this we now provide several new revised growth media for the gut on
github.com/micom-dev/media. In particular, we finally provide the often requested growth
medium for the carveME database.

### 0.19.0

`minimal_medium` now accepts weighting the fluxes bei molecular weight or
any elemental content. For instance, you can now calculate a minimal medium that
minimizes carbon import for instance. The used `weigths` argument propagates
to any workflow using this function including `complete_medium`, `fix_medium`
and `grow`.

### 0.1.0 - 0.18.0

`minimal_medium` now has an option to return all fluxes along with the
import and export fluxes. Useful if you want to check what every individual
taxa consumes.

Addition of `Community.cooperative_tradeoff` which brings a fast method to
get nearly unique growth rates for a particular growth tradeoff (for instance
50% maximal growth rate).

Addition of elasticity coefficients which allows you to study which import
and export reactions are sensitive to changes in diet or taxa abundances.

Several changes to make `micom` capable of handling numerical instable models.
This includes a crossover strategy to obtain an optimal or near-optimal
from an incomplete interior point optimization.
