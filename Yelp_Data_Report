
Section 1: Data Background Analysis
This report analyzes a series of tables from the Yelp database. Yelp is an American application that provides restaurant ratings, similar to China's Dianping. The database consists of six tables, which contain information about merchants, business hours, check-ins, users, reviews, and tips. The data spans from 2004 to 2017, and all merchants are located in the United States, including businesses that have closed. The data provides details such as location, type of business, business hours, and annual ratings for the merchants. For users, the platform primarily displays information from reviews and user ratings. There is also a table dedicated to reviews, including the review content, rating, and the merchants and users associated with each review.

Section 2: Data Interpretation
The Yelp database consists of six tables: yelp_business, yelp_business_hours, yelp_checkin, yelp_review, yelp_tip, and yelp_user. Each table contains the following data:

yelp_business: Contains 174,567 entries and describes basic merchant information.
yelp_business_hours: Contains 174,567 entries and describes business hours for each merchant.
yelp_checkin: Contains 146,350 entries and describes merchant check-ins.
yelp_review: Contains 5,261,668 entries and describes individual reviews.
yelp_tip: Contains 1,098,324 entries and describes tip information.
yelp_user: Contains 1,326,100 entries and describes user profiles.

Section 3: Research Questions
To analyze the sales situation and provide improvement suggestions for future sales, the data was analyzed from the following three perspectives:
1. From the Customer's Perspective:
   - What is the average number of reviews per month for regular users since account creation, and for elite users? What are the counts and ratios of useful, funny, and cool reviews?
   - Are reviews that receive more "useful" votes generally more positive or negative? What are the characteristics of the review length, and how do funny and cool reviews compare?
   - What is the average user review score, and how are restaurants rated based on user scores?
   - What is the relationship between the number of reviews, photos, and the number of fans? What is the relationship between the number of fans and elite users?
   - Which cities are most frequently reviewed by users?

2. From the Restaurant's Perspective:
   - How many restaurants have each star rating? What is the average rating of restaurants? How do star ratings relate to the number of reviews, the average score of reviews, and the ratio of positive to negative reviews?
   - Which states have the most restaurants? Which states have the highest proportion of five-star restaurants? Which states have the highest positive review rates, and how does this correlate with geography?
   - What is the relationship between a restaurant’s star rating and the number of days it has been open?
   - What are the characteristics of restaurants that have closed, in terms of days open and star rating?

3. From the Platform's Perspective:
   - What are the yearly and monthly numbers of registered users and reviews?
   - How many elite users are there each year, and what is the proportion?
   - What is the yearly retention rate for users, and for elite users?

Section 4: Data Preprocessing
Before performing data analysis, the data must first be cleaned. Here is an overview of how each table was handled:
1. yelp_business
   - Filtered outliers using the following code:
     ```SQL
     SELECT * FROM yelp_business
     WHERE name IS NULL OR state IS NULL OR stars < 0 OR stars > 5;
     ```
     No results were returned, indicating that all values are valid.

   - Split the table into "currently operating businesses" and "closed businesses" using the following code:
     ```SQL
     SELECT * FROM yelp_business WHERE is_open=1;
     SELECT * FROM yelp_business WHERE is_open=0;
     ```

2. yelp_business_hours
   - Removed businesses that are closed all seven days of the week and those without business hour data using the following code:
     ```SQL
     SELECT * FROM yelp_business_hours WHERE monday <> "None" OR tuesday <> "None" OR wednesday <> "None" OR thursday <> "None" OR friday <> "None" OR saturday <> "None" OR sunday <> "None";
     ```

3. yelp_checkins
   - Filtered outliers using the following code:
     ```SQL
     SELECT * FROM yelp_checkin WHERE weekday IS NULL OR hour IS NULL OR checkins IS NULL;
     ```

4. yelp_review
   - Filtered outliers using the following code:
     ```SQL
     SELECT * FROM yelp_review WHERE review_id IS NULL OR business_id IS NULL OR user_id IS NULL OR date IS NULL OR stars < 0 OR stars > 5;
     ```

