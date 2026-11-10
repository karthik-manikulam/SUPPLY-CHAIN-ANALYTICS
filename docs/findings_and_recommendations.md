## Findings

**1. Canceled orders were hiding in the "not late" bucket.**
The raw late-delivery logic counted canceled orders as "not late" by construction. This understated the true late rate and distorted every segment comparison. I replaced it with a 3-way outcome model (Late / On-Time-or-Advance / Canceled), so rates are calculated on orders that were actually delivered.

**2. Delivery performance is poor overall.**
The company-wide late rate is **54.8%**, and it is the benchmark used across all dashboard visuals.

**3. First Class shipping is the biggest anomaly.**
First Class runs a **90%+ late rate**, versus **30-40% for Standard Class**. This is counterintuitive, since the premium tier performs worst, and it holds across all regions. That points to a process or carrier-commitment problem rather than a local one.

**4. Impact is concentrated in a few segments.**
Ranking by **Priority Risk (late rate × order volume)** surfaces the segments where fixing delivery would matter most, instead of the ones with high rates on tiny samples.

**5. Small samples can mislead.**
Some segments show extreme late rates on very few orders (for example, Southern Africa on ~38 orders). I flagged these rather than ranking them as top priorities.

## Recommendations

1. **Audit First Class first.** Check whether the promised delivery windows are realistic and whether carrier or fulfillment handling differs from Standard Class. Since the problem is consistent across regions, start with the shipping-mode policy rather than regional operations.
2. **Prioritize by Priority Risk, not late rate alone.** Focus on high-volume, high-late-rate segments, where a fix has the largest effect.
3. **Set realistic delivery promises.** If First Class cannot hit its window, either fix fulfillment or adjust the promised timeline so customers aren't consistently let down.
4. **Fix the late-delivery metric in reporting.** Track Late / On-Time / Canceled separately so cancellations stop flattering the on-time numbers.
5. **Monitor against the company average.** Use the 54.8% benchmark to track whether top segments are converging toward it.
6. **Validate small-sample segments before acting.** Collect more data on low-volume segments with extreme rates before committing resources.

**Estimated impact:** if the top-priority segments matched the company average, late deliveries would fall by an estimated **X%**. *(Replace X with the figure from your dashboard.)* This is a relative benchmark, not a forecast of financial return.

## Limitations
- The analysis is descriptive and diagnostic: it shows where and how much, not why. Root-cause testing would need operational data such as carrier and warehouse information.
- The dataset is a single flat file with no carrier or warehouse detail.

I left the impact figure as a placeholder rather than invent one. Give me the number from your dashboard and I'll fill it in, or I can trim this to a shorter version for the README.
