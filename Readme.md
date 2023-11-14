Business Problem						
	"XYZ is an American bookseller. It is a Fortune 1000 company and the bookseller with the largest number of retail outlets in the United States. It also sells books through various ecommerce channels. They sell around 3684 unique books in their different stores among which there are  4 top selling books on the basis of customer reviews. So XYZ has approached Bridgei2i Analytics Solutions to help them plan their growth strategy.
"					
						
						
						
						
						
						
						
Questions						
						
	1)	XYZ wants to understand which books have most positive reviews among the whole set (They are expecting ranking for the top 4 books)				
	2)	XYZ bookseller wants to plan demand for the top shelf books for the upcoming 2 months across all the 21 states (given in data) of US				
	3)	XYZ also wants Bridgei2i to determine the profile of customers who have given the most positive reviews				




### Question 1:
-  Preprocessing:
    -  Word tokenization
    -  Removed punctuations
    -  Lemmatization

-  Approach:

Using the TextBlob package, I got the reviews for all the books, and converted the scores to 0 and 1, if the reviews Ire negative and positive respectively. 
Since I are concentrating on the most positive reviews for the books, this was done so that when I sum up the score, it will be a direct reflection of the scores of the positive reviews. 
I then summed up the scores across book level, to get overall scores for individual books.
But to remove the bias in the data, I normalized these scores, by dividing the scores of the books by the number of reviews each book had in the data. 
Using this approach, I Ire able to arrive at the top 4 books with positive reviews.

### Question 2:
-  Preprocessing:
Combined all the holiday variables into one feature called ‘isHoliday’ by summing up the holidays for every Bookcode, State, and Time Point.

-  Initial approach:
Firstly, I tried to identify the time series components such as trend, seasonality, and residuals by observing the seasonal decompose plots and periodic dickey fuller tests(to check for stationarity) after normal and seasonal differencing.
Then, I plotted the ACF and PACF plots to derive the orders (AR, MA coefficients) and the seasonality order if present.
Based on the observed orders and including the exogenous variables I build a model for the train-test split and compared the AIC and BIC scores without exogenous variables. The model building was done for all the combinations of exogenous variable inclusions and the loIst AIC/BIC score was observed for the one with exogenous variable as ‘isHoliday’ 
After obtaining the best model, I used the same for deriving our forecasts for the test data
-  Final approach:
The process of checking for time series components,differencing and obtaining the orders was automated using the `pmdarima` package which is the python equivalent of `auto arima` by R.
The remaining processes Ire the same as above

### Question 3:
The first approach that came into mind was taking the mean or median of age grouping it by the top book codes. When I tried grouping the data by BookCode and taking the mean and median of age both Ire almost equal. This provided an inference that the data doesn’t have outliers.
When I tried plotting the age I saw that most of the customers are around the age range of 30 to 60. So they are the primary customers for all the BookCodes.
The next approach I took was to build a balanced DecisionTreeClassifier with age variable as X and the top 4 BookCodes as Y. The output that I expected was to get the binned ages in the tree with BookCodes in the leaf so that I will be able to provide the age range for each of the BookCode. But this approach did not make sense because there was no clear distinction betIen the age range for each BookCode and the approach is complicated for a single variable. 
Therefore I tried manual binning of the age variable to get the count of readers in each range for each book to get more ideas. Initially, I binned the age variable into 10 bins and got the count of customers in each age group for each BookCode. But as mentioned above most of the customers fell in the primary age group of 32 to 60. About 80% of the customers fell in the same age range.
Finally, I binned the customers into 3 bins. As expected 34 to 57 age group bins had 80% of the customers. So I did a deep dive within that age range for each Bookgroup. There I found that BookCode 84987 and 22720 had most customers within the age range of (35 to 42) and (50 to 57), 22979 had most customers within (35 to 50) and 22722 had most customers within (35 to 57). 	
