# udc_cap
Analysing coffee shop customers reaction and engagement with offers, and predicting future offer completions. 

Please find the related blog post here: 

# 1. Installations

Libraries installed:
- pandas = 1.3.0
- numpy = 1.18.5
- matplotlib.pyplot = 3.2.2
- json = 2.0.9
- sklearn = 0.23.1 

Python version = 3.8.3

# 2. Project Motivation

The motivation behind this project was to explore synthetic data for Starbucks customers, relating to their reactions and activity surrounding promotional offers as part of the Starbucks Rewards programme. The aim was to see if we could predict wether a customer would complete an offer based on a variety of features. 

# 3. File Descriptions

The data is across three json files which are imported into the notebook as Pandas DataFrames:

portfolio.json: contains offer ids and relevant metadata for each offer
profile.json: contains demographic data for each customer all of whom are part of the Starbucks Rewards scheme
transcript.json: records for transactions, offers received, offers viewed, and offers completed

(The files cannot be uploaded to GitHub as they are too large)

# 4. Technical details

**Data Cleaning**
Initally I looked to better understand the 3 dataframes we were working with: portfolio, profile and transcript. 
The value column in the transcript dataframe was a dictionary so this was split out so we had separate columns for offer id, amount and reward.
We then cleaned up the data. Two columns for offer id now existed, these were investigated and it was found there was no time were both offer ids were null, so we were able to merge these columns by filling the null values in 'offer_id' column with the correlated value in 'offer id' column, then dropping 'offer id' since all this data was now contained in the 'offer_id' column. 
2175 rows with null values for gender and income were discovered. These were dropped to allow us to perform data exploration around how gender and income influence behaviour. 
We attempted to outline outliers but none were found - this could be due to the data being synthetic but it is always worth checking for outliers. 
Then we merged the portfolio and transcript datasets to be used later to see which type of offer performs best.
Additionally we merged all 3 datasets to be used for modelling later. 

**Data Exploration and Visualisation**
Exploring the data we found that 5 members never received offers, so these were dropped from our exploration. 

We created 2 sub dataframes - for those who receive and view offers and those who receive but never view offers. We found that 0.98% of all members who receive offers don't ever view them. We investigated the age and income characteristics of viewers vs. non-viewers and found no significant difference between the two groups or in comparison to all rewards members. 
We repeated the process of creating 2 sub dataframes this time for those who complete offers and those who do not. We found 80.88% of all members who receive offers complete at least 1. Again we investigated the age and income characteristics. Those who don't complete offers have a much lower average income and lower average age than both the general population and also those who do complete offers. 

Then we used matplotlib to plot 2 pie charts to show gender distrubtion amongst those who complete offers, vs. all members. Relative to breakdown of all members, males were slightly less likely to complete offers than females.

Next we investigated the number of times an offer was viewed in comparison to the number of times it was completed and plotted this as a scatter plot using matplotlib. 
One offer was completed more than it was viewed, so we investigated this. The difficulty of this offer was just 5 whereas the average across the offers is 7.7 so this could be why it was being completed so easily without customers even being aware of it.
The offer which had the greatest difference between number of viewings and completions (completions being significantly lower) had a difficulty of 10, which is the maximum difficult rating, and was valid for just 5 days which does not give users a lot of time to use the offer.

Then we wrote a function called perc_finder that found the percentage of each offer type that was viewed and completed. We found discount offers have the greatest conversion rate from being viewed to being completed. 
Using the information from perc_finder we used a scatter plot to plot the percentage of offers sent out that were viewed and completed for bogo offers and discount offers and found that whilst more bogo offers were viewed than discount offers less were actually completed suggesting discount offers are more attractive or easier to attain. 

**Modelling**
Finally we came to modelling. First we had to create dummy variables for the 'event', 'offer_id' and 'offer_type' columns so we could get these string values into a numeric form to use in the models. 
We removed whitespace from column names and dropped duplicate columns. 
Then we split our data into X and y, where y was our target variable of the 'offer_completed' dummy column.

We defined a function display_results which allows us to display the accuracy and f1score for a model. We chose to measure these metrics as accuracy is easy to interpret. But we also wanted to use the f1 score as it takes account of how data is distributed. 

Then we defined random_forest_classifer and created a RandomForestClassifier model using X and y, by first splitting into train and test datasets, then fitting the model, predicting on the X_test set and using display_results to observe the accuracy and f1 score. We repeated this process for the Naive Bayes classifier too. We found both models had an accuracy and f1 score of 1.0, this suggests perfect scores which we should be skeptical of. A perfect score is most likely evidence of overfitting.  

# 5. Licensing, Authors, Acknowledgements, etc.
Acknowledgements: Completed as part of a Udacity Nanodegree programme, thank you to Udacity for the resources and links to the data.
Data providers: The datasets are provided by Starbucks, and are synthetic data simulated for use in the project. 
