# Session log — SalesForecastingWithTVAR-AR4 Spec alignment

## Turn 1

**User:** Make a new version of `SalesForecastingWithTVAR-AR4_PORT^v4.ipynb`. Update the hide cell about making this new version to align with `FoAI_Notation_Spec.md`. Report deviations from the notation spec one-by-one, each time giving a before/after plan; wait for confirmation before applying. Also write the session to `session_log.md` in the project folder.

**Assistant:**

Created `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` from v4. Updated the `#| hide` cell (editor cell 2): prepended `SalesForecastingWithTVAR-AR4_PORT^v4.ipynb` under `# BASED ON`, and added:

```
# v5 starts ------------------------------------------------
# Align math/code notation to FoAI_Notation_Spec.md (v1.23)
# Report Spec deviations one-by-one with before/after plans; apply only after confirmation
# Log the session in session_log.md
```

Started this `session_log.md`. Spec reference: uploaded `FoAI_Notation_Spec.md` v1.23 (4 Nov 2026).

### Deviation 1 — trajectory mark in code: `꞉` (U+A789) vs Spec `ː` (U+02D0)

**Spec (§6):** code mark for a trajectory is `ː`.

**Current (v5):** sequences use modifier-letter colon `꞉` = `Char(0xA789)` (documented in cell 10: previous triangular colon failed silently in GraphPPL). Affects identifiers such as `y꞉`, `_x꞉`, `_xˣ꞉`, and plot/data cells that use them.

| | Before | After (option A) | After (option B) |
|---|---|---|---|
| Code trajectory mark | `꞉` (U+A789) | Spec `ː` (U+02D0) everywhere | Keep `꞉`; document as intentional Spec exception in the hide cell |
| Cell 10 note | prints/verifies U+A789 | update to U+02D0 | keep; add “exception vs Spec §6” |

**Plan if confirmed:**
- **Option A:** rename all `꞉` → `ː` in v5 code/comments; update cell 10; note in hide cell under v5.
- **Option B:** no renames; add one hide-cell line that `꞉` (U+A789) is a documented exception to Spec §6 `ː` (GraphPPL/RxInfer).

Waiting for confirmation (A / B / other) before applying.

## Turn 2

**User:** option B for dev1

**Assistant:**

Applied Deviation 1 option B in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` hide cell (editor cell 2):

```
# Exception (Spec §6): keep trajectory mark ꞉ (U+A789) instead of ː (U+02D0); GraphPPL/RxInfer
```

No renames of `꞉` identifiers.

### Deviation 2 — continuous state typeface: `\mathbf{x}` vs `\boldsymbol{x}`

**Spec (§2, §3.9, D33):** continuous multivariate state is italic bold $\boldsymbol{x}$ / $\boldsymbol{x}^{(t)}$, never upright-bold tuple $\mathbf{x}$.

**Where:** generative-model markdown (cell 8); plot labels in cells 32, 34, 46 (incl. commented hidden-state labels).

| | Before | After |
|---|---|---|
| State in math / `L"..."` | `\mathbf{x}` | `\boldsymbol{x}` |
| Time index / asterisk | keep as-is for now (`_t`, `^*_t`) | deferred to later deviations |
| Other `\mathbf{…}` (`A`, `V`, `v_u`, `m`, `I`, `0`) | unchanged | separate (D40 / array face) |

**Plan if confirmed:** replace `\mathbf{x}` → `\boldsymbol{x}` only at those sites in v5; leave time subscripts and process-asterisk placement for later.

Waiting for confirmation before applying.

## Turn 3

**User:** do dev2

**Assistant:**

Applied Deviation 2 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb`: all `\mathbf{x}` → `\boldsymbol{x}` (cells **8, 32, 34, 46**). Left `\mathbf{A}`, `\mathbf{V}`, `\mathbf{v}_u`, `\mathbf{m}_{\theta}`, `\mathbf{I}`, `\mathbf{0}` unchanged. Time subscripts and process-asterisk placement unchanged.

### Deviation 3 — time index: subscript → Spec `^{(t)}`

**Spec (§3.1, §3.5 / rule N):** time is a member index in round-bracket superscripts: $\boldsymbol{x}^{(t)}$, $y^{(t)}$, $x^{(t-1)}$. Name subscripts stay (`\lambda_x`, `\lambda_y`).

**Where:** cell 3 prose; cell 8 model + bullet list; plot labels in cells 14, 32, 34, 46; forecast label `x^{\mathrm{fcast}}_t` in cell 46.