5. yelp_tip
   - Filtered outliers using the following code:
     ```SQL
     SELECT * FROM yelp_tip WHERE text IS NULL OR business_id IS NULL OR user_id IS NULL OR date IS NULL OR likes < 0;
     ```

6. yelp_user
   - Filtered outliers using the following code:
     ```SQL
     SELECT * FROM yelp_user WHERE useful < 0 OR funny < 0 OR cool < 0 OR fans < 0 OR average_stars < 0 OR average_stars > 5;
     ```


Section 5: Data Analysis
1. From the Customer's Perspective:
(1) What is the average number of reviews per month for regular users and elite users since account creation? How many times and what proportion of reviews were considered useful, funny, or cool?

First, find the final date of data collection. In this case, the latest registration date is used to represent this:
```SQL
SELECT DISTINCT(yelping_since) FROM yelp_user ORDER BY yelping_since DESC LIMIT 1;
```
The result shows that the data collection ended on December 11, 2017.

To calculate the registration time and average daily reviews for non-elite users, the following code is used:
```SQL
SELECT user_id, DATEDIFF("2017-12-11", yelping_since) as register_time, (review_count / DATEDIFF("2017-12-11", yelping_since)) as avg_review_day
FROM yelp_user WHERE elite = "None";
```
The average number of reviews per day for non-elite users is 0.00959.

The following code calculates the percentage of reviews that received useful, funny, and cool votes:
```SQL
SELECT SUM(review_count) as total_review_count, SUM(useful) / SUM(review_count) as useful_rate_ne, SUM(funny) / SUM(review_count) as funny_rate_ne, SUM(cool) / SUM(review_count) as cool_rate_ne FROM yelp_user WHERE elite = "None";
```
The useful, funny, and cool votes account for 0.59, 0.21, and 0.21 times the total number of reviews, respectively.

Using the same method for elite users, the following query calculates the average number of reviews per day:
```SQL
SELECT AVG(sub.avg_review_day) as avg_review_day_elite FROM (SELECT user_id, DATEDIFF("2017-12-11", yelping_since) as register_time, (review_count / DATEDIFF("2017-12-11", yelping_since)) as avg_review_day FROM yelp_user WHERE elite <> "None") as sub;
```
Elite users post 0.0979 reviews per day on average, about 10.2 times more than non-elite users.

The ratio of useful, funny, and cool votes for elite users is calculated as follows:
```SQL
SELECT SUM(review_count) as total_review_count, SUM(useful) / SUM(review_count) as useful_rate_e, SUM(funny) / SUM(review_count) as funny_rate_e, SUM(cool) / SUM(review_count) as cool_rate_e FROM yelp_user WHERE elite <> "None";
```
The useful, funny, and cool votes account for 2.10, 1.14, and 1.63 times the total number of reviews for elite users, respectively.

(2) Are reviews with high useful counts more likely to be positive or negative? What are the characteristics of their length, and how do funny and cool reviews compare?

The following query calculates the number of reviews with more than 1000 useful votes, their average star rating, and the average length of these reviews:
```SQL
SELECT COUNT(sub.text) as review_count, ROUND(AVG(sub.stars), 1) as avg_stars, ROUND(AVG(sub.len), 0) as avg_length FROM (SELECT stars, text, LENGTH(text) as len, useful FROM yelp_review WHERE useful > 1000) as sub;
```
The average star rating of reviews with more than 1000 useful votes is 1.4, and their average length is 2204 words.

(3) What is the average score of user reviews? How are restaurant star ratings distributed across different user review scores?

The following query calculates the average business star rating for each unique review star rating:
```SQL
SELECT yelp_review.stars, ROUND(AVG(yelp_business.stars), 2) as avg_busi_star FROM yelp_review LEFT JOIN yelp_business ON yelp_review.business_id = yelp_business.business_id GROUP BY yelp_review.stars ORDER BY yelp_review.stars DESC;
```

(4) What is the relationship between the number of reviews, photos, and the number of fans? What is the relationship between the number of fans and elite users?

