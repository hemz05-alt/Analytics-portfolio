# Geopolitical Shocks and Oil Market Dynamics
### Lead-Lag Analysis Between Intraday Volatility and Trading Volume (2000–2026)

---

## Overview

This project investigates whether a consistent lead-lag relationship exists between intraday price volatility and trading volume during geopolitical oil shocks, and whether that relationship differs across conflicts with varying degrees of public anticipation.

The analysis was motivated by the 2026 Iran-US tensions and the closure of the Strait of Hormuz, which prompted a broader question: do oil markets react to geopolitical shocks sequentially (one metric moving before the other) or simultaneously (both moving together)?

---

## Research Question

> *During major geopolitical oil shocks, which moves first , intraday price volatility or trading volume , and does the lead-lag relationship differ based on how publicly anticipated the conflict was?*

This question is falsifiable: if a consistent lead-lag pattern exists, one metric should reliably predict the other the following day across multiple conflicts. If markets process shocks simultaneously, neither metric should consistently precede the other.

---

## Dataset

- **Source:** Crude oil OHLCV data from Kaggle
- **Coverage:** August 2000 , March 2026
- **Columns:** Date, Open, High, Low, Close, Volume
- **Completeness:** All years from 2000 to 2026 present, no gaps

---

## Methodology

### Volatility Measure
Intraday volatility is defined as the daily High-Low range:
```
Volatility = High - Low
```
This captures the full range of price movement within each trading session.

### Normalisation Approach
Raw volume and volatility numbers are not directly comparable across different time periods , average trading volumes in 2001 differ significantly from 2022. Each metric is normalised against a **fixed pre-event baseline** calculated from the 30 trading days immediately before each conflict began.

A rolling baseline was considered but rejected: once a conflict exceeds 30 days, conflict-period data begins contaminating the rolling mean, understating how abnormal later days actually are. The fixed pre-event approach eliminates this problem , the baseline remains frozen for the entire conflict duration.

A ratio of 1.5 means 50% above baseline. A ratio of 0.7 means 30% below.

**Limitation:** The 30-day window size is assumed rather than optimised. A sensitivity analysis across multiple window sizes would strengthen the robustness of these conclusions.

### Lead-Lag Measurement
The lead-lag relationship is measured using Pearson correlation with a one-day shift:
- **Volume leads Volatility:** correlation between yesterday's normalised volume and today's normalised volatility
- **Volatility leads Volume:** correlation between yesterday's normalised volatility and today's normalised volume

Whichever correlation has the higher absolute value indicates which metric tends to move first.

**Limitation:** The shift(1) Pearson approach assumes sequential relationships exist to detect. When markets process shocks simultaneously, this method produces weak correlations as a methodological artefact of simultaneity rather than absence of relationship. Visual inspection is therefore used alongside correlation analysis.

### Pre-Event Signal Detection
Each conflict's baseline period is split into early (first half) and late (second half) portions. The late baseline is expressed as a ratio of the early baseline to detect whether anticipatory positioning was already underway before the official event start.

---

## Conflict Selection

Four geopolitical conflicts were selected, all sharing the same transmission mechanism , supply disruption through physical obstruction or destruction of oil infrastructure and trade routes:

| Conflict | Baseline Period | Event Period |
|---|---|---|
| Post 9/11 | 12 Jul 2001 – 10 Sep 2001 | 11 Sep 2001 – 11 Mar 2002 |
| Iraq War | 1 Jan 2003 – 19 Mar 2003 | 20 Mar 2003 – 31 Dec 2003 |
| Russia-Ukraine | 25 Dec 2021 – 23 Feb 2022 | 24 Feb 2022 – 30 Jun 2023 |
| Iran-US Tension | 20 Jan 2026 – 26 Feb 2026 | 27 Feb 2026 – present |

**Conflicts deliberately excluded:**
- **COVID-19** , a demand-side shock (economic activity froze, collapsing demand) rather than a supply-side disruption. Mixing transmission mechanisms would produce misleading comparisons.
- **2008 Financial Crisis** , same reasoning. A demand and confidence collapse rather than a supply disruption.

**Note on event end dates:** End dates involve judgment calls rather than systematic criteria. A more rigorous approach would define event end as when prices return within 10% of the pre-event baseline.

**Note on Iran-US Tension:** Only 11 days of event data exist at time of writing. Insufficient for robust lead-lag conclusions. Included as a preliminary observation only and excluded from final conclusions.

---

## Results

### Lead-Lag Correlations

| Conflict | Volume leads Volatility | Volatility leads Volume |
|---|---|---|
| Post 9/11 | 0.1245 | -0.1679 |
| Iraq War | -0.1266 | -0.0406 |
| Russia-Ukraine | 0.2393 | 0.2411 |
| Iran-US Tension | NaN | NaN |

### Pre-Event Signal Detection

| Conflict | Late Volume Ratio | Late Volatility Ratio |
|---|---|---|
| Post 9/11 | 0.9810 | 0.9255 |
| Iraq War | 0.9620 | 1.4358 |
| Russia-Ukraine | 1.2421 | 1.3645 |