| | Before | After |
|---|---|---|
| State / obs | `\boldsymbol{x}_t`, `\boldsymbol{x}_{t-1}`, `y_t`, `x_t` | `\boldsymbol{x}^{(t)}`, `\boldsymbol{x}^{(t-1)}`, `y^{(t)}`, `x^{(t)}` |
| Components in bullets | `x_{t-1}`, `x_{t-M^{AR}}`, … | `x^{(t-1)}`, `x^{(t-M^{AR})}`, … |
| Forecast label | `x^{\mathrm{fcast}}_t` | `x^{(t)}_{\mathrm{fcast}}` (name as subscript) |
| Precisions | `\lambda_x`, `\lambda_y` | unchanged (name subscripts) |
| Process asterisk | `\boldsymbol{x}^*_t`, `\lambda_x^*` | deferred (D36) |
| Coeff plot `\\theta_{$(t),t}` | unchanged | later (entry + time) |

**Plan if confirmed:** apply the time-index replacements above in v5 only; leave asterisk placement and `\mathbf{A}`/sans-serif for later deviations.

Waiting for confirmation before applying.

## Turn 4

**User:** do dev3

**Assistant:**

Applied Deviation 3 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` (cells **3, 8, 14, 32, 34, 46**):

- `\boldsymbol{x}_t` / `\boldsymbol{x}_{t-1}` → `\boldsymbol{x}^{(t)}` / `\boldsymbol{x}^{(t-1)}`
- `x_t`, `y_t`, and lagged scalar components → `x^{(t)}`, `y^{(t)}`, `x^{(t-k)}`, …
- `x^{\mathrm{fcast}}_t` → `x^{(t)}_{\mathrm{fcast}}`
- Left unchanged: `\lambda_x` / `\lambda_y`; commented `\boldsymbol{x}^*_t` and `\lambda_x^*` (D36 next); `\theta_{$(t),t}` plots; engineering `\mathbf{A}` / `\mathbf{V}` / …

### Deviation 4 — process asterisk before indices (D36)

**Spec (§4 / D36):** process mark $^{\ast}$ always precedes any indices: $\boldsymbol{x}^{\ast(t)}$, not $\boldsymbol{x}^*_t$. Bare process symbols: $\lambda_x^{\ast}$ (name subscript stays; asterisk on the symbol).

**Where:** commented plot labels in cells 32, 46 (`\boldsymbol{x}^*_t`); commented real-precision labels in cells 29, 32 (`\lambda_x^*`).

| | Before | After |
|---|---|---|
| Process state | `\boldsymbol{x}^*_t` | `\boldsymbol{x}^{\ast(t)}` |
| Process precision | `\lambda_x^*` | `\lambda_x^{\ast}` |

**Plan if confirmed:** update those (mostly commented) labels in v5 only.

Waiting for confirmation before applying.

## Turn 5

**User:** do dev4

**Assistant:**

Applied Deviation 4 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` (cells **29, 32, 46**):

- `\boldsymbol{x}^*_t` → `\boldsymbol{x}^{\ast(t)}`
- `\lambda_x^*` → `\lambda_x^{\ast}`

(All still in commented labels.)

### Deviation 5 — engineering / LTI matrix face (D40 / D41)

**Spec (§5 / D40–D41):** classical / paper engineering matrices are **bold upright sans-serif** `\boldsymbol{\mathsf{…}}`, not upright-bold tuple `\mathbf{…}`. Code mark stays `_` (already used for `_θ`, etc.).

**Where:** generative-model markdown cell 8.

| | Before | After |
|---|---|---|
| Transition / cov / unit | `\mathbf{A}`, `\mathbf{V}`, `\mathbf{v}_u` | `\boldsymbol{\mathsf{A}}`, `\boldsymbol{\mathsf{V}}`, `\boldsymbol{\mathsf{v}}_u` |
| Prior mean / cov | `\mathbf{m}_{\theta}`, `\mathbf{V}_{\theta}` | `\boldsymbol{\mathsf{m}}_{\theta}`, `\boldsymbol{\mathsf{V}}_{\theta}` |
| Identity / zero block | `\mathbf{I}`, `\mathbf{0}` | `\boldsymbol{\mathsf{I}}`, `\boldsymbol{\mathsf{0}}` |

**Plan if confirmed:** replace those `\mathbf{…}` forms in cell 8 only (no code renames). Leave `\boldsymbol{\theta}` / `\boldsymbol{x}` as-is.

