# Superstore Profit Leak

This project looks at a retail dataset and asks a simple question:

**Where are we making sales but losing money?**

## Data

- File: Sample Superstore
- Years: 2014 to 2017
- Rows: 9,994
- Orders: 5,009
- One row = one product on an order, not one full order

I know that because there are 9,994 rows but only 5,009 different Order IDs.

## What I asked

1. Do discounts above 20% line up with losses?
2. Which products sell well but still lose money?
3. Which category brings in the least profit?

## What I found

**1. High discounts sit with the losses.**  
The High discount band (above 20%) has **-$135,376.06** profit and a **-37.32%** margin.  
The 0% and Low bands are still in profit.  
All three categories go negative when the discount is High.

This does not prove that the discount alone caused every loss. Cost and list price can also matter. It does show that this band is where profit disappears.

**2. A short list of products is doing real damage.**  
Cubify CubeX 3D Printer Double Head Print: sales **$11,099.96**, profit **-$8,879.97**, margin **-80.00%**, 3 orders, weighted discount **48%**.

The loss is not on every order. Drill-through shows three orders:

| Order ID | Date | Region | Sales | Profit | Margin | Discount |
|---|---|---|---|---|---|---|
| CA-2015-147830 | 15 Dec 2015 | East | $1,799.99 | -$2,639.99 | -146.67% | 70% |
| CA-2016-108196 | 25 Nov 2016 | East | $4,499.99 | -$6,599.98 | -146.67% | 70% |
| CA-2017-149881 | 01 Apr 2017 | West | $4,799.98 | $360.00 | 7.50% | 20% |
| Total | | | $11,099.96 | -$8,879.97 | -80.00% | 48% |

Two East orders at 70% discount create the whole loss. The West order at 20% is in profit. So the product is not a loss on its own. The 70% discount is.

The same list also includes the other CubeX printer, a Lexmark printer, conference tables, and the GBC DocuBind P400.  
I drilled into DocuBind earlier: sales **$17,965.07**, profit **-$1,878.17**, margin **-10.45%**, 6 orders, weighted discount **0.29**.

**3. Furniture is the weak category.**  
Technology and Office Supplies carry most of the profit. Furniture profit is much smaller on the same page ($18K). It is still positive. It is not a loss.

## What I would do

**Action 1 — Sales**  
Review every discount above 20%. Do not use that level as a normal offer.  
Evidence: High band profit **-$135,376.06**, margin **-37.32%**.  
If this works: High-band losses shrink and company margin goes up.

**Action 2 — Sales / product**  
Do not stop the CubeX Double Head printer. Stop the 70% discount on it. Then check the other names on the loss list the same way: order by order, not product by product.  
Evidence: two East orders at 70% (CA-2015-147830 and CA-2016-108196) make the -$8,879.97. The West order at 20% (CA-2017-149881) makes +$360.  
If this works: those two orders no longer sit near -147% margin, and the product margin is no longer around -80%.

**Action 3 — Sales / Consumer**  
Review high discounts in Consumer. The whole Consumer segment can look fine. The High-discount slice inside it does not.  
Evidence: Consumer + High discount profit **-$71,890**, margin **-37.99%**.  
If this works: that slice loses less money. Note: full Consumer in 2017 is still in profit (margin **13.73%**). The problem is the high-discount part, not every Consumer order.

## How I built it

1. Cleaned the file in Power Query (types, trim, blank rows). Added `ProfitFlag` and `DiscountBand` (0 / Low / High). Discount stays between 0 and 0.8. **1,871** rows have negative profit.
2. Built a star schema in Power BI: FactSales + Customer, Product, Location, and Date.
3. `Product ID` is not unique (1,862 IDs, 1,894 ID + name pairs), so I made `ProductKey` = Product ID + Product Name.
4. `LocationKey` = Country | Region | State | City.
5. All numbers on the report come from DAX measures, not dragged columns.
6. Two pages: **Profit Leak** and **Loss Product Detail** (drill-through). Page 2 shows the product name, five KPIs, and the orders behind that product.

## Checks

I compared Power BI with Excel (filters + SUBTOTAL / SUMIFS).

| Check | Number |
|---|---|
| Company sales | $2,297,200.86 |
| Company profit | $286,397.02 |
| Company margin | 12.47% |
| Orders | 5,009 |
| High-discount profit | -$135,376.06 |
| 2017 + Consumer | Sales $331,904.70, Profit $45,568.24, Margin 13.73% |
| Consumer + High discount | Profit -$71,890, Margin -37.99% |
| CubeX Double Head | Sales $11,099.96, Profit -$8,879.97, Margin -80.00%, Orders 3, Wtd discount 48% |

Order Count is 5,009 orders, not 9,994 rows.  
Profit PY is in the model. It is not on the page.  
On page 2, row discount is the order discount (70 / 70 / 20). The total 48% is the sales-weighted discount, not a simple average.

## Report

!(profit-leak.png)
-----------------------------------------------------------------------------------------------------------------------------
!(loss-detail.png)
## AI

I used AI to help write questions and to check if my actions were too vague.  
I did not let it invent numbers. Excel and Power BI did that.  
I also kept the line that “linked with losses” is not the same as “caused the losses.”

## Tools

Excel, Power Query, Power BI, DAX

