DataCo Supply Chain: Delivery Performance Analysis

Where, when and why are orders delivered late, and what does it cost?
An end-to-end analysis of ~181K e-commerce orders (2015–2018) using Python and Power BI.

![Overview](dashboard/screenshots/1_overview.png)

Headline findings
1. 55% of orders arrive late, and the rate is stable across years (54.4%–56.3%).
2. Canceled orders were being counted as "not late" by construction. I built a 3-way
   outcome (Late / On-Time-or-Early / Canceled) so cancellations (4.3%) never inflate
   the on-time rate.
3. First Class is the outlier: 95.3% late vs a 54.8% network average, while
   Standard Class is 38.1%. The gap holds across regions, so it is a
   shipping-mode problem, not a geography problem.
4. Late orders carry ~$21M in sales exposure across ~16K customers, with about
   the same average order value (~$205) as on-time orders. Lateness is not
   limited to low-value orders.
5. Priority Risk (late rate × volume) ranks Western Europe and Central America
   as the top regions to act on first.
6. Recommendation: fix First Class first. Bringing it to network-average
   performance would remove ~[X] late orders (estimate, not a forecast).


Dashboard (Power BI, 3 pages: what → why → so what)
| Overview | Bottleneck Detection | Business & Customer Impact |
| ![](dashboard/screenshots/1_overview.png) | ![](dashboard/screenshots/2_bottleneck.png) | ![](dashboard/screenshots/3_business_impact.png) |

Design rules: one diverging color scale centered on the 54.8% company average,
a baseline line on every rate chart, and a narrative title on each page.

Methodology notes
- Late rate = late orders ÷ all orders (canceled included in the denominator).
- Minimum-volume filter (30 orders) applied to segment rankings.
- The dataset has no delay-reason column, so root cause is inferred from
  scheduled vs actual shipping days and labeled a likely driver.
- Exposure figures show what late orders carry, not proven losses caused by lateness.

Run it yourself
1. Download the data from Kaggle: `shashwatwork/dataco-smart-supply-chain-for-big-data-analysis`
   and put the CSV in `data/raw/` (see `data/README.md`).
2. `pip install -r requirements.txt`
3. `python src/bronze.py && python src/silver.py && python src/gold.py`
4. Open `dashboard/dataco_delivery.pbix` in Power BI Desktop and refresh the source path.

Python (pandas, numpy, matplotlib, seaborn) · Power BI / DAX

## Author
Karthik · [LinkedIn] · [Email]
