Approximate number of rows: 9,994
Number of columns: 19

What I think one row means:
One row represents one product line/item within an order.

10 columns and what they mean:

1. Row ID — A unique ID/number given to each row in the dataset.
2. Order ID — ID of the order.
3. Order Date — The date when the order was placed.
4. Ship Date — The date when the order was shipped.
5. Ship Mode — The method/type of shipping used for the order.
6. Customer ID — The unique ID of the customer.
7. Segment — The customer segment, such as Consumer, Corporate, or Home Office.
8. Category — The main category of the product, such as Furniture, Office Supplies, or Technology.
9. Sales — The sales/revenue amount for that row.
10. Profit — The profit earned (or loss if the value is negative) from that row.

Data types checked — columns have appropriate data types.

Data Profiling

No nulls found.
Order Date: 1/3/2014 – 12/30/2017
Sales: 0.444 – 22638.48
Quantity: 1 – 14
Discount: 0 – 0.8
Profit: -6599.978 – 8399.976
Profit < 0: 1871 rows
Total rows: 9994
Distinct Order IDs: 5009

Cleaning

1. Trimmed text columns.
2. Removed blank rows.
3. Confirmed Discount is within 0–1.
4. Added ProfitFlag (Loss/Profit).
5. Added DiscountBand (0/Low/High)

GRAIN: One row = one product line on an order.
Proof: row count is 9,994; distinct Order IDs are 5,009.

Kill Vanity Questions

Keep:
1. Which products make sales but lose money?
2. Do high discounts tend to be associated with lower profit?
3. Which segment has high sales but weak profit margin?
4. What happens to profit when discounts are above 20%?

Vanity:
1. Which region has the highest sales?
2. Are losses concentrated in one sub-category?
3. Which sub-category has the most profit loss?
4. Which ship mode is used most?

Question: Which products make sales but lose money?
Metric I will use: Sales and Profit
Who cares: Finance / Product manager
Possible action if the answer is ugly: Review pricing, discounts, or costs for loss-making products.

Question: Do high discounts tend to be associated with lower profit?
Metric I will use: Average Profit by Discount level
Who cares: Finance / Sales
Possible action if the answer is ugly: Review high-discount deals.

Question: Which segment has high sales but weak profit margin?
Metric I will use: Sales and Profit Margin %
Who cares: Sales / Finance
Possible action if the answer is ugly: Review pricing, discounts, or costs for the weak-margin segment.

Question: What happens to profit when discounts are above 20%?
Metric I will use: Total Profit and Average Profit
Who cares: Finance / Sales
Possible action if the answer is ugly: Review or limit high-discount deals.

AI Check

1. AI said Q1 metric is strong.
2. AI said Q2 metric should include profit margin, not only average profit.
3. AI said Q3 metric is strong.
4. AI said Q4 overlaps with Q2.
5. Decision: Accepted Q2 metric change; kept Q4 because it focuses specifically on discounts above 20%.

WEEK 2

Q1: Which products make sales but lose money?
Metric: Sales and Profit

Q2: Do high discounts tend to be associated with lower profit?
Metric: WEEK 2

Q1: Which products make sales but lose money?
Metric: Sales and Profit

Q2: Do high discounts tend to be associated with lower profit?
Metric: Total Profit and Profit Margin % by DiscountBand

Day 2 — Base Pivots

4 PivotTables created:

* Category: Sales, Profit, Profit Margin
* Region: Sales, Profit, Profit Margin
* Segment: Sales, Profit, Profit Margin
* DiscountBand: Sales, Profit, Profit Margin, Row count

Sales vs Profit Mismatch:
No mismatch found. In Category, Region, and Segment, the group with the highest sales also had the highest total profit.

What I expected: High discount leads to loss.

What the table shows: All three categories become loss-making at High discount.

Possible fake insight: High discounts cause losses.

Why it might be fake: The table shows an association, but it does not prove that high discounts caused the losses. Other factors such as pricing or costs may also affect profit.

Shop-owner takeaway: High discounts are buying sales but burning profit, especially across all three categories.

Number: High-discount profit

Pivot result: -135,376.056

Second method result: -135,376.056

Match? Yes

If no, why: Not applicable

Question: Which products make sales but lose money?

Data used: Product Name, Sales, and Profit.

Metric: Total Sales and Total Profit.

Finding (one sentence, with the number): Cubify CubeX 3D Printer Double Head Print generated $11,099.963 in sales but had a loss of $8,879.9704.

So what?: The product is generating sales but losing a large amount of profit.

Next action (one concrete action): Review the product's pricing, discount, and costs before continuing to push sales.

Verified how: PivotTable and SUMIFS; both returned -8,879.9704 profit.

Question: Do high discounts tend to be associated with lower profit?

Data used: DiscountBand, Sales, Profit, and Profit Margin.

Metric: Total Sales, Total Profit, and Profit Margin %.

Finding (one sentence, with the number):** High discounts are associated with losses, with the High discount band having **−$135,376.06 profit** and a **−37.32% profit margin**.

So what?: Discounts may increase sales, but high discounts are also associated with significant losses.

Next action(one concrete action): Review high-discount deals and consider reducing or limiting discounts above 20%.

Verified how: PivotTable using DiscountBand with Sales, Profit, and Profit Margin.
