# Analysis of Non-UK Conversions in UK-Targeted Campaigns: Insights for Geo-Targeting Optimization

## Project Overview
The primary objective of this project is to analyze the dataset and determine the factors contributing to the increasing share of non-UK conversions in UK-targeted campaigns. This will involve comparing the performance of UK and non-UK conversions. The insights gained will help inform whether adjustments are needed in the geo-targeting setup or other aspects of the campaign strategy.


## Data Sources
The provided dataset aggregates data from passengers converted through digital marketing campaigns in both the UK and Germany. The key dimensions include:

- Publisher: Identifying which publisher the campaign belongs to.
- Mobility Type: Type of fleet used (e.g., Private Hire).
- Conversion Country: The country where the conversion occurred.
- Country Code by Phone: The country code of the user based on their phone.
- Asset Country: The country where the campaign was targeted (expected to be the UK).
- Platform: The platform on which the conversion occurred (iOS or Android).
- Duration Install to Activation: The time between app install and activation.
- Conversions: The number of conversions.
- Month: Time period of the campaign.

## **Key Insights:**
- UK vs. Non-UK Conversion Trends:
    - UK conversions dropped from 83.5% to 28.2% (Jan 2022 to Sep 2023), while non-UK conversions grew from 16.5% to 71.8%, signaling geo-targeting issues and cross-border traffic.
    - Non-UK conversions may be displacing UK conversions, indicating potential audience cannibalization.

- Geo-Targeting Accuracy:
    - UK campaigns show higher mismatch rates, with publishers like PublisherS causing up to 54.6% non-UK conversions.
    - DE campaigns perform better with fewer geo-targeting issues.

- Cross-Border Traffic:
    - UK campaigns face high cross-border conversions from key publishers, with nearly 50% of conversions coming from non-UK users.

- Attribution Type Performance:
    - UK install conversions are evenly split between UK and non-UK conversions, while DE campaigns show stronger local conversion rates (80.56%).

- Mobility Type and Activation:
    - TAXI (<2h activation) drives UK conversions, while Private Hire performs better in non-UK and DE markets.


## **Takeaways:**
- Geo-Targeting Needs Refinement:
	- UK campaigns are experiencing geo-targeting issues, leading to wasted ad spend on non-UK conversions. Immediate action is required to _**review publishers' geo targeting**_, _**install attribution type**_,  **_negative keyword settings_** and Google’s Keyword Planner of the **_current trend of geo-targeted keywords_** for optimization.
- Cross-Border Traffic Monitoring:
	- Both UK and DE campaigns show notable cross-border conversion rates, particularly for specific publishers. Regular monitoring and adjustments to target audience settings are essential.
- Mobility Type-Specific Messaging:
	- Campaigns should emphasize TAXI services in the UK and Private Hire in non-UK/DE markets to align with regional preferences and drive conversion growth.


## Analysis framework
In order to find out the potential factors for non-UK conversion increase, this analysis will dive into **_Geo-Targeting Accuracy_** from the location targeting perspective and **_Cross-Border Traffic_** from target audience perspective.

How to define Geo-Targeting / Cross-Border Traffic?
- **conversion country vs. asset country mismatch:** Geo-targeting issues arise when the **_conversion_country (where the conversion happens)_** is different from the **_asset_country (where the campaign is targeted)_**.
    - Scenario 1: Although geo-targeting is set to the UK, it's possible that some conversions are coming from outside the UK.  
- **country code by phone mismatch:** A strong indicator of cross-border traffic is when the **_country_code_by_phone (user’s phone number)_** differs from the **_conversion_country_**. This implies that a user from another country completed a conversion in a different country.
    - Scenario 1: check for UK users converting outside the UK
    - Scenario 2: check for non-UK users converting outside the UK, even though the campaign is targeted at the UK



Therefore, in this case, there could be 2 conditions:

1. Condition 1 - Geo-targeting issue: **_accesst_country != conversion_coutry_** > publisher
    - Scenario 1: Although geo-targeting is set to the UK, it's possible that some conversions are coming from outside the UK. 
     - accesst_country = UK
     - conversion_country != UK


