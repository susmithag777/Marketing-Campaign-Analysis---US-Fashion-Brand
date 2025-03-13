# Marketing Campaign Analysis--UK Fashion Brand

# Background
A UK-based clothing store launched a series of targeted marketing campaigns for the Spring, Fall, and summer seasons. Each campaign included two distinct advertisements on Facebook, Pinterest, and Instagram: one highlighting discount and the other showcasing the latest clothing collections. The campaigns specifically targeted three major cities: London, Birmingham, and Manchester. Daily performance metrics were meticulously captured across various dimensions, including cities, channels, devices, and individual ads. These metrics encompassed Impressions, Click-Through Rate (CTR), Clicks, Daily Average Cost-Per-Click (CPC), Spend, Conversions, Total Conversion Value, Likes, Shares, and Comments

# About the Data
The dataset is a Microsoft Excel file that contains one table, consisting of 9,900 rows and 18 columns, of the Daily performance metrics, of a Marketing Campaign data for a UK Fashion brand, carried out during 3 seasons in 2023 - Spring (March to May), Summer (June to August), and Fall (October to November).

# Business Need
A comprehensive Power BI report that:
1. Analyzes the Marketing Campaign performance metrics
2. Provides insights into the effectiveness of each campaign, and
3. Identifies opportunities for optimization.

# Concepts Applied
1. Defining & Computing KPIs
2. Cleaning in Power Query
3. Power BI DAX Concepts: Calculated Measures
4. Data Visualization in Power BI
5. Power BI Dashboard building
6. Filters and Slicers
7. Bookmarks and Tooltips

# Defining Key Performance Indicators (KPIs)
The following metrics were identified as essential Key Performance Indicators (KPIs) to provide the business user with insights into the effectiveness of each campaign, and Identify opportunities for optimization:

Provided KPIs (Within the dataset)
1. Impressions – Daily impressions (times ad was shown to a viewer) for each ad 
2. CTR (%) - Daily average click-through rate for each ad 
3. Clicks - Daily clicks for each ad 
4. Daily Average CPC - Daily average cost-per-click for each ad 
5. Spend, GBP - Total daily amount of advertising spending for each ad, in Great Britain Pounds(GBP)
6. Conversions - Total daily purchases attributed to a specific ad
7. Total Conversion Value, GBP - Total amount received from purchases attributed to a specific ad (Revenue)
8. Likes - Total daily likes (or other reactions) per ad
9. Shares - Total daily shares per ad (For the simplicities sake, each Pin on Pinterest was counted as a share)
10. Comments - Total daily comments per ad 

Additional(computed) KPIs.
1. Total Engagement - What is the overall level of user interaction or engagement with our Ads across different metrics (likes, shares, and comments)?
2. Conversion rate (%) - What percentage of ad viewers are converting into Paying customers?
3. Cost per Acquisition - How much does it cost to acquire a single customer?
4. Return on Ad-spend (ROAS) - How much revenue is generated for every dollar spent on advertising?
5. Return on Investment (ROI) (%) - How much profit or loss is generated from our investment relative to its cost?
6. Monetary ROI (£) - What is the net financial return (in monetary terms) on our investment after accounting for all costs and revenues?


# Data Transformation/Preprocessing
The dataset was imported into Power BI’s Power Query for data validation and cleaning. The column profiling was changed from ‘based on Top 1000 rows’ to ‘based on entire dataset’. ‘Column quality’ and ‘Column distribution’ checkboxes were selected to get a summary information about each column for effective cleaning/Preprocessing. 

The data table was fairly clean and required minimal transformation, which was carried out as follows:
1. Column datatypes were validated appropriately - the data types of Impressions, Clicks and comments columns were changed from decimal to whole number. The data type of Click-through rate (CTR) column was changed from decimal to percentage.
2. Columns that contain financial data such as the daily “Average CPC (cost-per-click) & Spend (GBP) Columns were rounded off to 2 decimal places, for consistency.
3. There were no missing values, empty cells or duplicates To enable flexibility of time-based analysis, a Calendar Table was then created in power query.


