# Evo Pull Odds

**Live site:** https://johnlenya.github.io/evo-pull-odds/

A single-file, dependency-free calculator for the chance of pulling Evo skins from Blood Strike-style stashes. It reports exact probabilities for 1 pull, 10 pulls, 50 pulls, or any custom plan up to 300 pulls.

> Fan-made project. Not affiliated with or endorsed by NetEase. Stash odds vary by banner and region — the in-game odds panel is authoritative.

## What the site shows

- **Pull-plan comparison** — chance of at least one Evo for 1, 10, and 50 pulls, plus a custom pull count.
- **Hero result** — chance of at least one Evo for the selected plan, with rough odds and the active pity rule.
- **Secondary stats** — expected number of Evos, chance of 2+ Evos (useful for evolutions), average and median draws to the first Evo.
- **Cumulative chart** — chance of at least one Evo at every draw, with markers at 1, 10, and 50 and an auto-scaled y-axis.
- **Distribution chart** — chance of ending the plan with exactly *k* Evos.
- **Probability table** — exact percentages for exactly *k* and at least *k* Evos.

## Stash settings

| Setting | Default | Meaning |
| --- | --- | --- |
| Per-draw Evo chance | 1% | Community-reported top-tier weapon-skin rate. A striker preset is about 0.5%. |
| Hard pity | 100 draws | 100% chance on this draw, then the counter resets. |
| Soft pity start | 100 draws | Where an optional per-draw rate ramp begins. |
| Extra chance per draw | 0 pp | Percentage points added per draw after the soft-pity start. |
| Pity guarantee | On | Toggle off for stashes without a guaranteed featured reward. |

The quick-fill chips set 0.5%, 1%, 2%, or 4% per draw. All values are editable, so you can enter the exact odds shown in your stash's in-game probability panel.

### Defaults and assumptions

The defaults follow community-reported Blood Strike stash behavior:

- Top-tier weapon skin ≈ **1% per draw**; striker skin ≈ **0.5% per draw**.
- A **guarantee at 100 draws** for the featured reward in many stashes.
- **No soft-pity ramp** before the guarantee.
- Independent draws, a fresh pity counter at the start, and a reset whenever an Evo drops.
- Multi-pull discounts and non-Evo guarantees are not modeled.

If your stash's rules differ — for example, no guarantee, a different threshold, or a pity that resets on any legendary — adjust the fields or toggle pity off.

## How the math works

- **Flat rate / no pity:** exact binomial distribution,

  `P(k Evos in n pulls) = C(n, k) · p^k · (1 − p)^(n−k)`

- **With pity:** a dynamic program over two state dimensions, the number of consecutive misses (pity counter) and the number of Evos obtained. Each draw uses the effective probability for that pity counter, resets the counter on success, and is capped at 100% on the hard-pity draw.
- **Draws to first Evo:** the survival curve is deterministic conditional on no prior success, so the mean and median come directly from `P(T > t)`.

Everything is computed exactly in the browser — no Monte Carlo sampling.

## Run locally

No build step or server is required:

```bash
# Option 1: open the file directly
start index.html

# Option 2: serve it locally
python -m http.server 8000
# then visit http://localhost:8000/
```

Deployment is a static GitHub Pages site built from the `main` branch root.

## Project structure

```text
index.html   # markup, styles, probability engine, charts, and UI
README.md    # project and site details
```

## Sources

These third-party and community sources informed the stash-based defaults. They are not official NetEase publications.

- [Blood Strike 100-Draw Pity Guide: Gold Budget 2026 — Bittopup](https://news.bittopup.com/news/blood-strike-100-draw-pity-guide-gold-budget-2026)
- [Blood Strike Pivot vs Gold Stash: Complete 2026 Guide — Bittopup](https://news.bittopup.com/news/blood-strike-pivot-vs-gold-stash-complete-2026-guide)
- [Approximately how much gold do I need for ultra stash? — r/ProjectBloodstrikeBR](https://www.reddit.com/r/ProjectBloodstrikeBR/comments/1c48ym1/approximately_how_much_gold_do_i_need_for_ultra/)
- [Probabilities on Stash(s) — r/ProjectBloodstrikeBR](https://www.reddit.com/r/ProjectBloodstrikeBR/comments/1d5c7ox/probabilities_on_stashs/)
- [Blood Strike Lucky Draw Explained: Crates, Odds & Duplicates — Gamemarket](https://gamemarket.gg/news/blood-strike/blood-strike-lucky-draw-explained-crates-odds-duplicates)
- [Blood Strike Skin Rarity Tiers Explained — Gamemarket](https://gamemarket.gg/news/blood-strike/blood-strike-skin-rarity-tiers-explained-ultra-legendary-epic)

## Disclaimer

This is a fan-made statistical tool for entertainment and planning. It does not guarantee outcomes, and gacha spending carries financial risk. Verify all odds in the game's official probability disclosure before spending.
