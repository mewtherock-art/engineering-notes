# 13 · Evaluating trading strategies without fooling myself

**Stack:** MQL5 expert advisors, Python analysis scripts, MetaTrader backtester · **Scope:** a personal research project; no account details or strategy parameters published

## Problem
It is easy to find a strategy that looks great in a backtest. The hard part is knowing whether it is
real.

## Rules I settled on
1. **Judge by risk-adjusted metrics**, never net profit alone.
2. **Remove the top 5 winning trades** and re-check. If the edge disappears, it was a few outliers.
3. **A challenger must beat the baseline on every risk-adjusted metric** to replace it. One candidate
   that split the strategy in two lost on all of them, so the idea was closed rather than kept
   "just in case".
4. **Collect forward data** (live and demo) and compare with the backtest before trusting either.
5. **Reconcile results in code.** A false-negative trade result was found and fixed by reconciling
   what the EA believed with what actually closed.

## A bug found by reading live logs
One short trade opened with **7.27% account risk instead of the configured 2% (3.6×)** and hit its
full stop-loss. The post-mortem traced it end to end:
- Two thresholds measured the same thing but were set unequally: the entry threshold (0.25) sat
  *below* the "big candle → tight stop" threshold (0.31). Candles in the 0.25–0.31 gap could enter
  but never got the tight stop.
- The stop fell back to a swing/ATR stop ~8× wider than normal, so the risk-sized lot was below the
  broker minimum.
- A setting (`blockUnderMinLot = false`) then let the trade open at minimum lot, which was the
  over-risk.
- I ruled out trailing as a cause (it behaved as designed; the wider stop just moved its first
  trigger out by the same factor).

**Fix choice:** first a one-line config change that rejects any trade whose minimum lot would exceed
the risk budget. I chose it over changing the threshold because it caps risk on *every* path, not just
this gap (an earlier trade with the tight stop active still carried 3.12% risk) and it leaves stop
placement, and so the tuned trailing behaviour, untouched. Next version adds a start-up warning when
the thresholds are inconsistent and a log line for the fallback case. I deliberately delayed
deploying it until after a scheduled review so the binary and the config would not change at once.

The post-mortem states plainly that the root cause was proven but the **fix was not yet validated**.

## Lesson
Most of the work is building tests that can say "no". I kept a log of what was stopped and why
(including a strategy version I confirmed stopped), so I do not rediscover the same mistake.