# Data Modelling
The Model from the cleaned data is a basic Star Schema comprising of: one fact table (the data table) and two, dimension tables (Calendar & Image). The date column from the Marketing data table was connected to the date column in the calendar table, via a Many-to-one relationship while the channel column from the Marketing data table was connected to the Channel column in the image table, via a Many-to-one relationship. This is shown in the image below:
<img width="728" alt="image" src="https://github.com/user-attachments/assets/ac0a19ec-9585-4d8b-9b7b-14c0d8b7c69c" />


# Data Exploration (EDA)
With the data now transformed and modelled, it’s time to explore the data to analyze the Marketing Campaign performance metrics, in order to provide insights into the effectiveness of each campaign, and Identify opportunities for optimization, in response to the Business need.

I approached the analysis by segmenting it into 3 Levels – Campaign, Channel and Ad performance, with filters, slicers and tooltips to enable interactivity and drill-down into other categories such as Cities, device, and Month. The metrics were then analyzed & summarized within 3 areas – Engagement, Revenue generated and Cost Analysis. To analyze Engagement performance of the campaign, the following DAX Measures were created:

1. Total Ads = COUNTROWS('data') 
2. Total Clicks = SUM('data'[Clicks])
3. Average CTR = AVERAGE('data'[CTR, %])
4. Total Impressions = SUM('data'[Impressions])
5. Total Likes = SUM('data'[Likes (Reactions)])
6. Total Shares = SUM('data'[Shares])
7. Total Comments = SUM('data'[Comments])

To analyze revenue and cost aspects of the campaign, the following DAX Measures were created-

Cost Analysis measures:
1. Total Ad Spend = SUM('data'[Spend, GBP])
2. Average CPC = AVERAGE('data'[Daily Average CPC])
3. Total Conversions = SUM('data'[Conversions])
4. Conversion rate = DIVIDE([Total Conversions],[Total Clicks])
5. Cost per Acquisition (CPA) = DIVIDE('_Metrics'[Total Ad Spend],'_Metrics'[Total Conversions])

Revenue Analysis Measures:
1. Total Conversion Value (£) = SUM('data'[Total conversion value, GBP])
(Conversion Value ≈ Revenue)

3. Return On Ad-spend = DIVIDE([Total Conversion Value (£)],[Total Ad Spend])

4. Return-On-Investment (ROI) = 
VAR TotalRevenue = SUM('data'[Total conversion value, GBP])
VAR TotalSpend = SUM('data'[Spend, GBP])
RETURN
IF(
    TotalSpend <> 0,
    (TotalRevenue - TotalSpend) / TotalSpend,
    BLANK()
)


5. Monetary ROI = 
    VAR _Profit = SUMX(
        'data', 
        [Total conversion value, GBP] - [Total Ad Spend]
    )
    VAR _MonetaryROI = 
        IF(
            SUMX(
                'data',
                [Total Ad Spend]
            ) = 0,
            BLANK(), // Handle division by zero error
            _Profit
        )
    RETURN _MonetaryROI
    
With the measures for the metrics computed, it’s time to bring our analysis to life with visuals.


# KPI Visualization
Our Fashion brand needed to increase its revenue while staying above the competition. We thought, in our current digital global village, what other way than a digital marketing campaign - 3 Seasons, Endless Style😉. Every weather season has an attire for it, and so we launched into the deep.

However, we needed to carry out a test-run, before a larger global based campaign. Thus, during 3 different Seasons (Fall, Spring & Summer), we Launched 2 Advert types (Discounts & Latest collections) across 3 social media channels - Facebook, Instagram and Pinterest, with a focus on 3 cities- Birmingham, Manchester and London. We also made room to understand the devices used to access the Ads, to help our design team in digital Ad design & deployment.

To begin, Let’s see how the campaigns performed on a general level.
<img width="506" alt="image" src="https://github.com/user-attachments/assets/76a11786-9244-4202-b51b-cad40fa76ca7" />

From the above visual, we see that a total of 9,900 Ads was launched (3,300 Ads per campaign season), resulting in 14.65 million impressions and over 180,000 clicks with an average Click-through rate (CTR) of 1.23%. Wow!😃 that was an impressive social reach. Nevertheless, having spent a total of £163.25K on the Ads we needed to see how the engagement translated to conversions, which is a driver for the revenue to be gotten.