Waiting for confirmation before applying.

## Turn 6

**User:** do dev5

**Assistant:**

Applied Deviation 5 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` cell **8**:

- `\mathbf{A}`, `\mathbf{V}`, `\mathbf{v}_u`, `\mathbf{m}_{\theta}`, `\mathbf{V}_{\theta}`, `\mathbf{I}`, `\mathbf{0}`
  → `\boldsymbol{\mathsf{A}}`, `\boldsymbol{\mathsf{V}}`, `\boldsymbol{\mathsf{v}}_u`, `\boldsymbol{\mathsf{m}}_{\theta}`, `\boldsymbol{\mathsf{V}}_{\theta}`, `\boldsymbol{\mathsf{I}}`, `\boldsymbol{\mathsf{0}}`

No remaining `\mathbf{…}` in the notebook. Code identifiers unchanged (`_vᵤ`, etc.).

### Deviation 6 — observation mean: vector vs selected scalar

**Spec / consistency:** $y^{(t)}$ is scalar; the code mean is `dot(_vᵤ, _x꞉[t])`. Cell 8 currently writes $\mathcal{N}(y^{(t)};\ \boldsymbol{x}^{(t)}, \lambda_y^{-1})$, which puts a vector in the scalar mean slot.

| | Before | After |
|---|---|---|
| Likelihood | `p(y^{(t)} \mid \boldsymbol{x}^{(t)}) = \mathcal{N}(y^{(t)};\ \boldsymbol{x}^{(t)}, \lambda_y^{-1})` | `p(y^{(t)} \mid \boldsymbol{x}^{(t)}) = \mathcal{N}(y^{(t)};\ \boldsymbol{\mathsf{v}}_u^\top\boldsymbol{x}^{(t)}, \lambda_y^{-1})` |

**Plan if confirmed:** edit that one line in cell 8 (aligned display).

Waiting for confirmation before applying.

## Turn 7

**User:** give me the before and after latex in rendered form

**Assistant:**

Deviation 6 (observation mean) — rendered before/after. Still waiting for confirmation before applying.

**Before:**

$$p(y^{(t)} \mid \boldsymbol{x}^{(t)}) = \mathcal{N}(y^{(t)};\ \boldsymbol{x}^{(t)}, \lambda_y^{-1})$$

**After:**

$$p(y^{(t)} \mid \boldsymbol{x}^{(t)}) = \mathcal{N}(y^{(t)};\ \boldsymbol{\mathsf{v}}_u^\top\boldsymbol{x}^{(t)}, \lambda_y^{-1})$$

## Turn 8

**User:** do dev6

**Assistant:**

Applied Deviation 6 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` cell **8**:

$$p(y^{(t)} \mid \boldsymbol{x}^{(t)}) = \mathcal{N}(y^{(t)};\ \boldsymbol{\mathsf{v}}_u^\top\boldsymbol{x}^{(t)}, \lambda_y^{-1})$$

### Deviation 7 — multi-letter name `$M^{AR}$` → `$M^{\mathit{AR}}$`

**Spec (§5):** multi-letter names in math use `\mathit`.

**Where:** cell 8 bullets; cells 16, 22 prose; plot titles in cells 32, 34, 36, 46 (`L"$M^{AR}=$"`).

| | Before | After |
|---|---|---|
| Order symbol | `$M^{AR}$` | `$M^{\mathit{AR}}$` |

**Plan if confirmed:** replace all math/plot occurrences of `M^{AR}` with `M^{\mathit{AR}}` in v5 (not the Unicode code ident `Mᴬᴿ`).

Waiting for confirmation before applying.

## Turn 9

**User:** remember now ... before & after as RENDERED latex

**Assistant:**

Understood — for every deviation plan and applied change, show **before/after as rendered math** (markdown `$…$` / `$$…$$`), not raw TeX source strings. Will do that from here on.

### Deviation 7 — multi-letter name (re-stated, rendered)

**Spec (§5):** multi-letter names in math use `\mathit`.

**Before:** $M^{AR}$

**After:** $M^{\mathit{AR}}$

**Sites:** cell 8 bullets; prose cells 16, 22; plot titles in cells 32, 34, 36, 46. Code identifier `Mᴬᴿ` unchanged.

Waiting for confirmation before applying.

## Turn 10

**User:** do dev7

**Assistant:**

