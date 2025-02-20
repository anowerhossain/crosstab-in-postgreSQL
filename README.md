# crosstab-in-postgreSQL

`crosstab()` is a table function from the tablefunc extension in PostgreSQL that pivots rows into columns, transforming normalized data into a tabular (pivoted) format.

✅ Transforms rows into columns → Makes data more readable.

✅ Great for reporting dashboards → Pivoting helps in analytics.

✅ Works with dynamic data → New categories can be auto-added.

A digital marketing team is running multiple campaigns across various social media platforms. They track daily ad clicks to measure performance.

We’ll use PostgreSQL to:
✅ Store campaign data
✅ Generate a standard report
✅ Pivot the data using crosstab
✅ Automatically update the pivot table when new platforms are added

## Step 1️⃣: Creating a Dataset

```sql
CREATE TABLE ad_clicks (
    campaign VARCHAR(100),
    platform VARCHAR(50),
    date DATE,
    clicks INT
);
```

```sql
INSERT INTO ad_clicks (campaign, platform, date, clicks) VALUES
    ('Black Friday Sale', 'Facebook', '2024-02-01', 1200),
    ('Black Friday Sale', 'Instagram', '2024-02-01', 950),
    ('Black Friday Sale', 'Google Ads', '2024-02-01', 1800),
    
    ('Summer Deals', 'Facebook', '2024-02-01', 850),
    ('Summer Deals', 'YouTube', '2024-02-01', 700),

    ('Christmas Offers', 'Instagram', '2024-02-02', 1200),
    ('Christmas Offers', 'LinkedIn', '2024-02-02', 350),
    
    ('New Year Promo', 'Facebook', '2024-02-02', 1050),
    ('New Year Promo', 'Twitter', '2024-02-02', 600),
    
    ('Valentine Special', 'TikTok', '2024-02-03', 1300),
    ('Valentine Special', 'Google Ads', '2024-02-03', 950);
```

We can now generate a standard report grouping clicks by campaign and platform.


💡 Issue: This report is readable, but if we want to compare platform performance per campaign, it’s not ideal.

## Step 3️⃣: Using crosstab to Pivot Data

To make the report easier to analyze, we’ll pivot the data so each platform becomes a column.
```sql
CREATE EXTENSION IF NOT EXISTS tablefunc;
```

```sql
SELECT * FROM crosstab(
    'SELECT campaign, platform, SUM(clicks) FROM ad_clicks GROUP BY campaign, platform ORDER BY campaign',
    'SELECT DISTINCT platform FROM ad_clicks ORDER BY platform'
) 
AS pivot_table (campaign TEXT, 
                Facebook INT, 
                Google_Ads INT, 
                Instagram INT, 
                LinkedIn INT, 
                TikTok INT, 
                Twitter INT, 
                YouTube INT);
```