2. Condition 2 - Cross-border traffic : **_country_code_by_phone != conversion_coutry_** > publisher > target audience
- Scenario 1: checks for UK users converting outside the UK
    - accesst_country = UK
    - country_code_by_phone = UK
    - conversion_country != UK
- Scenario 2: checks for non-UK users converting outside the UK, even though the campaign is targeted at the UK
    - accesst_country = UK
    - country_code_by_phone = !UK
    - conversion_country != UK
 
  


## Exploratory Data Analysis

## Data Analysis - Trend Analysis
### **1. Showcase the UK/Non-UK conversion trend from UK targeted campaigns.**
   - Why do I want to know?
      - The campaign targets UK users, but increasing non-UK conversions could signal geo-targeting issues, platform behavior anomalies, or cross-border traffic. Month-over-month (MoM) analysis helps identify when this trend began and its severity, revealing if it's an anomaly or long-standing issue.
   - So what?
        - **Budget Inefficiency:** Marketing spend intended for UK users is being wasted on non-UK conversions, reducing overall ROI.
        - **Geo-Targeting Accuracy:** Misconfigured geo-targeting impacts UK user reach and conversions
        - **Cross-Border Traffic:** If non-UK conversions consistently exceed UK conversions, it's worth re-evaluating the campaign’s target audience and how geo-targeting is functioning.

          
### **Findings**
- **UK conversion decline:** UK conversions dropped from 83.5% in January 2022 to just 28.2% by September 2023, indicating a sharp decline in UK-based conversions over time.
- **Non-UK conversion growth:** Non-UK conversions increased steadily, growing from 16.5% in January 2022 to 71.8% by September 2023, highlighting a significant shift in audience location.
  
As non-UK conversions increased, UK conversions decreased in proportion, suggesting potential cannibalization. 
- **Cannibalization Effect:**
The inverse relationship between UK and non-UK conversions suggests potential cannibalization, where non-UK traffic is displacing UK conversions. As non-UK conversions rise, UK conversions fall, which indicates that geo-targeting or target audience may be influencing this pattern.


### **Next Steps:**

- **Review Geo-Targeting Setup:** Conduct a thorough review of geo-targeting settings across all publishers to ensure ads are focused on the UK markett, and review keyword targeting to exclude irrelevant keywords by adding them as negatives.

- **Analyze Non-UK Conversion Audience:** Examine which publishers are driving the most non-UK users, as this could indicate an unintended and unsophisticated targeting audience that doesn’t align with the campaign’s objectives.