Applied Deviation 7 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` (cells **8, 16, 22, 32, 34, 36, 46**): $M^{AR}$ → $M^{\mathit{AR}}$. Code `Mᴬᴿ` unchanged.

### Deviation 8 — AR coefficient entries: subscripts → Spec square brackets (zero-based)

**Spec (§3.2, §1 / D35):** array entries use plain superscript square brackets; count $M^{\mathit{AR}}$ ⇒ last index $M^{\mathit{AR}}-1$.

**Before:**

$$\boldsymbol{\theta}=(\theta_1, \theta_2, \ldots, \theta_{M^{\mathit{AR}}})^{\top}$$

**After:**

$$\boldsymbol{\theta}=(\theta^{[0]}, \theta^{[1]}, \ldots, \theta^{[M^{\mathit{AR}}-1]})^{\top}$$

**Where:** cell 8 bullet. (Plot labels $\theta_{i,t}$ deferred.)

Waiting for confirmation before applying.

## Turn 11

**User:** do dev8

**Assistant:**

Applied Deviation 8 in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` cell **8**:

$$\boldsymbol{\theta}=(\theta^{[0]}, \theta^{[1]}, \ldots, \theta^{[M^{\mathit{AR}}-1]})^{\top}$$

### Deviation 9 — coefficient plot labels: $\theta_{i,t}$ → Spec time then entry

**Spec (§3.5 rule N, §3.6):** time first as a member; array entry in square brackets. Loop index `t` in the plot is the coefficient index (1-based today); Spec wants zero-based entry index.

**Before:**

$$\theta_{i,t}$$

(as built by `latexstring("\$ \\theta_{$(t),t} \$")` with loop `t in 1:…`)

**After:**

$$\theta^{(t)[i]}$$

with loop over zero-based entry `i` (and VI iteration as time $t$), e.g. label $\theta^{(t)[0]}$, $\theta^{(t)[1]}$, … — **or**, if the plot’s x-axis is VI iterations and each series is one coefficient (no per-time $\theta$), simpler Spec form:

$$\theta^{[i]}$$

**Clarification in plan:** Current plot x-axis is inference iterations; each ribbon is one coefficient. The `,t` in $\theta_{i,t}$ is misleading (not generative-process time). Proposed after: $\theta^{[i]}$ with `i` from `0` to `M^{\mathit{AR}}-1` (change loop `1:length` → `0:length-1` / `eachindex` carefully for Julia 1-based arrays: keep Julia index `k=1:…` but label with `k-1`).

| | Before | After |
|---|---|---|
| Label | $\theta_{k,t}$ (k from 1) | $\theta^{[k-1]}$ |
| Loop | `for t in 1:length(first(θms))` | same Julia range; label uses `t-1` |

Waiting for confirmation (and whether you prefer $\theta^{[i]}$ only vs $\theta^{(t)[i]}$ with $t$ = VI iteration).

## Turn 12

**User:** do dev9

**Assistant:**

Applied Deviation 9 (recommended) in `SalesForecastingWithTVAR-AR4_PORT^v5.ipynb` cell **36** (live + commented latexstring):

**Before:** $\theta_{k,t}$

**After:** $\theta^{[k-1]}$ via `latexstring("\$ \\theta^{[$(t-1)]} \$")` (Julia loop still 1-based).

### Deviation 10 — point estimate of learning $\boldsymbol{\theta}$

**Spec (§2 / D6):** learning parameter object is italic bold $\boldsymbol{\theta}$. A named point estimate should keep that face; the name is a subscript/superscript label.

**Before:**

$$\theta^{\mathrm{best}}$$

**After:**

$$\boldsymbol{\theta}^{\mathrm{best}}$$

**Where:** cell 37 markdown. (Code `_θᵇᵉˢᵗ` already has `_` array mark.)

Waiting for confirmation before applying.

## Turn 13

**User:** \theta^{(t)[i]}

**Assistant:**

Revised Deviation 9 in cell **36** to the user’s form (not bare $\theta^{[i]}$):

**Before:** $\theta_{k,t}$

**After:** $\theta^{(t)[i]}$ with Julia loop `i` (1-based) and label `$(i-1)`; live + commented series updated.

### Deviation 10 — point estimate of learning $\boldsymbol{\theta}$

**Spec (§2 / D6):** learning parameter object is italic bold $\boldsymbol{\theta}$.

**Before:**

$$\theta^{\mathrm{best}}$$

**After:**

$$\boldsymbol{\theta}^{\mathrm{best}}$$

**Where:** cell 37.

