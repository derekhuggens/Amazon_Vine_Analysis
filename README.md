# Amazon_Vine_Analysis
ETL w/ PySpark/Pandas/SQL

## Overview of the analysis: Explain the purpose of this analysis.

The Amazon reviews landing page was accessed at https://s3.amazonaws.com/amazon-reviews-pds/tsv/index.txt and a random dataset was chosen for an Amazon Vine/Non-Vine Review Bias analysis.

I chose: https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Video_Games_v1_00.tsv.gz US REVIEWS DATASET

Using Google Colab notebooks and the PySpark library, the above Amazon S3 Cloud Object was read in as a .csv and stored in a DataFrame with the following data columns:

DATA COLUMNS:<br>
marketplace       - 2 letter country code of the marketplace where the review was written.<br>
customer_id       - Random identifier that can be used to aggregate reviews written by a single author.<br>
review_id         - The unique ID of the review.<br>
product_id        - The unique Product ID the review pertains to. In the multilingual dataset the reviews<br>
                    for the same product in different countries can be grouped by the same product_id.<br>
product_parent    - Random identifier that can be used to aggregate reviews for the same product.<br>
product_title     - Title of the product.<br>
product_category  - Broad product category that can be used to group reviews <br>
                    (also used to group the dataset into coherent parts).<br>
star_rating       - The 1-5 star rating of the review.<br>
helpful_votes     - Number of helpful votes.<br>
total_votes       - Number of total votes the review received.<br>
vine              - Review was written as part of the Vine program.<br>
verified_purchase - The review is on a verified purchase.<br>
review_headline   - The title of the review.<br>
review_body       - The review text.<br>
review_date       - The date the review was written.<br>
<br>


The columns of interest for our Amazon Vine/Non-Vine Review Bias analysis were selected with spark and made into a new DataFrame, `vine_df`.

![vine_df](https://github.com/derekhuggens/Amazon_Vine_Analysis/blob/8097059b39f139605edc98d5c764f3ca9e58d16c/README_IMAGES/vine_df.png)<br><br>

From `vine_df`, filters were applied to select all reviews that had at least 20 or more total votes and was made into a new DF, `df1`.

From `df1`, `df2` was made with an additional filter wherein all rows where helpful_votes divided by total_votes >= 0.5 were retrieved.

From `df2`, two different DFs were made, one contained Vine eligible reviews, and the other contained non-Vine reviews.<br>

![filters_df](https://github.com/derekhuggens/Amazon_Vine_Analysis/blob/8097059b39f139605edc98d5c764f3ca9e58d16c/README_IMAGES/df_filters.png)<br><br>

## Results: Please see the below image for Amazon review calculations.

![calculation_dfs](https://github.com/derekhuggens/Amazon_Vine_Analysis/blob/8097059b39f139605edc98d5c764f3ca9e58d16c/README_IMAGES/df_calculations.png)<br>

### How many Vine reviews were there? 94

### How many non-Vine reviews were there? 40,471

### How many Vine reviews were 5 stars? 48

### How many non-Vine reviews were 5 stars? 15663

### What percentage of Vine reviews were 5 stars? 51.06%

### What percentage of non-Vine reviews were 5 stars? 38.70% <br><br>

## Summary: Provide one additional analysis that you could do with the dataset to support your statement.

By using PySpark to specifically select data and filter it, we were able to see bias correlations from calculated ratios. There was a positivity bias for Amazon Vine reviewers in regards to their video game reviews, possibly because of funding or observer bias. From the Results section above, one can see that the Vine Five-Star Percentage ratio was 51.06% of all Vine reviews whereas the Non-Vine Five-Star Percentage ratio was 38.70% amongst all Non-Vine reviews. Using a z-test, we could compare the Vine review ratings mean with the mean rating of the Non-Vine reviews to see if there is a statistical difference. A t-test would be better with a smaller sample size, however the Vine review count was 94 and the Non-Vine review count was 40,471. With the Vine bias ratio seen above as 51.06%, a z-test would help see how many standard deviations a sample (Vine reviews) mean is above (positive) or below (negative) from the mean population (Non-Vine reviews).
