# R_COVID_impact_Airbnb

In this project, the COVID-shock on the Airbnb-rental market is measured and possible scenarios are forecasted. The main question that needed to be answered is 

### How has the COVID-19 pandemic affected the Airbnb-market in Amsterdam and what are its implications for the future?

The bigger picture of this project was to define how resilient Airbnb is towards unforeseeable events in its market environment. In order to analyze that, this project focuses on the economic framework of supply and demand by looking at three variables: the price of a listing, its occupancy, and availability. While price is self-explanatory, occupancy
(the number of bookings per month for example) represents the demand side, while availability (the number of days per month a host makes their location available) represents
the supply side.

This theory of supply and demand states that if demand exceeds supply, the price for a particular product or service increases and vice versa (OpenStax, 2016). Therefore,
one assumption related to the study's research question is that a decrease in bookings on Airbnb's website due to global travel restrictions will result in an oversupply of available accommodation and thus cause listing prices to go down.

Next, the step-by-step process is defined 

![Step-by-step image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Plan.PNG)

## Used datasets

### Measurements

The first dataset contained measurements against COVID-19. This dataset was provided by the WHO and was susbsequently filtered to provide a picture of the local actions taken against the pandemic.

![Mesaurements image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Measurements.PNG)

These measures will be cross-checked in order to provide explanations for drops or increases in the data.

### COVID-19 data

This dataset was provided by the governmental health department of the Netherlands. It contains total cases, hospital admissions and deaths related to COVID-19 per municipality code, as well as the municipality name and province for the time frame between 13.03.2020 and 14.12.2020.

![Cases image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Total%20COVID%20cases.PNG)

### Airbnb data

The data taken directly from InsideAirbnb were taken from three different source files: Calendar, Listings and Reviews. The calendar data provides information regarding the availability of each listing, as well as its price per day and restrictions on minimum and maximum length of stay. The Listing data includes important qualitative information, e.g. whether the host has a superhost status or in which neighbourhood the respective apartment is located in and if the listing has been available in the last 365 days. The review data is shortened to contain only the listing-id and date of review in order to get the number of reviews per listing and month.

## Interdependency of variables

The three variables price, availability and occupancy are endogenous variables, meaning that they interact with each other in the model. ordinary least squares (OLS) regression was used to determine underlying influences on price and bookings. 

A relationship between price and 
1. the status of superhost, 
2. the neighbourhood, 
3. the room type, 
4. the number of people it accommodates,
5. minimum nights, and 
6. Months 
was established.

## Calculating demand

The demand/occupancy rate is not readily available in the dataset. So, it was calculated over the number of reviews left for each listing. But not every customer leaves a review, so the question is: How many customers leave a review? Airbnbs co-founder Mr. Chesky sets the review rate at 72%, the Budget and Legislative Analyst's Office calculated it at 30.5% but does not account for missing reviews in deleted listings. It was chosen to take the middle-way at 50%, which is also taken by InsideAribnb.

## Data Pre-Processing

Due to the datasets size (8GB), it needed to be reduced in complexity. Unneeded columns were deleted, necessary column names were changed and character string were transformed into a correct format. The Calendar data was set as the basis for merges. Outliers with crazy price increases (Airbnb housing next to a F1-circuit for 90.000 € a night) were excluded. To prepare for the Decision Tree regression, the location of each listing was allocated to one of their eight boroughs in Amsterdam.

# Data Analytics

## Exploratory Analysis

The current outlook is rather grim with Amsterdam following the European trend of an increase in cases since fall 2020. Therefore, the model will include another demand shock in the near future. As the data is regularly scraped and comes in continuous time intervals, the structure allows itself best for a Time Series Analysis. The three underlying variables are interconnected which makes VAR together with the impulse response function an obvious choice. However, the newest data already includes the effect of the COVID-19 shock, which might make this model prone to errors. Therefore, another analysis will be conducted using the Autoregressive integrated moving average (ARIMA) model. Besides the Time series analysis, a regression using Decision tree was also added, in order to catch possible interaction between the three variables and three other factors: the number of new COVID cases, the locations of the listing, the seasonality.

### Regression
As the model includes categorical as well as numerical data input, the OLS is an obvious choice. Another advantage is that regression does not require normally distributed data as input.

### VAR / ARIMA
ARIMA is a statistical model for analyzing and forecasting time series data and is also suitable for providing a visual representation of the development of variables over time. VAR is the more elaborated TS model further allows the model multi-variate interdependencies. Furthermore, it allows to impose shocks onto the data by using a Impulse Response Function (IRF). For the first scenario, no further shock is expected and a successful vaccination of the population is assumed. The second scenario expects a second shock and a successfull roll-out of the vaccine.

### Decision Tree
Primarily, it is used to describe determinants of changes in the price variable using the availability and demand variables. According to the pre-processing, the listings for each borough were as follows

![Listings image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Listings.PNG)

Westpoort did not contain listings due to it being an industrial borough with mainly warehouses and company grounds.

