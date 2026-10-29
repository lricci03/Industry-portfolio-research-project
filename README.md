# Industry Momentum: A Historical Analysis of US Industry Portfolios

An exploratory quantitative research project testing whether the best-performing US industry in one month continues to outperform in the following month.

Using the **Kenneth R. French 10 Industry Portfolios** daily value-weighted returns, the notebook constructs a simple monthly industry-rotation strategy, compares it with an equal-weighted industry benchmark, and examines how results change over time.

## Research question

**Does the previous month's winning industry provide useful information about next month's returns?**

The analysis considers two related questions:

1. **Winner persistence:** How often is the top-performing industry in one month also the top performer in the next?
2. **Investment performance:** What would happen if we invested each month in the previous month's winner, rather than holding an equal-weighted portfolio of all ten industries?

## Data

- **Source:** [Kenneth R. French Data Library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html), *10 Industry Portfolios, Daily*.
- **Sample:** July 1926–September 2026, as recorded in the downloaded September 2026 file.
- **Coverage:** 10 US industry portfolios; the notebook uses the **value-weighted daily returns** table.
- **Processing:** Identify the relevant table within the source CSV; convert dates to a datetime index; check missing-value codes; compound daily returns into calendar-month returns.

The original daily returns are percentages (for example, `1.5` means 1.5%); the derived monthly returns are decimals (`0.015`).

## Methodology

### 1. Winner persistence

For every month, identify the industry with the highest monthly return and count the number of times the same industry wins in consecutive months.

The notebook first considers an equal-probability model: every industry has equal probability of winning. 
Then we allow industries to have different historical win frequencies. Under an **i.i.d.** winner model with industry probabilities $p_i$, the probability of a consecutive repetition is $\sum_i p_i^2$. The calculation of the repetition-count variance also accounts for dependence between adjacent repetition indicators.

### 2. Momentum strategy and benchmark

- **Strategy:** At the end of month $t$, select the industry with the highest return in month $t$; hold that industry through month $t+1$.
- **Benchmark:** Hold an equally weighted portfolio of the ten industries, rebalanced monthly.
- **Comparison:** Average monthly returns, monthly excess returns, cumulative wealth, and log relative wealth.

The notebook also calculates a preliminary one-sided test of whether average monthly excess returns are positive, assuming i.i.d. monthly excess returns.

### 3. Performance by historical period

Compare annualised compounded return, annualised volatility, return-to-volatility ratio, and maximum drawdown for **pre-2000** and **2000 onward** samples. The split was motivated by the observed relative-wealth plot and is exploratory, not a precommitted out-of-sample test.

## Main findings

| Metric | Strategy pre-2000 | Benchmark pre-2000 | Strategy 2000 onward | Benchmark 2000 onward |
|---|---:|---:|---:|---:|
| Annualised compounded return | 15.64% | 11.67% | 5.98% | 9.19% |
| Annualised volatility | 23.00% | 18.78% | 23.54% | 14.91% |
| CAGR / annualised volatility | 0.680 | 0.621 | 0.254 | 0.616 |
| Maximum drawdown | −83.16% | −82.06% | −62.36% | −48.09% |

Additional exploratory results:

- The same industry was the monthly winner in **176** consecutive-month comparisons.
- Mean monthly strategy return: **1.237%**; equal-weight benchmark: **1.005%**.
- Mean monthly excess return: **0.232 percentage points**, with monthly excess-return standard deviation of approximately **4.11%**.
- The relative-wealth plot shows that the strategy accumulated most of its relative advantage before approximately **1999**, followed by a sustained period of relative underperformance.

**Takeaway:** The simple one-month winner strategy performs better than the benchmark in the earlier historical period, but worse after 2000, with considerably greater volatility. Its apparent full-sample advantage is therefore not stable across time.

## Limitations

- **Exploratory analysis:** The strategy and historical split were evaluated on a dataset already inspected during research. Results should not be presented as independent out-of-sample validation.
- **Hypothesis-test assumptions:** The winner-persistence calculation uses historical frequencies as estimates of constant industry win probabilities; those probabilities may change over time. The reported normal-approximation p-value does not fully account for estimating these probabilities from the same sample.
- **Serial dependence:** The preliminary excess-return significance calculation assumes i.i.d. monthly observations; returns may be serially correlated.
- **Implementation costs:** Transaction costs, slippage and practical investability are not modelled.
- **Risk and concentration:** The momentum strategy allocates to just one industry each month.
- **Performance terminology:** The notebook currently labels `CAGR / annualised volatility` as a “Sharpe ratio.” This is **not** a conventional Sharpe ratio, which uses excess returns over a risk-free rate and a different return convention.

## Running the notebook

1. Download **10 Industry Portfolios — Daily** from the [Kenneth R. French Data Library](https://mba.tuck.dartmouth.edu/pages/faculty/ken.french/data_library.html).
2. Extract the file and place `10_Industry_Portfolios_Daily.csv` in the same directory as `industry_porfolio.ipynb`.
3. Install the required Python packages:

   ```bash
   pip install pandas numpy matplotlib scipy jupyter
   ```

4. Open the notebook in Jupyter or VS Code and run the cells in order.

**Before running:** The current CSV import uses fixed row offsets (`skiprows=9`, `nrows=26338`) specific to the September 2026 source file. If using a different vintage, verify the file layout and update those parameters.

This is an educational, exploratory research notebook, not an investable strategy recommendation.
