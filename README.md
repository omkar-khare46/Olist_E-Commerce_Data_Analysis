Olist E-Commerce Delivery Performance Analysis

Exploratory analysis of ~99,400 orders on the Olist Brazilian e-commerce marketplace, identifying where delivery delays originate in the fulfillment pipeline and what they cost the business in customer satisfaction and revenue exposure. Includes a Python EDA notebook, a Power BI dashboard, and a written business report with prioritized recommendations.

Problem

Late deliveries hurt an e-commerce marketplace two ways: directly, through customer dissatisfaction, and indirectly, through repeat-purchase risk. Before you can fix late delivery, you need to know where in the pipeline it's happening — is it the seller shipping late, the carrier taking too long in transit, or payment approval delaying the order before it even starts moving — because each has a different fix and a different owner.

This project asks three questions:

Where do delays originate: approval, seller handoff, or carrier transit?
Which sellers, cities, and categories are disproportionately affected, once order volume is accounted for?
Does delivery performance actually move customer reviews and repeat purchases, or is that assumption wrong?
Approach
Cleaned and joined the Olist orders, order items, products, customers, sellers, and reviews tables (~99,400 orders, ~112,600 line items, ~99,200 reviews).
Built on-time/late flags at each pipeline stage (approval, carrier handoff, customer delivery) and cross-tabbed them to separate seller-caused delay from carrier-caused delay.
Ranked sellers, cities, and categories by late rate, not raw late count, with a minimum order-volume floor — a high-volume seller or city will always show more late orders in absolute terms, so rate is the metric that actually points to a real problem.
Tested whether delivery lateness is statistically associated with review score using a chi-square test of independence, rather than reading the pattern off a table by eye.
Corrected customer_unique_id (the actual per-shopper identifier) in place of the per-order customer_id field, which the dataset's schema makes easy to use incorrectly, to get a real repeat-purchase rate.
Built a companion Power BI dashboard covering revenue, order volume, delivery status, review scores, and a geographic view of where late deliveries concentrate.
Key Findings
Most customer-facing delay is a carrier problem, not a seller problem. 72.8% of orders that arrived late to the customer had reached the carrier on time — the seller did their job; the delay happened in transit.
A small number of sellers and one city (Salvador) are disproportionately responsible for lateness. Salvador's late rate (15.3%) is roughly 3x São Paulo's despite far lower order volume — a route/regional issue, not a demand issue.
Delivery lateness is statistically associated with review score (chi-square test, p < 0.001) — on-time delivery share rises from ~62% among 1-star reviews to 97% among 5-star reviews. It doesn't fully explain poor reviews, though: even among 1-star reviews, most orders were still delivered on time.
Repeat-purchase rate is low — around 3% of customers placed more than one order over the two-year window studied, using the corrected customer identifier.
Approval delay (>48hrs, 5.1% of orders) and carrier handoff delay (8.9% of orders) are largely independent problems — only ~0.6% of orders are late on both, meaning they need separate fixes, not one shared root cause.
Business Recommendations
Priority	Finding	Recommendation
1	72.8% of late-to-customer orders had on-time carrier handoff	Review carrier/logistics partner performance on affected routes, prioritizing Salvador
2	A few sellers show late rates far above the ~9% marketplace average	Build a rate-based (not count-based) seller watchlist with a minimum volume floor
3	Approval delay is independent of carrier delay	Audit payment/fraud-check turnaround as its own workstream
4	Low repeat-purchase rate + review-score link to delivery	Test a win-back offer for customers who experienced a late delivery

Full reasoning, quantified impact, and tracking metrics for each recommendation are in reports/Olist_Delivery_Performance_Report.docx.

Dashboard Preview

Show Image Revenue, order volume, monthly trend, and top categories/cities by sales.

![alt text](Olist_Sales_Analysis_Dashboard-1.png)

Show Image Review score vs. delivery status, payment method breakdown, and a geographic view of late deliveries across Brazil.

![alt text](Olist_Customer_Analysis_Dashboard-1.png)

Repo Structure
├── notebooks/
│   └── olist_eda.ipynb              EDA notebook — cleaning, delay analysis, hypothesis testing
├── reports/
│   └── Olist_Delivery_Performance_Report.docx
├── dashboards/
│   └── screenshots/
└── README.md
Data

Olist Brazilian E-Commerce Public Dataset (Kaggle). Not included in this repo — download separately to reproduce.

Dashboard built in Power BI Desktop; the .pbix file isn't included here due to size — screenshots above show the key views.

Tools

Python (pandas, numpy, matplotlib, seaborn, scipy), Power BI, DAX