## Explanatory Analysis

### Price
The average prices from 2019 to 2020 difer slighlty. Whereas the mean price in 2019 was about 162.64€ per night, it dropped in 2020 by 1€ to 162.3€ per night. The absolute change seems to be insignificant, but the development of the price in time shows an ongoing downwards trend which increased in speed and entropy during the COVID-19 pandemic.

![Price image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Price%20over%20time.PNG)

While I'm unsure of the actual reason for this drop, a possible external explanation might be that the Amsterdam municipality started introducing stricter regulations rules on Airbnb: maximum limit of renting going from 60 days a year to 30, limit of only four guest per stay, and obligatory registration with city council. Another explanation, from the data files, might be the 1.800.000 listing ids that were scrapped in 2018 due to not being recorded properly.

The superhost status has a significant influence on the price (p < :00). The estimator is ß = 1.205e^-01. Exponential transformation yields e^ß = 1.13, hence the price is on average 13% higher if the host is a superhost. The same reasoning applies for the room type: As the base level of this factor is the entire room-apartment, the private room is on average e^ß = 0.69, hence 31% cheaper.

Interestingly enough, WHO measures restricting traveling (including the actual travel ban or obligations like quarantine and negative test results upon entry), the increase in number of cases, nor their interaction effect had a significant impact on the price. The
only estimator which is marginally significant (p < .1) are WHO measurements with a percentage increase of 2%. However, this might be due to random rather than systematic effects.

A increase in prices in the inner city is represented by a higher accumalition of yellow points in the heatmap.

![Heatmap image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Price%20Heatmap.PNG)

### Demand

Demand experienced a hard drop during the "hot" phase of COVID-19. It becomes obvious that during the lockdown which lasted until
early June, as well as after August when new travel restrictions were imposed, demand dropped. However, the graph also makes it clear that demand is very likely to regain its old levels as soon as the country opens for tourism again.

![Demand image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Demand%20over%20time.PNG)

Interestingly, the demand for all other room types compared to entire apartments is higher. Also the number of minimum nights has a significant, but marginal impact on demand. Each additional night of minimum required nights reduces the amount of bookings by 1%. Again, due to multicollinearity, this variable was excluded and will be analysed again in the time series analysis.

In fact, rising numbers in infections have a significant effect on demand (p < 0.01), even stronger during phases of lockdown, the interaction effect WHOxCOVID is present at a level of p < 0.02. For each additional case a day during lockdown, the demand decreases by 0.05%.

### Availability

The concept of Airbnb evolves around the idea that people rent their private places to tourists at times when they do not need their
own property. Free choice is not implementable into the model. Underlying structural patterns like an increase in demand, might motivate home owners to rent their place out more, but this cannot be captured by an OLS regression.

![Availability image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Availablity%20over%20time.PNG)

It becomes clear that as COVID-19 cases rise, availability also increases, meaning that the average number of days a home was available has continuously risen throughout 2020. Nevertheless it remains difficult to understand the underlying pattern of the data. A possible explanation could be that many home owners, e.g. students, went back to live together with their family and set the at to be available throughout the entire time they were not home.

## Exploratory Analysis

As mentioned before, the three independent variables of analysis are linearly correlated.

![Correlation image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Correlation%20graph.PNG)

### Arima

The ARIMA model enables the description and analysis of time series. Due to the noisiness of the data at hand it was unfortunately not possible to polish the data set to a satisfactory level. Therefore, a different approach was taken with regards to the exploratory analysis and to extend the model's complexity. 

The analysis started with the ARIMA on a global level, meaning aggregated average data was used without distinguishing between different categories (e.g. a specific neighbourhood). For price, an ARIMA with 1st order Auto regression was identified (ARIMA(1,0,0)). The augmented Dickey-Fuller test (ADF) revealed that the data indeed is not stationary and determined a lag order of 3, thus evoking the need to implement stationarity in the auto.arima function. The Ljung-Box test was used to run diagnostics on the analysis and yielded a p-value of p = 0:3681.