Waiting for confirmation before applying.

## Turn 14

**User:** do dev10

**Assistant:**

Applied Deviation 10 in cell **37**:

**Before:** $\theta$, $\theta^{\mathrm{best}}$

**After:** $\boldsymbol{\theta}$, $\boldsymbol{\theta}^{\mathrm{best}}$

### Deviation 11 — companion-matrix first row: spacing hack vs $\boldsymbol{\theta}$

**Spec (§2):** the coefficient vector is italic bold $\boldsymbol{\theta}$; no `\qquad` inside the symbol.

**Before:**

$$\boldsymbol{\mathsf{A}}(\boldsymbol{\theta}) = \begin{bmatrix} \boldsymbol{\qquad\theta}^{\top} \\ \boldsymbol{\mathsf{I}}_{M^{\mathit{AR}}-1} & \boldsymbol{\mathsf{0}} \end{bmatrix}$$

**After:**

$$\boldsymbol{\mathsf{A}}(\boldsymbol{\theta}) = \begin{bmatrix} \boldsymbol{\theta}^{\top} \\ \boldsymbol{\mathsf{I}}_{M^{\mathit{AR}}-1} & \boldsymbol{\mathsf{0}} \end{bmatrix}$$

**Where:** cell 8.

Waiting for confirmation before applying.

## Turn 15

**User:** do dev11

**Assistant:**

Applied Deviation 11 in cell **8**: $\boldsymbol{\qquad\theta}^{\top}$ → $\boldsymbol{\theta}^{\top}$ in $\boldsymbol{\mathsf{A}}(\boldsymbol{\theta})$.

### Deviation 12 — observation plot labels: $x^{(t)}=y^{(t)}$ vs $y^{(t)}$

**Spec:** observation is $y^{(t)}$; $x^{(t)}$ is the (leading) state component. Equating them in the legend conflates observation and state.

**Before:**

$$x^{(t)}=y^{(t)}$$

**After:**

$$y^{(t)}$$

**Where:** scatter labels in cells 32, 34, 46 (`Observation $x^{(t)}=y^{(t)}$` → `Observation $y^{(t)}$`).

Waiting for confirmation before applying.

## Turn 16

**User:** do dev12; also increase size of text and symbols on all legends but do not make bigger than size of heading. Also make plots taller

**Assistant:**

Applied Deviation 12 in cells **32, 34, 46**:

**Before:** $x^{(t)}=y^{(t)}$

**After:** $y^{(t)}$

Also (not a Spec deviation):

- Cell **5**: `default(titlefontsize=14, legendfontsize=11, guidefontsize=11, tickfontsize=10, linewidth=2, markersize=5, size=(800,560))` — legend text below title; thicker lines/markers for legend symbols; taller default figures.
- Cell **32** combined layout: `size=(900, 720)`.
- Cell **36**: `size=(800, 620)`.
- Hide cell notes for Dev12 + plot sizing.

### Deviation 13 — braced learning $\boldsymbol{\theta}$ in prose

**Spec (§2):** write `\boldsymbol{\theta}` with braces.

**Before:** $\boldsymbol\theta$

**After:** $\boldsymbol{\theta}$

**Where:** cell 3 intro.

Waiting for confirmation before applying.

## Turn 17

**User:** do dev13

**Assistant:**

Applied Deviation 13 in cell **3**:

**Before:** $\boldsymbol\theta$

**After:** $\boldsymbol{\theta}$

### Deviation 14 — leading state component index

**Spec (§3.9 / D33):** a component of continuous multivariate state is $x^{(t)[n]}$ (time round, component square). The “most recent sales” amount is the leading entry of $\boldsymbol{x}^{(t)}$.

**Before:**

$$x^{(t)}$$

(as “the $x^{(t)}$ state component”)

**After:**

$$x^{(t)[0]}$$

**Where:** cell 3 intro prose. (Companion-form bullets in cell 8 that write $\boldsymbol{x}^{(t)}=(x^{(t)},x^{(t-1)},\ldots)$ deferred unless you want those rewritten as $x^{(t)[0]},x^{(t)[1]},\ldots$.)

Waiting for confirmation before applying.

## Turn 18

**User:** do dev14

**Assistant:**

Applied Deviation 14 in cell **3**:

**Before:** $x^{(t)}$ (as “the state component”)

**After:** $x^{(t)[0]}$

### Deviation 15 — companion-state components as array entries

