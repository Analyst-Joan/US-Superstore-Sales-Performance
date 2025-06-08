# US-Superstore-Sales-Performance - Exploring Profitability Across Products, Segments & Regions

![Store Image](products.jpeg)

[Photo Credit](https://create.microsoft.com/en-us/features/ai-image-generator)

## Background
As a Business Manager for a fictitious United States based Superstore, I conducted an Exploratory Data Analysis (EDA) on product performance data to uncover underperforming areas across states, market segments, and product categories. The goal was to identify key pain points and provide data-driven recommendations to improve sales, optimize product mix, and enhance regional strategies.

## Target Audience
This project is designed to provide value to a range of business and data professionals. The insights and visualizations generated are relevant for decision-making, performance monitoring, and strategic planning across various roles:

**1. Business Stakeholders** (Sales Managers, Regional Managers, Product/Category Managers)
_Why it matters:_ Enables them to identify underperforming products, loss-making states, and profit drivers, guiding actionable improvements in pricing, inventory, and regional sales strategies.

**2. Data & Business Analysts** (Data Analysts, Business Intelligence Analysts, CX Analysts)
_Why it matters:_ Offers a case study in effective exploratory data analysis, visual storytelling, and deriving business-relevant insights from retail data, valuable for benchmarking and learning.

**3. Executives & Strategy Teams** (C-level Executives (e.g., CRO, COO), Strategic Planners)
_Why it matters:_ Provides high-level KPIs and performance breakdowns to support strategic decisions on market focus, product investment, and profitability optimization.

**4. Aspiring Data Professionals** (Junior Analysts, Students, Career Switchers)
Why it matters: Serves as a learning tool for how to conduct and present impactful EDA in a business context, with real-world relevance and dashboard design best practices.

## Business Need Addressed
The insights from this EDA serve strategic, operational, and tactical needs as outlined below:

**1. Assess Overall Business Performance**
- Understand total orders, revenue, profit, and average discounts at a glance.
- Evaluate the geographical spread and volume of operations across 49 states and 531 cities.

**2. Identify High and Low Performing Products**
- Detect top-grossing sub-categories and those consistently generating losses.
- Determine which product categories contribute most to total sales and profit.

**3. Uncover Regional and State-Level Weaknesses**
- Map revenue and profit contribution across US states.
- Pinpoint underperforming states to inform regional sales strategies.

**4. Evaluate Segment and Customer Group Performance**
- Compare revenue and profit by customer segment (Consumer, Corporate, Home Office).
- Identify the most and least profitable segment-product combinations.

**5. Understand the Impact of Discounts and Order Volumes**
- Analyze how average discount levels affect profitability across product categories.
- Flag areas where excessive discounting leads to losses despite high sales volume.

**6. Optimize Shipping Mode and Logistics Strategy**
- Assess how shipping mode (e.g., Standard, First Class) affects profit margins.
- Reveal cost-heavy shipping patterns across regions and product segments.

**7. Support Data-Driven Decision-Making**
- Provide actionable insights to guide pricing, inventory, regional targeting, and logistics.
- Empower business managers to optimize strategies for profitability and growth.

 ## Key Questions Answered
1. What are the overall sales and profit figures?
2. Which segments and product categories drive or drag performance?
3. Which states or cities are underperforming or excelling?
4. How do product sub-categories differ in terms of sales and profit?
5. What are the risks of discounting or over-ordering on profitability?
6. Which shipping modes affect cost efficiency?
7. Which customer segment + shipping method combinations reduce profitability?
8. What are the most and least profitable products?
9. Where are we losing money despite high product movement?
10. How can shipping and pricing strategies be optimized by region or segment?

## Skills/Concepts applied
-	Cleaning/Validation in Power Query
-	Defining & Computing KPIs
-	Power BI DAX Concepts: Calculated Measures
-	Data Visualization in Power BI
-	Power BI Dashboard building
-	Filters and Slicers
-	Tooltips

## About the Data
The dataset comprises a single table with 9,994 rows and 13 columns, representing detailed product order and sales records from a fictitious U.S.-based Superstore. It includes transactional data across various: **States, Market segments (e.g., Consumer, Corporate, Home Office), Product categories and sub-categories.**
Each record provides insights into key business metrics such as: *Order Date & Ship Date, Sales and Profit, Quantity and Discount, Customer and Region Details, Product Information.* The dataset was gotten from [The Spark Foundation_GRIP](https://bit.ly/3i4rbWl).  

## Defining Key Performance Indicators (KPIs)
The following metrics were identified as essential Key Performance Indicators (KPIs) to provide the business user with insights into the effectiveness of each campaign, and  Identify opportunities for optimization:

**Provided KPIs (Within the dataset)**
- _Impressions_ – Daily impressions (times ad was shown to a viewer) for each ad
- _CTR (%)_ - Daily average click-through rate for each ad


**Additional(computed) KPIs.**
- Total Engagement - _What is the overall level of user interaction or engagement with our Ads across different metrics (likes, shares, and comments)?_
- Conversion rate (%) - _What percentage of ad viewers are converting into Paying customers?_

## Data Transformation/Preprocessing
The dataset was imported into Power BI’s Power Query for data validation and cleaning.  The column profiling was changed from ‘based on Top 1000 rows’ to ‘based on entire dataset’. ‘Column quality’ and ‘Column distribution’ checkboxes were selected to get a summary information about each column for effective cleaning/Preprocessing. The data table was fairly clean and required minimal transformation, which was carried out as follows:
- Column datatypes were validated appropriately - Columns that contain financial data such as the `Sales`,`Profit`, and `Discount` were changed to Currency Type.
- There were no missing values, empty cells or duplicates.
  
## Data Modelling
The dataset was a single table. This is shown in the image below:
![](Model.JPG)

## Data Exploration (EDA)
With the data now transformed, it’s time to explore the data to analyze ?, in order to provide insights into ?, and Identify opportunities for profitability, in response to the Business need. 

I approached the analysis by .....
The metrics were then .... the following DAX Measures were created:
```
Target profit Margin = 0.20
```
```
% target profit margin to profit margin = 
DIVIDE(DIVIDE(SUM('SampleSuperstore'[Profit]), SUM('SampleSuperstore'[Sales]), 0),
[Target profit Margin])
```
```
Most Used Ship Mode = 
VAR MostUsedMode = 
    TOPN(
        1, 
        SUMMARIZE(
            'SampleSuperstore',
            'SampleSuperstore'[Ship Mode],
            "ShipModeCount", COUNTROWS('SampleSuperstore')
        ),
        [ShipModeCount], 
        DESC
    )
RETURN
    MAXX(MostUsedMode, 'SampleSuperstore'[Ship Mode])
```
```
Profit Margin = DIVIDE(SUM('SampleSuperstore'[Profit]), SUM('SampleSuperstore'[Sales]), 0)
```
```
Profit Margin to Target = 
VAR Targetdiff = [Target profit Margin] - [Profit Margin]
RETURN
IF(Targetdiff<0,0,Targetdiff)
```
Other Measures created to enhance visualiztion of the metrics:
```
Profit margin Vs Target profit Margin = 
VAR _Value = FORMAT(DIVIDE([Profit Margin],[Target profit Margin]),"0.0%")
VAR _PreText = "△% Target-PM ‎"
VAR _Space = REPT("‎",10)
VAR _Label = 
    _Space & _PreText & _Space & _Value
RETURN
_Label
```
```
Data Bar - Profit margin Vs Target profit Margin = 
VAR _NrIcons_Total = 10
VAR _PctofProfitMargin = DIVIDE([Profit Margin],[Target profit Margin])
VAR _NrIcons_Fill = _PctofProfitMargin * _NrIcons_Total
VAR _NrIcons_Empty = _NrIcons_Total - _NrIcons_Fill

VAR _IconFill = "⬤"
VAR _IconEmpty = "○"

VAR _DataBar =
    REPT(_IconFill, _NrIcons_Fill) &
    REPT(_IconEmpty, _NrIcons_Empty)

    RETURN
    _DataBar
```
With the measures for the metrics computed, it’s time to bring our analysis to life with visuals. 

## KPI Visualization/Presentation (WIP)

## Dashboard:
Having analyzed and visualized the required KPIs, [the interactive dashboard](https://app.powerbi.com/view?r=eyJrIjoiOWM0NjgzYzYtZTU1OS00M2QwLTk0NjUtZjE1Mjg0YTdlOTRkIiwidCI6ImFiMTA0YzYwLTZkZTYtNDc1ZC1hMjBmLTg5M2Y2OWQ2NzlhNCJ9), with Image shown below, was then designed with slicers, tooltips and bookmarks to enable the business user drill down and gain additional insights from the report. A brief video guide of the dashboard can also be accessed through this [link](https://drive.google.com/file/d/1ksyEqHmqp5qyN-NQhzaSSTBv6_M0o5_f/view?usp=sharing)

![](Maketing_campaign_dashboard.JPG)

## Conclusion
In Summary, 

![](Thanks_img.jpeg)