For demand, stationarity was also not given and the lag order was 3 again. The resulting ARIMA was of 2nd order Auto regression. Diagnostics were again done with the Ljung-Box test which yielded a p-value of p = 0:5144. Lastly, availability had the same structural pre-requisites, the lag order was again 3. I identified an ARIMA with 1st order Auto regression (ARIMA(1,0,0), the Ljung-Box
test resulted in a p-value of p = 0:2934. The rather low p-values of the Box-Ljung test already reveal that modeling will not
yield reliable results. Nevertheless, a prediction of the three variables for the upcoming six months was executed.

![Forecast ARIMA image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/ARIMA.PNG)

It can be deducted from the data, that, without further shocks, price and demand will slowly regain their old levels. 95% Confidence Interval (CI) are substantial in all cases to account for the underlying uncertainty provided by the noise in the data. One can nicely
depict the counter-movements of demand and price to availability. What is interesting here is that even though demand and price are negatively correlated the model predicts both of them to rise when only looking at the isolated effects.

### VAR

Next, the VAR is implemented to account for the relationship between price, demand and availability. It depicts the concurrent and counter-movements of the respective variables.

![TSP image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Time%20Series%20Plot.PNG)

In accordance to ARIMA, diagnostics revealed that there is indeed no stationarity and that the data has a trend. The data was differentiated once according to the recommended number of differentiation required for stationary analysis. From there on, multiple problems occured. The Akaike Information Critera (AIC) suggested 6 legs, but the resulting value would have been -Ind, indicating a overfitting of the model. Hence, 5 legs were used in order to minimize the AIC. Whereas the diagnostics for serial testing and
normality testing yielded acceptable results, the roots were unfortunately very high, starting from 0.98. This still complies with the rule of thumb to be smaller than 1, meaning that the VAR is at least stable in a sense that shocks reduce over time. However, any
interpretations with regards to demand need to be made with caution.

![IRF image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Impulse%20Response%20VAR.PNG)

The cumulative IRF models a positive demand shock. This could be the case e.g. if a vaccination gets approved or the country releasing their travel-ban What makes it difficult for the VAR to properly estimate the results of a shock is that the underlying data with which the VAR was modeled with was already exposed twice to shocks in demand. However, one can also argue that especially that inherent
characteristic makes it appropriate to model another COVID-19 wave which might hit the Netherlands after Christmas and New Year's Eve. Price responds with a sharp reaction in the opposite direction for two lags, contradicting the findings of the ARIMA, until it changes direction again.

Lastly, it will be interesting to look at the forecast of the VAR model and compare it to the forecast of the ARIMA.

![FC image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Forecast%20VAR.PNG)

Without the externally imposed shock, the forecasts move in similar directions. Demand will slowly rise, together with a slight increase in average price per night. Availability will fluctuate but remain on a similar level.

### Decision Tree

Finally an attempt at modelling variation in Price was made using a regression tree. One of the advantages of regression trees over regular regression comes when subsets of data have different data generating process, which is assumed here with the shocks caused by
COVID on the market. Two models were constructed to try to look at the determinant of price. As described earlier the first one takes five variables: availability, demand, location, room type, new cases of COVID. In the second one, the month of the year was added in
order to try to capture possible seasonal effects. For both, the whole data set was divided into a training and testing set, with the conventional 70/30 split.

When the models are fed to R, it was observed that for both, the relevant variables appeared to be room type, availability, and location in that order of importance. One problem of the model being that 61% of the data set population ends up in one end leaf. This implies that not enough determining variables were added to the model.

Finally, looking at the prediction efficiency of the model in regards to the training and the testing data sets again proves the limitation of the model: Here we see that the predicted prices form horizontal lines. The model is too general in its classification to really hold any predictive power.

![Tree image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Regression%20Tree%20Splits.PNG)

![Tree Accuracy image](https://raw.githubusercontent.com/PhilCPH/R_COVID_impact_Airbnb/main/pics/Accuracy%20of%20Regression%20Tree.PNG)

Contradicting prior research, which suggests that the longer an external shock takes place the longer it will take for the organization to recover, Airbnb proved to be more resilient. The demand for the listings only experiences a sharp drop when strict measures are in place, however as soon as restrictions are lifted the number of listings recover swiftly. This suggests that Airbnb might continue to struggle whilst the pandemic is continuing. Nevertheless, the future prospects for the company post COVID- 19 are optimistic. This could be attributed to the fact that many travellers might see Airbnb as a safer alternative to more crowded hotels and therefore their preferred choice of accommodation.

Due to the vast amount of data that needed to be combined in order to conduct this project, the limits in computational
power of personal computers really showed. Thus, an amazon web server was set up to run some data heavy parts of the code.

### Summary: 
First, the VAR is arguably the best model as it allows shock implementation and forecasting at the same time. Especially insightful is the comparison of shock data to forecast prediction: a shock in demand leads to short-term changes in price, but the cumulative effect will even out to zero again. Hence, the rent remains, on a long term basis, rather stable after shock situations. Furthermore, the
VAR model appeared to be the most stable of the three and the most efficient as the ARIMA models' Ljung test proved that result might not be too reliable and the decision tree's predicting power appear to be too superficial. Forecasting future demand, price development and availability given regular travel conditions revealed that demand and price may actually counteract economic intuition as both are expected to rise. This may stem from the fact that the formerly endogenous variable of demand was externalized by the Dutch government's decision to impose strict travel bans. Market mechanisms will probably work again as soon as demand has recovered to its former level. Overall, the preceding study succeeded in capturing the impact of the global coronavirus pandemic on Airbnb's market in Amsterdam and revealed a largely negative impact of case numbers on the three variables. Nevertheless, the company's strong resilience to external shocks in its market environment make it seem likely that as soon as the global travel restrictions are lifted again, Airbnb can continue its success story.
