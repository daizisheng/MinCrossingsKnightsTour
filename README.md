# MinCrossingsKnightsTour — with SequenceP_40 patch (fork)

> **Fork notice.** This is a fork of
> [nmamano/MinCrossingsKnightsTour](https://github.com/nmamano/MinCrossingsKnightsTour),
> the reference implementation of Besa, Johnson, Mamano, Osegueda, and Williams
> (henceforth **BJMOW**),
> *Taming the Knight's Tour: Minimizing Turns and Crossings*,
> [Theoretical Computer Science **902** (2022)](https://doi.org/10.1016/j.tcs.2021.12.002).
>
> This fork uses BJMOW's optimised $4\times8$ heel (28 crossings per
> slot, realising their claimed **$12n$** bound; see Section 4.2.2)
> as the baseline, and adds a single-element patch:
> *5 ×* (BJMOW heel) *→* one `SequenceP_40` in the heel band,
> reducing the asymptotic crossing constant from **12** to **11.5**.

## Live demo

**[daizisheng.github.io/MinCrossingsKnightsTour](https://daizisheng.github.io/MinCrossingsKnightsTour/index.html)**

Single canvas with a toggle: unchecked = BJMOW's optimised $12n$ tour;
checked = the same tour with the `SequenceP_40` patch.
Green rectangles mark each $4\times8$ BJMOW heel in the unpatched view;
red rectangles mark each $4\times40$ `SequenceP_40` in the patched view.
Every edge outside the marked rectangles &mdash; the bulk, side bands,
and corner templates &mdash; is bit-for-bit identical between the two
tours.

Try $n = 80$: BJMOW optimised = 976 crossings, patched = 956 (saving 20).
At $n = 1000$: BJMOW optimised = 12016, patched = 11536 (saving 480).
Asymptotically the saving is $\tfrac{1}{2}\,n + O(1)$ on top of the
$12n$ baseline, or $\tfrac{3}{2}\,n + O(1)$ relative to the default
$13n$ Sequence1 baseline.

## What this fork adds to `board.js`

Three additions against
[upstream `board.js`](https://github.com/nmamano/MinCrossingsKnightsTour/blob/master/board.js):

1. **`Sequence1` redefined to BJMOW's $12n$-optimised heel**
   (4 × 8 cell-pair codes; the upstream "default" Sequence1 is kept
   as `Sequence1Default` for reference).

2. **`SequenceP_40` template** (4 × 40 cell-pair-code matrix found
   by Google OR-Tools CP-SAT search, status `OPTIMAL`).

3. **`genTourPatched(width, height)`** — a copy of upstream
   `genTour` with two extra `while (j + 40 <= ...) { ...SequenceP40... }`
   loops inserted just before the existing 8-step heel-band loops.

4. **Single-canvas rendering scaffold** in `CanvasState`: a checkbox
   toggles between unpatched and patched modes, drawing the
   corresponding green/red template frames as overlays.

The unified `diff -u` against upstream is in
**[`board.js.patch`](board.js.patch)**.

## Why the patch is valid

`SequenceP_40` was found under the constraint that its
**cross-boundary edges** match those of *5 ×* (BJMOW heel). Three
additional finite combinatorial identities — boundary endpoint set,
internal-path endpoint pairing, no internal cycle — hold by direct
enumeration. Together these four identities make the substitution
*interface-preserving*: BJMOW's Hamilton-property proof
(their Theorem 2, an $S_3$ positional-matching argument) examines
each tile only through its boundary interface, and so applies
**verbatim** to the patched tour. The same `SequenceP_40` works as a
drop-in for both the default ($32$-crossing) Sequence1 and the
optimised ($28$-crossing) BJMOW heel — their cross-template interface
and path-pairing are identical.

## Original BJMOW description (from upstream)

The [interactive demo](https://nmamano.github.io/MinCrossingsKnightsTour/index.html)
of the knight's tour algorithm with a small number of turns and
crossings, by Juan Jose Besa Vidal, Timothy Johnson, Nil Mamano, and
Martha Osegueda. See the paper
[arXiv:1904.02824](https://arxiv.org/pdf/1904.02824.pdf).
The repo also contains some scripts used in the project (under
`Code/`).

## Licence

MIT (inherited from upstream).
