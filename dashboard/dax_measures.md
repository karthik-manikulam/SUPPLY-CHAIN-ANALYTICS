1)Average Delay Days = CALCULATE(AVERAGE(df[delay_days]),df[delay_days]>0)

2)Avg Delay Days - Worst Mode = 
VAR WorstMode =
    TOPN(
        1,
        ADDCOLUMNS(
            VALUES(df[shipping_mode]),
            "LateRate",
            CALCULATE(AVERAGE(df[late_delivery_risk]))
        ),
        [LateRate], DESC
    )
RETURN
    CALCULATE(
        AVERAGEX(
            FILTER(
                df,
                df[delay_days] > 0
            ),
            df[delay_days]
        ),
        KEEPFILTERS(WorstMode)
    )

3)Canceled Orders = 
CALCULATE(
    COUNTROWS(df),
    df[delivery_outcome] = "Canceled"
)

4)Cancellation % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(df),
        df[delivery_status] IN {"Shipping canceled"}
    ),
    COUNTROWS(df)
)

5)Customers Affected = 
CALCULATE(
    DISTINCTCOUNT(df[customer_id]),
    df[delay_days] > 0
)
6)Late Delivery Rate = 
DIVIDE(
    CALCULATE(
        SUM(df[late_delivery_risk]),
       df[delay_days]>0 
    ),
    COUNTROWS(df)
)
7)Late Orders = 
CALCULATE(
    COUNTROWS(df)
    ,df[delivery_outcome]= "Late"
)
8)Negative Profit from Late Orders = 
CALCULATE(
    SUM(df[order_profit_per_order]),
    df[delay_days] > 0,
    df[order_profit_per_order] < 0
)
9)On Time or Early Orders = 
CALCULATE(
    COUNTROWS(df),
    df[delivery_outcome] = "On-Time/Advance"
)
10)On-Time/Advance Rate % = 
DIVIDE(
    CALCULATE(
        COUNTROWS(df),
        df[delivery_status] IN {"Advance shipping", "Shipping on time"}
    ),
    COUNTROWS(df)
)
11)Product Late Rate Rank = 
RANKX(
    ALLSELECTED('df'[product_name]),
    [Late Delivery Rate],
    ,
    DESC,
    DENSE
)
12)Profit on Late Orders = 
CALCULATE(
    SUM(df[order_profit_per_order]),
    df[delay_days] > 0
)
13)Sales Exposure from Late Orders = 
CALCULATE(
    SUM(df[sales]),
    df[delay_days] > 0
)
14)total % of orders = DIVIDE(COUNT(df[order_id]),[Total orders])
15)Total orders = COUNT(df[order_id])
16)Worst Mode Late Rate = 
VAR WorstMode =
    TOPN(
        1,
        ADDCOLUMNS(
            VALUES(df[shipping_mode]),
            "LateRate", CALCULATE(AVERAGE(df[late_delivery_risk]))
        ),
        [LateRate], DESC
    )
RETURN
    MAXX(WorstMode, [LateRate])
17)Worst Mode vs Average = (
[Worst Mode Late Rate] - [Late Delivery Rate])*100
18)Worst Shipping Mode = 
VAR ModeTable =
    ADDCOLUMNS(
        VALUES(df[shipping_mode]),
        "LateRate",
            CALCULATE(
                AVERAGE(df[late_delivery_risk])
            )
    )
RETURN
    CONCATENATEX(
        TOPN(1, ModeTable, [LateRate], DESC),
        df[shipping_mode]
    )