The relationship between elite users' review count, photo count, and fan count is calculated as follows:
```SQL
SELECT SUM(fans), SUM(review_count), SUM(compliment_photos), SUM(review_count)/SUM(fans) as fans_re_rate_e, SUM(compliment_photos)/SUM(fans) as fans_ph_rate_e FROM yelp_user WHERE elite <> "None";
```

2. From the Restaurant's Perspective:
(1) How many restaurants have each star rating, and what is the average star rating? What is the relationship between a restaurant’s star rating and the number of reviews, average review score, and the ratio of positive to negative reviews?

The average star rating of all restaurants is calculated using the following query:
```SQL
SELECT ROUND(AVG(stars), 1) FROM yelp_business;
```

(2) Which states have the most restaurants? Which states have the highest proportion of five-star restaurants, and which states have the highest positive review rates?

The number of restaurants per state is calculated as follows:
```SQL
SELECT state, COUNT(state) FROM yelp_business GROUP BY state ORDER BY COUNT(state) DESC;
```

(3) What is the relationship between a restaurant’s star rating and the number of days it has been open?

The number of open days per week for each restaurant is calculated using the following query:
```SQL
WITH open_days AS (SELECT business_id, ((CASE WHEN monday = 'None' THEN 0 ELSE 1 END) + (CASE WHEN tuesday = 'None' THEN 0 ELSE 1 END) + (CASE WHEN wednesday = 'None' THEN 0 ELSE 1 END) + (CASE WHEN thursday = 'None' THEN 0 ELSE 1 END) + (CASE WHEN friday = 'None' THEN 0 ELSE 1 END) + (CASE WHEN saturday = 'None' THEN 0 ELSE 1 END) + (CASE WHEN sunday = 'None' THEN 0 ELSE 1 END)) AS open_days FROM yelp_business_hours);
```

(4) What are the characteristics of closed restaurants in terms of days open and star rating?

The average number of open days and average star rating for closed restaurants is calculated as follows:
```SQL
SELECT ROUND(AVG(open_days.open_days), 1) as avg_open, ROUND(AVG(yelp_business.stars), 2) as avg_stars FROM yelp_business LEFT JOIN open_days ON yelp_business.business_id = open_days.business_id WHERE yelp_business.is_open = 0;
```

3. From the Platform's Perspective:
(1) What are the yearly and monthly numbers of registered users and reviews?

The number of users registered each year is calculated as follows:
```SQL
SELECT user_id, YEAR(yelping_since) as year_since, COUNT(user_id) FROM yelp_user GROUP BY YEAR(yelping_since) ORDER BY YEAR(yelping_since) ASC;
```

(2) How many elite users are there each year, and what is the proportion?

The number of elite users each year is calculated using the following query:
```SQL
SELECT user_id, elite FROM yelp_user WHERE elite <> 'None';
```

(3) What is the yearly retention rate for users, and for elite users?

The retention rate of all users is calculated by analyzing the number of reviews posted by each user per year:
```SQL
SELECT user_id, SUM(CASE WHEN YEAR(yelp_review.date) = 2008 THEN 1 ELSE 0 END) as 2008_exist, SUM(CASE WHEN YEAR(yelp_review.date) = 2009 THEN 1 ELSE 0 END) as 2009_exist... FROM yelp_review GROUP BY user_id;
```

Section 6: Conclusion
1. From the User's Perspective:
Users aiming to gain elite status should focus on posting more reviews and photos. Reviews should be informative, and users should aim to provide valuable information rather than focusing solely on language style.

For regular users, restaurant ratings generally align with user scores. However, five-star ratings might have some bias, and user feedback should be carefully considered.

2. From the Merchant's Perspective:
Businesses seeking more customer traffic might consider expanding in states like Nevada, Arizona, North Carolina, Ohio, and Pennsylvania. Achieving five-star status requires focusing on both positive reviews and operational quality.

3. From the Platform's Perspective:
The platform retains a high level of user engagement, but the decline in new user registrations indicates a need for strategies to attract new users, such as summer promotions or expanding into new regions and countries.


