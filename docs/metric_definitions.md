Metric Definitions

This document defines every metric used in the Supply Chain Delivery Performance dashboard so results can be interpreted and reproduced consistently.

1. Delivery Outcome (3-way classification)

Each order is assigned exactly one outcome:

Outcome	Definition
Late	Order was delivered and actual shipping days exceeded scheduled shipping days
On-Time or Advance	Order was delivered within or ahead of the scheduled shipping days
Canceled	Order was canceled and never delivered

Why this matters: In the raw data, canceled orders were silently counted as "not late", which understated the true late rate. Classifying them separately keeps cancellations out of the on-time numbers.

Verify against your code: confirm the exact columns used (e.g. actual vs. scheduled shipping days, order status) and update this table if your logic differs.

2. Core Metrics
Metric	Definition	Formula
Total Orders	Count of all orders in scope	COUNT(orders)
Delivered Orders	Orders that were not canceled	Total Orders - Canceled Orders
Late Orders	Orders classified as Late	COUNT(outcome = Late)
Late Rate	Share of delivered orders that arrived late	Late Orders / Delivered Orders
Cancellation Rate	Share of all orders that were canceled	Canceled Orders / Total Orders

Verify against your code: confirm whether Late Rate uses delivered orders or all orders as the denominator, and keep this table consistent with the dashboard.

3. Benchmark
Metric	Definition	Value
Company Average Late Rate	Overall Late Rate across all delivered orders. Anchors the diverging color scale and all "vs. average" comparisons	54.8%
4. Prioritization Metrics
Metric	Definition	Formula
Priority Risk	Ranks segments by combined impact of lateness and size, so high-rate but tiny segments don't dominate	Late Rate × Order Volume
Rate vs. Average	Gap between a segment's Late Rate and the company average	Segment Late Rate - 54.8%
5. Prescriptive Metrics (Page 2, in progress)
Metric	Definition
Loss Gap %	How far a segment's loss is above the company-average benchmark (finalize formula once Page 2 is complete)
Estimated Recoverable Loss	Estimated reduction achievable if a segment performed at the company average. A relative benchmark, not a forecast of financial return (finalize formula once Page 2 is complete)
6. Segmentation Dimensions

Metrics can be sliced by: Shipping Mode, Market, Region, Country, Product / Category, Customer Segment, and Month.

7. Interpretation Rules
Rate vs. volume: Always read a rate alongside order volume.
Small samples: Segments with very few orders (e.g. Southern Africa, ~38 orders) are flagged and not ranked as top priorities.
Color scale: One diverging scale centered on 54.8%. Warmer means worse than average.
No fabricated ROI: Impact is expressed relative to the company average, not as invented financial returns.