```ruby
-- UK vs. Non-UK conversion Trend Overview
SELECT 
    month,
    SUM(CASE WHEN conversion_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversions,
    SUM(CASE WHEN conversion_country = 'UK' THEN conversions ELSE 0 END) / SUM(conversions) AS UK_Conversion_Share,
    SUM(CASE WHEN conversion_country != 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversions,
    SUM(CASE WHEN conversion_country != 'UK' THEN conversions ELSE 0 END) / SUM(conversions) AS Non_UK_Conversion_Share
FROM 'campaigns.csv'
WHERE asset_country = 'UK'
GROUP BY month
ORDER BY month ASC;
```
![截圖 2024-09-09 14 42 03](https://github.com/user-attachments/assets/dd6d6136-38b3-438c-ba17-16b67103686e)
![截圖 2024-09-09 14 41 56](https://github.com/user-attachments/assets/ca207ee8-f8ef-45c4-a131-c4d643a97ab5)


```ruby
-- explore the data in the MoM table
WITH campaign_data AS (
    SELECT 
        month,
        SUM(CASE WHEN conversion_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversions,
        ROUND(SUM(CASE WHEN conversion_country = 'UK' THEN conversions ELSE 0 END) /
                      SUM(conversions), 4) AS UK_Conversion_Share,
        SUM(CASE WHEN conversion_country != 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversions,
        ROUND(SUM(CASE WHEN conversion_country != 'UK' THEN conversions ELSE 0 END) /
                      SUM(conversions), 4) AS Non_UK_Conversion_Share
    FROM 'campaigns.csv'
    WHERE asset_country = 'UK'
    GROUP BY month
)

SELECT 
    month,
    UK_Conversions,
	-- MoM for UK Conversions
    UK_Conversions - LAG(UK_Conversions, 1) OVER (ORDER BY month) AS UK_Conversions_MoM,
    CONCAT(ROUND(UK_Conversion_Share * 100, 2), '%') AS UK_Conversion_Share,
    -- MoM for UK Conversion Share
    CONCAT(ROUND((UK_Conversion_Share - LAG(UK_Conversion_Share, 1) OVER (ORDER BY month)) * 100, 2), '%') AS UK_Conversion_Share_MoM,
    Non_UK_Conversions,
    -- MoM for Non-UK Conversions
    Non_UK_Conversions - LAG(Non_UK_Conversions, 1) OVER (ORDER BY month) AS Non_UK_Conversions_MoM,
    CONCAT(ROUND(Non_UK_Conversion_Share * 100, 2), '%') AS Non_UK_Conversion_Share,
    -- MoM for Non-UK Conversion Share
    CONCAT(ROUND((Non_UK_Conversion_Share - LAG(Non_UK_Conversion_Share, 1) OVER (ORDER BY month)) * 100, 2), '%') AS Non_UK_Conversion_Share_MoM
FROM campaign_data
ORDER BY 1;
```

![截圖 2024-09-09 18 17 42](https://github.com/user-attachments/assets/181fc125-be62-4167-a79d-567e5dac547e)

## Data Analysis: Geo-targeting accuracy
By comparing the publishers vs. with the **_conversion_country_** (where the conversion occurred) and **_asset_country_** (marketing targeted country - in this case it's uk), It allows us to detect if there is any potential geo-targeting inefficiencies.

Therefore, I'll dive into condition 1.
1. Condition 1: **_accesst_country != conversion_coutry_** > Geo-targeting issue > publisher
     - accesst_country = UK
     - conversion_country != UK


### **2.  Evaluate the performance of geo-targeting from the publishers for UK and DE.**
- Why Do I Want to Know?
    - I want to see if specific publishers are causing conversions outside the target market, which could indicate geo-targeting issues.

- So What?
    - If certain publishers are off-target, it means wasted ad spend. Identifying them lets us fix the issue or reallocate the budget to improve campaign performance.

 
### **Findings**
- **Higher UK Mismatches:** UK has consistently higher mismatched conversion rates than DE. For example, PublisherS has a 54.6% UK mismatch vs. 21.8% in DE, and PublisherT shows 52% in the UK vs. 28% in DE.
- **DE Performs Better:** DE generally shows lower mismatched rates. PublisherK has 47% UK mismatches but only 26% in DE

### **Recommendations:**
- Focus on urgent publishers with the highest mismatch rates to refine targeting strategies.

```ruby
-- how mismatches look like from publisher
SELECT 
    publisher,
    SUM(CASE WHEN asset_country = 'UK' AND conversion_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversions,
	SUM(CASE WHEN asset_country = 'UK' AND conversion_country = 'UK' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversion_Share,
    SUM(CASE WHEN asset_country = 'UK' AND conversion_country != 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversions,
	SUM(CASE WHEN asset_country = 'UK' AND conversion_country != 'UK' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversion_Share,
    SUM(CASE WHEN asset_country = 'DE' AND conversion_country = 'DE' THEN conversions ELSE 0 END) AS DE_Conversions,
	SUM(CASE WHEN asset_country = 'DE' AND conversion_country = 'DE' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'DE' THEN conversions ELSE 0 END) AS DE_Conversion_Share,
    SUM(CASE WHEN asset_country = 'DE' AND conversion_country != 'DE' THEN conversions ELSE 0 END) AS Non_DE_Conversions,
	SUM(CASE WHEN asset_country = 'DE' AND conversion_country != 'DE' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'DE' THEN conversions ELSE 0 END) AS Non_DE_Conversion_Share
FROM 'campaigns.csv'
GROUP BY publisher
ORDER BY UK_Conversions DESC;
```

![截圖 2024-09-09 18 18 17](https://github.com/user-attachments/assets/24e4e0a5-ad76-4737-b059-8e82a9f43529)





### **3.  Evaluate the conversion performance breakdown by the attribution type for UK and DE.**
- Why Do I Want to Know?
    - I want to see conversion performance in each attribution type, which could indicate geo-targeting issues.

- So What?
    - If certain attribution_type are off-target, it means wasted ad spend. Identifying them lets us fix the issue or reallocate budget to improve campaign performance.

### **Findings:**
- **UK Conversions:**
    - Install: 51.79% of conversions come from UK users, while 48.21% are non-UK, indicating that we need to require a better review for better geo-targeting.
    - Retargeting: Strong UK focus with 72.42% of conversions.
    - Reattribution: 88.40% of UK conversions highlight successful win-back efforts.

- **DE Conversions:**
    -  Install: 80.56% of conversions are local, showing highly effective acquisition campaigns.
    -  Retargeting: 90.54% of conversions come from within Germany, indicating strong re-engagement.

```ruby
-- check attribution_type for an overview
SELECT 
    attribution_type,
    SUM(CASE WHEN asset_country = 'UK' AND conversion_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversions,
	SUM(CASE WHEN asset_country = 'UK' AND conversion_country = 'UK' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversion_Share,
    SUM(CASE WHEN asset_country = 'UK' AND conversion_country != 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversions,
	SUM(CASE WHEN asset_country = 'UK' AND conversion_country != 'UK' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversion_Share,
    SUM(CASE WHEN asset_country = 'DE' AND conversion_country = 'DE' THEN conversions ELSE 0 END) AS DE_Conversions,
	SUM(CASE WHEN asset_country = 'DE' AND conversion_country = 'DE' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'DE' THEN conversions ELSE 0 END) AS DE_Conversion_Share,
    SUM(CASE WHEN asset_country = 'DE' AND conversion_country != 'DE' THEN conversions ELSE 0 END) AS Non_DE_Conversions,
	SUM(CASE WHEN asset_country = 'DE' AND conversion_country != 'DE' THEN conversions ELSE 0 END) / SUM(CASE WHEN asset_country = 'DE' THEN conversions ELSE 0 END) AS Non_DE_Conversion_Share
FROM 'campaigns.csv'
GROUP BY attribution_type
ORDER BY UK_Conversions DESC;
```

![截圖 2024-09-09 18 18 35](https://github.com/user-attachments/assets/9fd1e1cd-5c75-4234-a6c2-7eba304f64fe)

![截圖 2024-09-09 18 18 40](https://github.com/user-attachments/assets/42f387be-584e-479c-a3e1-853a5cb57ad2)


## Data Analysis: Cross-border traffic for target audience insights
By comparing the with the **_conversion_country_** (where the conversion occurred) and **_country_code_by_phone (user’s phone number)_**, it allows us to detect if there is any target audience issue.

Therefore, in this case, we'll look int 2nd conditions:
- Condition 2: **_country_code_by_phone != conversion_coutry_** > target audience > cross-border traffic
    - Scenario 1: checks for UK users converting outside the UK
        - accesst_country = UK
        - country_code_by_phone = UK
        - conversion_country != UK
    - Scenario 2: checks for non-UK users converting outside the UK, even though the campaign is targeted at the UK
        - accesst_country = UK
        - country_code_by_phone = !UK
        - conversion_country != UK

To understand the increase in Non-UK conversions despite geo-targeting being set for the UK, we can evaluate whether the publishers are targeting the correct audience. We will calculate the cross-border traffic for UK-targeted campaigns by analyzing two key metrics: 1) UK users converting outside the UK, and 2) Non-UK users converting outside the UK. This will help us assess the percentage of cross-border traffic in relation to total conversions for UK campaigns.

### **4. Publisher and Cross-Border Traffic Analysis in UK-Targeted Campaigns (with DE Comparison)**
- Why Do I Want to Know?
    - Understanding target audience misalignment in UK and DE markets helps optimize campaign performance by focusing on the right users, and improving ROI.
 
### **Findings:**
- UK Market:
    - Many publishers show high UK cross-border conversions, especially PublisherB (44.1%), PublisherS (44.6%), and PublisherG (48.9%), indicating significant misalignment in target audience settings.
    - Cross-Border Traffic: The high percentage of non-UK conversions with UK phone numbers (e.g., PublisherB, PublisherS, and PublisherG) suggests that the campaigns may not be accurately targeting the intended UK audience, leading to conversions from users outside the target region. This misalignment could be due to incorrect audience definitions or broader targeting parameters.

- DE Market:
    - PublisherB (20.6%) and PublisherG (20.8%) exhibit notable cross-border conversions, suggesting similar challenges with DE audience targeting.
    - Misaligned Audience Segmentation: The similar trend in non-DE conversions with DE phone numbers indicates that audience segmentation might not be specific enough, causing ads to reach users outside the desired regions.

### **Recommendations:**
- Refine Audience Segmentation: Ensure the target audience criteria are specific and aligned with the campaign goals for both UK and DE markets.
- Evaluate Broader Targeting Settings: Investigate if the targeting parameters are too broad, leading to irrelevant cross-border traffic.

  
```ruby
-- look into user insights from non-uk conversions with geo-tarketing in UK
WITH non_uk_conversion_users AS (
    SELECT 
        publisher,
        -- Cross-Border Traffic for UK Users Converting Outside the UK
        SUM(CASE WHEN
			country_code_by_phone = 'UK' AND conversion_country != 'UK' 
            THEN conversions ELSE 0 END) AS UK_Phone_Cross_Border_Conversions,

        -- Cross-Border Traffic for Non-UK Users Converting Outside the UK
        SUM(CASE WHEN
			country_code_by_phone != 'UK' AND conversion_country != 'UK' 
            THEN conversions ELSE 0 END) AS Non_UK_Phone_Cross_Border_Conversions,

        -- Total Conversions for UK Campaigns
        SUM(conversions) AS Total_UK_Conversions
    FROM 'campaigns.csv'
    WHERE asset_country = 'UK'
    GROUP BY publisher
),

non_de_conversion_users AS (
    SELECT 
        publisher,
        -- Cross-Border Traffic for DE Users Converting Outside Germany
        SUM(CASE WHEN
			country_code_by_phone = 'DE' AND conversion_country != 'DE' 
            THEN conversions ELSE 0 END) AS DE_Phone_Cross_Border_Conversions,

        -- Cross-Border Traffic for Non-DE Users Converting Outside Germany
        SUM(CASE WHEN
			country_code_by_phone != 'DE' AND conversion_country != 'DE' 
            THEN conversions ELSE 0 END) AS Non_DE_Phone_Cross_Border_Conversions,

        -- Total Conversions for DE Campaigns
        SUM(conversions) AS Total_DE_Conversions
    FROM 'campaigns.csv'
    WHERE asset_country = 'DE'
    GROUP BY publisher
)

-- Join UK and DE CTEs
SELECT 
    COALESCE(non_uk_conversion_users.publisher, non_de_conversion_users.publisher) AS publisher,

    -- UK Metrics
    non_uk_conversion_users.UK_Phone_Cross_Border_Conversions,
    non_uk_conversion_users.Non_UK_Phone_Cross_Border_Conversions,
    non_uk_conversion_users.Total_UK_Conversions,
    -- UK Cross-Border Percentage
    (non_uk_conversion_users.UK_Phone_Cross_Border_Conversions / non_uk_conversion_users.Total_UK_Conversions) AS UK_Phone_Cross_Border_Percentage,
    (non_uk_conversion_users.Non_UK_Phone_Cross_Border_Conversions / non_uk_conversion_users.Total_UK_Conversions) AS Non_UK_Phone_Cross_Border_Percentage,

    -- DE Metrics
    non_de_conversion_users.DE_Phone_Cross_Border_Conversions,
    non_de_conversion_users.Non_DE_Phone_Cross_Border_Conversions,
    non_de_conversion_users.Total_DE_Conversions,
    -- DE Cross-Border Percentage
    (non_de_conversion_users.DE_Phone_Cross_Border_Conversions / non_de_conversion_users.Total_DE_Conversions) AS DE_Phone_Cross_Border_Percentage,
    (non_de_conversion_users.Non_DE_Phone_Cross_Border_Conversions / non_de_conversion_users.Total_DE_Conversions) AS Non_DE_Phone_Cross_Border_Percentage

FROM non_uk_conversion_users 
LEFT JOIN non_de_conversion_users
ON non_uk_conversion_users.publisher = non_de_conversion_users.publisher
ORDER BY non_uk_conversion_users.publisher;
```

![截圖 2024-09-09 18 19 15](https://github.com/user-attachments/assets/0a4a32e1-8310-43d1-addb-7e162081e0fa)




## Data Analysis: UK conversion behavioral insights

### **5.  Evaluate mobility type and activation time to identify key drivers for potential UK/DE conversion growth.**
- Why Do I Want to Know?
    - It helps optimize strategies for increasing UK conversions and improving campaign effectiveness.
 
### **Findings**
- **TAXI (<2h activation)** is the top driver for UK conversions, while **Private Hire** sees stronger for Non-UK and DE/Non-DE conversions.

### **Recommendations**
- **Adjust Geo-Targeted messaging**: Tailor messaging to emphasize the mobility types that perform best in each region. For UK campaigns, highlight the immediate availability of taxis, while for DE/Non-DE campaigns, focus on promoting private hire services to align with user preferences.

```ruby
SELECT 
    mobility_type,
    duration_install_to_activation,

    -- UK Conversions (campaign targeted at UK, conversion in UK)
    SUM(CASE WHEN asset_country = 'UK' AND conversion_country = 'UK' THEN conversions ELSE 0 END) AS UK_Conversions,

    -- Non-UK Conversions (campaign targeted at UK, conversion outside UK)
    SUM(CASE WHEN asset_country = 'UK' AND conversion_country != 'UK' THEN conversions ELSE 0 END) AS Non_UK_Conversions,

    -- Germany Conversions (campaign targeted at Germany, conversion in Germany)
    SUM(CASE WHEN asset_country = 'DE' AND conversion_country = 'DE' THEN conversions ELSE 0 END) AS DE_Conversions,

    -- Non-Germany Conversions (campaign targeted at Germany, conversion outside Germany)
    SUM(CASE WHEN asset_country = 'DE' AND conversion_country != 'DE' THEN conversions ELSE 0 END) AS Non_DE_Conversions

FROM 'campaigns.csv'
GROUP BY 1,2
ORDER BY UK_Conversions DESC;
```

![截圖 2024-09-09 18 19 25](https://github.com/user-attachments/assets/25e7cd50-423a-4d55-9517-7c59d3de8332)



## **Conclusion:**
- Geo-Targeting and Audience Targeting Challenges:
	- The data indicates a significant drop in UK conversions, while non-UK conversions have sharply increased. This shift could be linked to geo-targeting issues, particularly with publishers like PublisherS and PublisherT, where a high percentage of conversions are happening outside the UK. Adjusting geo-targeting settings and reassessing audience segmentation might help reduce cross-border traffic and refocus the campaigns on the intended UK audience.

- Mobility Type and Attribution Findings:
	- TAXI services with fast activation times appear to be driving most UK conversions, while Private Hire correlates more with non-UK and DE conversions. The imbalance in install campaign performance could indicate a need for better geo-targeting, while retargeting and reattribution efforts seem effective in the UK. Tailoring messages based on mobility type and monitoring cross-border traffic more closely could improve campaign performance and ROI.

## **Limitations:**
- Lack of Phone Country Code Data:
	- Around 20% of users lack phone country code information, which might limit our ability to accurately track cross-border traffic. If this data were available, it could help clarify whether the conversion discrepancies are due to geo-targeting issues or user behavior patterns.

- Need for More Detailed Campaign and User Data:
	- Having more granular data on user demographics, campaign performance (e.g., which ads and placements are most effective), and behavioral insights (e.g., time and location of conversions) could provide deeper insights. This additional data might help explain underperforming campaigns and lead to more targeted engagement strategies.