---

## Interpretation

### Post 9/11 , Complete Surprise
Both pre-event metrics were suppressed below 1.0 , markets were quieter than normal before the attack. No anticipatory positioning was possible because no information was available. This serves as a natural control case.

During the event, volatility led volume at -0.1679 versus 0.1245 , the strongest directional asymmetry across all three conflicts. High volatility days tended to be followed by below-average volume, suggesting traders hedged immediately through options (spiking volatility) before committing to directional positions (volume). Directional conviction requires a framework for understanding what an event means for prices , a complete surprise provides no such framework immediately.

### Iraq War , Partial Anticipation
Pre-event results show a striking asymmetry: volume ratio near 1.0 while volatility ratio sits at 1.44. Traders were elevating volatility through options hedging while leaving directional volume completely unchanged , consistent with a conflict that was publicly expected but whose exact timing remained uncertain.

During the event, near-zero correlations in both directions suggest the market was caught between sequential and simultaneous processing. Neither metric consistently preceded the other , random directional noise rather than a clean pattern in either direction. This likely reflects pre-event price discovery: tensions had been building publicly for months, meaning markets had already partially priced in the conflict before the invasion began.

### Russia-Ukraine , Full Anticipation
Both pre-event metrics were elevated simultaneously , volume 24% above early baseline, volatility 36% above , reflecting confident directional positioning enabled by weeks of publicly visible troop buildups.

During the event, near-identical correlations of 0.2393 and 0.2411 suggest simultaneous rather than sequential movement. However, visualisation of the full conflict window reveals this as a methodological artefact: both metrics spiked simultaneously on the invasion's first days, meaning the shift(1) approach actively misaligns their peaks against each other's declines, mechanically producing weak correlations. Three distinct phases are visible:

- **Phase 1 (Feb–Mar 2022):** Simultaneous sharp spike. Volatility's initial reaction was proportionally ~4x larger than volume's, consistent with immediate options hedging.
- **Phase 2 (Apr–Sep 2022):** Both metrics dropped back toward and below baseline as the initial shock was absorbed.
- **Phase 3 (Oct 2022–Jun 2023):** Both metrics remained largely below baseline, the conflict having become background noise.

---

## Central Finding

Geopolitical oil shocks do not produce a consistent universal lead-lag pattern. Instead, the degree of public information available before a shock determines where on a spectrum the market's reaction falls:

| Anticipation Level | Pre-Event Signal | Event-Period Pattern |
|---|---|---|
| Full surprise (Post 9/11) | Suppressed , no signal | Sequential: volatility leads volume |
| Partial anticipation (Iraq War) | Asymmetric , volatility only | Random near-zero , neither metric leads |
| Full anticipation (Russia-Ukraine) | Both metrics elevated | Simultaneous , artefact of simultaneity |

This challenges the assumption that volume reliably acts as a leading confirming indicator during geopolitical crises. Whether volume leads, follows, or moves simultaneously with volatility depends fundamentally on how much information markets had access to before the shock arrived.

---

## Important Methodological Acknowledgment

The spectrum hypothesis emerged from interpreting results rather than being pre-specified before analysis. This is **exploratory analysis rather than hypothesis testing**, and the finding should be framed accordingly.

A falsifiable prediction that would confirm this hypothesis against new data: a fourth conflict with moderate and partially public anticipation should produce moderate correlations , neither the directional asymmetry of Post 9/11 nor the simultaneous movement of Russia-Ukraine, but the near-zero random noise pattern of Iraq War. Testing this prediction against additional conflicts would determine whether the spectrum is a genuine market phenomenon or a pattern retrofitted to three data points.

---

## Limitations Summary

1. **Lead-lag measurement:** shift(1) Pearson correlation assumes sequential relationships. Simultaneous movement produces misleading weak correlations.
2. **Baseline window:** 30-day window assumed rather than optimised.
3. **Event end dates:** Judgment calls rather than systematic criteria.
4. **Sample size:** Three historical conflicts insufficient to generalise with statistical confidence.
5. **Single variable:** Analysis uses one commodity. Cross-commodity validation would strengthen conclusions.

---

## Tools and Libraries

- **Python 3.13**
- **pandas** , data manipulation and time series analysis
- **numpy** , numerical computing
- **matplotlib** , visualisation
- **Jupyter Notebook** , development environment (Carnets on iPad)

---

## Repository Structure

```
oil-market-analysis/
│
├── README.md
├── oil_prices_project.ipynb       # Main analysis notebook
└── data/
    └── crude_oil_ohlcv.csv        # Raw dataset (source: Kaggle)
```

---

## Author

Self-directed project by Hemanth, developed during National Service (2024–2026).
Motivated by the 2026 Iran-US tensions and an interest in understanding how geopolitical events are processed by financial markets.

---

## Acknowledgments

Dataset sourced from Kaggle. Analysis conducted independently using publicly available data.
