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


### Summary:
This project investigated the  COVID-19 pandemic on the Airbnb rental market in Amsterdam. For that, data sets of the website InsideAirbnb were separately downloaded and merged. Development in demand, price and availability were subject to time series analysis. The results were used to visualize the external shock's influence on the three components. A downwards trend for price and demand were present, however the business model of Airbnb seems to be quite resilient towards external shocks with a short recovery phase especially for rental prices. In case a vaccine is permitted for distribution and travel restrictions are lifted, the market will likely recover to its former performance.