**Spec (§3.9 / D33):** components of $\boldsymbol{x}^{(t)}$ are $x^{(t)[n]}$, not a second use of bare $x^{(t)}$ beside the vector $\boldsymbol{x}^{(t)}$.

**Before:**

$$\boldsymbol{x}^{(t-1)} = (x^{(t-1)}, x^{(t-2)}, \ldots, x^{(t-M^{\mathit{AR}})})^{\top}$$

$$\boldsymbol{x}^{(t)} = (x^{(t)}, x^{(t-1)}, \ldots, x^{(t-M^{\mathit{AR}}+1)})^{\top}$$

**After:**

$$\boldsymbol{x}^{(t-1)} = (x^{(t-1)[0]}, x^{(t-1)[1]}, \ldots, x^{(t-1)[M^{\mathit{AR}}-1]})^{\top}$$

$$\boldsymbol{x}^{(t)} = (x^{(t)[0]}, x^{(t)[1]}, \ldots, x^{(t)[M^{\mathit{AR}}-1]})^{\top}$$

**Where:** cell 8 bullets. (Optional gloss: $x^{(t)[k]}$ is the lag-$k$ sales value.)

Waiting for confirmation before applying.

## Turn 19

**User:** do dev15

**Assistant:**

Applied Deviation 15 in cell **8**:

**Before:** $\boldsymbol{x}^{(t)}=(x^{(t)},x^{(t-1)},\ldots)$ (and lagged twin)

**After:** $\boldsymbol{x}^{(t)}=(x^{(t)[0]},x^{(t)[1]},\ldots,x^{(t)[M^{\mathit{AR}}-1]})^{\top}$ (and lagged twin)

### Deviation 16 — plot labels for the leading component

**Spec (§3.9):** plots use `first.(…)` of the state — that is $x^{(t)[0]}$, not the whole vector $\boldsymbol{x}^{(t)}$.

**Before:**

$$\boldsymbol{x}^{(t)},\qquad \boldsymbol{x}^{\ast(t)},\qquad x^{(t)}_{\mathrm{fcast}}$$

**After:**

$$x^{(t)[0]},\qquad x^{\ast(t)[0]},\qquad x^{(t)[0]}_{\mathrm{fcast}}$$

**Where:** cells 32, 34, 46 (incl. commented hidden-state labels).

Waiting for confirmation before applying.

## Turn 20

**User:** do dev16

**Assistant:**

Applied Deviation 16 in cells **32, 34, 46**:

**Before:** $\boldsymbol{x}^{(t)}$, $\boldsymbol{x}^{\ast(t)}$, $x^{(t)}_{\mathrm{fcast}}$

**After:** $x^{(t)[0]}$, $x^{\ast(t)[0]}$, $x^{(t)[0]}_{\mathrm{fcast}}$

### Deviation 17 — make the unit-vector selection explicit as $x^{(t)[0]}$

**Spec (§3.4 / §3.9):** $\boldsymbol{\mathsf{v}}_u$ selects the leading component; that entry is $x^{(t)[0]}$.

**Before:** (no identity stated; likelihood already uses $\boldsymbol{\mathsf{v}}_u^\top\boldsymbol{x}^{(t)}$)

**After:** add a bullet in cell 8:

$$\boldsymbol{\mathsf{v}}_u^\top\boldsymbol{x}^{(t)} = x^{(t)[0]}$$

Alternatively **skip** and treat the Spec math pass as complete (remaining items are API/`꞉` exceptions already noted).

Waiting for confirmation: **apply** / **skip & close Spec pass**.

## Turn 21

**User:** do dev17

**Assistant:**

Applied Deviation 17 in cell **8**: added

$$\boldsymbol{\mathsf{v}}_u^\top\boldsymbol{x}^{(t)} = x^{(t)[0]}$$

Hide cell notes Spec math pass through Dev17 (`꞉` exception kept).

No further Spec math deviations identified in the notebook. Remaining documented exception: trajectory mark `꞉` (U+A789) vs Spec `ː`.

## Turn 22

**User:** we also need a section near the beginning that defines the symbols

**Assistant:**

Proposed new **Working notation** markdown cell after the intro (after current cell 3, before `versioninfo`), Spec-aligned to the symbols used in this notebook. Waiting for confirmation before inserting.

## Turn 23

**User:** go

**Assistant:**

Inserted **Working notation** as new cell **4** (after intro cell 3; `versioninfo` shifted to cell 5). Hide cell updated. Spec-aligned symbol table + code-mark note (`꞉` exception).
