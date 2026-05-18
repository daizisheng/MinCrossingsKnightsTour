# MinCrossingsKnightsTour — with Li 2026 patch (fork)

> **Fork notice.** This is a fork of
> [nmamano/MinCrossingsKnightsTour](https://github.com/nmamano/MinCrossingsKnightsTour),
> the reference implementation of Besa, Johnson, Mamano, Osegueda, and Williams
> (henceforth **BJMOW**),
> *Taming the Knight's Tour: Minimizing Turns and Crossings*,
> [Theoretical Computer Science **902** (2022)](https://doi.org/10.1016/j.tcs.2021.12.002).
>
> This fork adds a single-element patch:
> *5 ×* `Sequence1` *→* `SequenceP_40` in the heel band, reducing the
> asymptotic crossing constant from **12** to **11.5**.
> See [Li 2026](https://shisheng.li/f8a-ex161/arxiv/ex161.pdf).

## Live demo (side-by-side)

**[daizisheng.github.io/MinCrossingsKnightsTour](https://daizisheng.github.io/MinCrossingsKnightsTour/index.html)**

Two canvases: left = BJMOW's original Algorithm 1, right = the same
algorithm with our `SequenceP_40` patch.  Adjust width/height; the
two **crossing counts** are displayed below each canvas.  The red
rectangles on the right canvas mark the **only cells that differ
between the two tours** — every other edge is bit-for-bit identical
to BJMOW.

Try $n = 80$: BJMOW = 1040 crossings, patched = 980 crossings (saving 60).
At $n = 240$: BJMOW = 3120, patched = 2786 (saving 334).
Asymptotically the saving is $\frac{3}{2}\, n + O(1)$.

## What this fork adds to `board.js`

Three additions, no deletions, against
[upstream `board.js`](https://github.com/nmamano/MinCrossingsKnightsTour/blob/master/board.js):

1. **`SequenceP_40` template** (4 × 40 cell-pair-code matrix found
   by Google OR-Tools CP-SAT search, status `OPTIMAL`).  Marked
   `=== PATCH ADDITION (Li 2026) ===`.

2. **`genTourPatched(width, height)`** — a copy of upstream
   `genTour` with two extra `while (j + 40 <= ...) { ...SequenceP40... }`
   loops inserted just before the existing 8-step `Sequence1`
   heel-band loops.  Marked `LI 2026 PATCH (bottom band)` and
   `LI 2026 PATCH (top band, rotated)`.

3. **Side-by-side rendering scaffold** in `CanvasState`: each
   instance is parametrised by a tour-generating function (`genTour`
   or `genTourPatched`) and an optional list of red-bordered "patch
   regions" overlaid on top of the drawn tour.

Upstream's `genTour` is unchanged.

The unified `diff -u` against upstream is in
**[`board.js.patch`](board.js.patch)** (≈ 80 lines).

## Why the patch is valid

`SequenceP_40` was found under the constraint that its
**cross-boundary edges** match those of
*5 ×* `Sequence1*`.  Three additional finite combinatorial
identities — boundary endpoint set, internal-path endpoint pairing,
no internal cycle — hold by direct enumeration (proven by a single
deterministic script that runs in ~0.05 s; see
[`ex161-tools.tar.gz`](https://shisheng.li/f8a-ex161/arxiv/ex161-tools.tar.gz)).
Together these four identities make the substitution
*interface-preserving*: BJMOW's Hamilton-property proof
(their Theorem 2, an $S_3$ positional-matching argument) examines
each tile only through its boundary interface, and so applies
**verbatim** to the patched tour.

Full proof: [Li 2026](https://shisheng.li/f8a-ex161/arxiv/ex161.pdf)
(5 pages).

## Original BJMOW description (from upstream)

The [interactive demo](https://nmamano.github.io/MinCrossingsKnightsTour/index.html)
of the knight's tour algorithm with a small number of turns and
crossings, by Juan Jose Besa Vidal, Timothy Johnson, Nil Mamano, and
Martha Osegueda.  See the paper
[arXiv:1904.02824](https://arxiv.org/pdf/1904.02824.pdf).
The repo also contains some scripts used in the project (under
`Code/`).

## Licence

MIT (inherited from upstream).
