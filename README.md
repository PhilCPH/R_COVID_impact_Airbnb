# R_COVID_impact_Airbnb

In this project, the COVID-shock on the Airbnb-rental market is measured and possible scenarios are forecasted. The main question that needed to be answered is 

### How has the COVID-19 pandemic aected the Airbnb-market in Amsterdam and what are its implications for the future?

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

The three variables price, availability and occupancy are endogenous variables, meaning that they interact with each other in the model.

Summary:
This project investigated the  COVID-19 pandemic on the Airbnb rental market in Amsterdam. For that, data sets of the website InsideAirbnb were separately downloaded and merged. Development in demand, price and availability were subject to time series analysis. The results were used to visualize the external shock's influence on the three components. A downwards trend for price and demand were present, however the business model of Airbnb seems to be quite resilient towards external shocks with a short recovery phase especially for rental prices. In case a vaccine is permitted for distribution and travel restrictions are lifted, the market will likely recover to its former performance.