Analyzing further, we see that the over 180,000 clicks resulted in a little over 40,000 conversions, an indication that our conversion rate was about 22%.

Now the question is, How much did we make from these conversions?

The visual shows that we made 1.73 million pounds (£1.73M) with a Return on Ad-Spend (ROAS) of 10.61. That means the Overall campaign generated over £10 for every £1 spent on ads. Hmm 🙂, this was generally a profitable campaign outcome.

However, it will be beneficial to dive deeper and see a more granular information on the performance by campaigns, Channels and Ads as well as our cities and our users’ devices.

Moving to the Campaigns, we see from the image below that highest impression to conversion was recorded during Fall season. This high conversion is a likely reason for high revenue generated during the same season (September to November), as shown in our monthly trend analysis.

<img width="520" alt="image" src="https://github.com/user-attachments/assets/9e970e59-9981-49c4-bee4-4f6000594405" />

However, in terms of cost analysis, we see that despite having the highest Ad-spend and Cost per Acquisition, the fall season had the lowest Return on Ad-Spend (ROAS) across the campaign season.

Thus, our analysis shows that, the most effective Campaign was in Summer (June to August), which was characterized by:

lowest Ad-spend - Less expense on Ad campaigns
low cost-per-acquisition (CPA), indicating that the brand is acquiring customers or conversions at a relatively low cost, and
A High Return on Ad Spend (ROAS), which indicates that the revenue generated from those acquisitions is significantly higher than the cost.

So, What are the opportunities from the campaign seasons outcomes?

The Summer Campaign indicates an opportunity to scale. By scaling the ad spend, we can:- Reach more potential customers, Drive more conversions, and Increase revenue. With the low CPA , scaling the ad spend is likely to remain cost-effective, and the high ROAS indicates that the increased spend will likely lead to even moSo, Did the high engagement in London and among the mobile devices translate to conversions and revenue? 🤔 Let's find out.So, Did the high engagement in London and among the mobile devices translate to conversions and revenue? 🤔 Let's find out.re revenue growth.
Conversely, the Fall Campaign presents an opportunity to optimize – It had the highest Ad-Spend and CPA, yet with the lowest ROAS.
Similarly, The Spring Campaign also indicates an opportunity to optimize, given its moderate Ad-spend and good ROAS.

Moving on, we look at our performance/opportunities across the 3 cities and based on device usage.
<img width="344" alt="image" src="https://github.com/user-attachments/assets/39b0fe59-ad23-42be-a12d-5be37eafbb02" />


From the visual above, we see that across the cities, the highest engagement was observed in London, with highest engagement coming from mobile device users. A similar trend was also seen in Manchester and Birmingham, with mobile Engagement consistently outpacing desktop.

So, Did the high engagement in London and among the mobile devices translate to conversions and revenue? 🤔 Let's find out.
<img width="499" alt="image" src="https://github.com/user-attachments/assets/644681de-6609-4ce9-a42b-dbad66de67fd" />

From the visual above, Birmingham showed the highest clicks and conversion rate, suggesting it is highly effective at turning interest into purchases, While Manchester, despite lower engagement and click, generated the highest revenue, indicating effective campaigns. Across the devices, highest conversions and revenue came from our Desktop users, rather than Mobile. This was recorded across the channels and campaigns alike.

What should be done?

The highest engagement was observed in London → This indicates a strong interest that can be leveraged to drive more clicks and conversions by refining ad content with compelling calls to action.
Birmingham showed the highest clicks and conversion rate, suggesting it is highly effective at turning interest into purchases.
→ Allocating more budget to Birmingham could maximize conversions.
Manchester, despite lower engagement and click, generated the highest revenue, indicating effective campaigns.
→ Analyzing successful strategies in Manchester and replicating them in other locations could further enhance performance.
For the Devices:

Desktop ads outperformed mobile in terms of click-to-conversion rate and revenue. This suggests maintaining a strong focus on desktop-targeted campaigns, with high-quality, detailed ad content optimized for larger screens.

Mobile performance can be improved by ensuring ads are mobile-optimized, with responsive design and faster load times. Introducing mobile-specific promotions and enhancing the mobile user experience could boost engagement and conversions, helping to capture a broader audience effectively.







