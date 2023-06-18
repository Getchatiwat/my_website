---
categories:  
- ""    #the front matter should be like the one found in, e.g., blog2.md. It cannot be like the normal Rmd we used
- ""
date: "2021-09-30"
description: Flight Operation Analysis # the title that will show up once someone gets to this page
draft: false
image: flights.jpg # save picture in \static\img\blogs. Acceptable formats= jpg, jpeg, or png . Your iPhone pics wont work

keywords: ""
slug: flight_oper # slug is the shorthand URL address... no spaces plz
title: Flight Operation Analysis
---





{r}
#| label: load-libraries
#| echo: false # This option disables the printing of code (only output is displayed).
#| message: false
#| warning: false

library(tidyverse)
library(wbstats)
library(skimr)
library(countrycode)
library(here)
library(dplyr)
library(ggplot2)

Data Visualisation - Exploration

Now that you've demonstrated your software is setup, and you have the basics of data manipulation, the goal of this assignment is to practice transforming, visualising, and exploring data.

Mass shootings in the US

In July 2012, in the aftermath of a mass shooting in a movie theater in Aurora, Colorado, Mother Jones published a report on mass shootings in the United States since 1982. Importantly, they provided the underlying data set as an open-source database for anyone interested in studying and understanding this criminal behavior.

Obtain the data

{r}
#| echo: false
#| message: false
#| warning: false


mass_shootings <- read_csv(here::here("data", "mass_shootings.csv"))

glimpse(mass_shootings)

column(variable)

description

case

short name of incident

year, month, day

year, month, day in which the shooting occurred

location

city and state where the shooting occcurred

summary

brief description of the incident

fatalities

Number of fatalities in the incident, excluding the shooter

injured

Number of injured, non-fatal victims in the incident, excluding the shooter

total_victims

number of total victims in the incident, excluding the shooter

location_type

generic location in which the shooting took place

male

logical value, indicating whether the shooter was male

age_of_shooter

age of the shooter when the incident occured

race

race of the shooter

prior_mental_illness

did the shooter show evidence of mental illness prior to the incident?

Explore the data

Specific questions

Generate a data frame that summarizes the number of mass shootings per year.

{r}
mass_shootings %>% 
  group_by(year) %>% 
  summarise(count=n())

Generate a bar chart that identifies the number of mass shooters associated with each race category. The bars should be sorted from highest to lowest and each bar should show its number.

{r}
mass_shootings %>% 
  
  # Group race
  group_by(race) %>% 
  
  # Count #of mass shooters in each race
  summarise(count=n()) %>%
  
  # Plot Bar chart by order x-axis(race) from max count begins first
  ggplot(aes(x=reorder(race,-count),y=count)) +
  geom_col() +
  geom_text(aes(label=count)) +
  labs(x="Race",y="No. of Mass Shooters") +
  ggtitle("The number of Mass shooters associated with each race category")

Generate a boxplot visualizing the number of total victims, by type of location.

{r}
mass_shootings %>% 
  
  # Group by location
  group_by(location_type) %>% 

  # Plot bar chart
  ggplot(aes(x=location_type,y=total_victims))+
  geom_boxplot()+
  labs(x="Location Types",y="Total Victims")+
  ggtitle("Total victims by Location types")

Redraw the same plot, but remove the Las Vegas Strip massacre from the dataset.

{r}
mass_shootings %>% 
  
  # Group by location
  group_by(location_type) %>% 
  
  # Filter out "Las Vegas Strip Massacre"
  filter(case != "Las Vegas Strip massacre") %>% 
  
  # Plot bar chart
  ggplot(aes(x=location_type,y=total_victims))+
  geom_boxplot()+
  labs(x="Location Types",y="Total Victims")+
  ggtitle("Total victims by Location types")

More open-ended questions

Address the following questions. Generate appropriate figures/tables to support your conclusions.

How many white males with prior signs of mental illness initiated a mass shooting after 2000?

{r}
mass_shootings %>% 
  
  # Filter white males with prior signs of mental illness inititated after 2000
  filter(race=="White" & male =="TRUE" & year > 2000 & prior_mental_illness == "Yes") %>% 
  summarise(count=n())

#ANS: 22 men

Which month of the year has the most mass shootings? Generate a bar chart sorted in chronological (natural) order (Jan-Feb-Mar- etc) to provide evidence of your answer.

{r}
mass_shootings %>% 
 
  # Group month
  group_by(month) %>% 
  
  # Sum mass shooting for each month
  summarise(count=n()) %>% 
 
  # Order month 
  mutate(month = factor(month, levels = month.abb)) %>% 
  
  # Plot Bar Chart
  ggplot(aes(x=month,y=count))+
  geom_col() +
  labs(x="Month",y="No. of Mass shootings") +
  ggtitle("Number of mass shootings by month")

How does the distribution of mass shooting fatalities differ between White and Black shooters? What about White and Latino shooters?

{r}
mass_shootings %>% 
  
  # Filter only race is white or black
  filter(race %in% c("White","Black")) %>% 
  
  # Group race and year and summarise each of it
  group_by(race,year) %>% 
  summarise(sum=sum(fatalities)) %>% 
  
  # Plot line chart
  ggplot(aes(x=year,y=sum,color=race))+
    geom_line()+
    labs(x="Year",y="No. of mass fatalities") + 
    ggtitle("Distribution of mass shooting fatalities between White and Black")


mass_shootings %>% 
  
  # Filter only race is white or Latino
  filter(race %in% c("White","Latino")) %>% 
  
  # Group race and year and summarise each of it
  group_by(race,year) %>% 
  summarise(sum=sum(fatalities)) %>% 
  
  # Plot line chart
  ggplot(aes(x=year,y=sum,color=race))+
    geom_line()+
    labs(x="Year",y="No. of mass fatalities") + 
    ggtitle("Distribution of mass shooting fatalities between White and Latino")

Very open-ended

Are mass shootings with shooters suffering from mental illness different from mass shootings with no signs of mental illness in the shooter?

{r}
mass_shootings %>% 
  group_by(prior_mental_illness) %>% 
  summarise(sum_victims = sum(total_victims), sum_injured=sum(injured), sum_fatalities=sum(fatalities))

#ANS: Mass shootings with shooters suffering from mental illness has more injury people (120 injuries) and fatalities (503 death) more than non-mental illness cases (81 injuries and 120 death) 

Assess the relationship between mental illness and total victims, mental illness and location type, and the intersection of all three variables.

{r}
mass_shootings %>% 
  
  # Group by mental illness and summarise total victims in each type
  group_by(prior_mental_illness) %>% 
  summarise(sum_victims = sum(total_victims)) %>% 
  
  # Plot bar chart
  ggplot(aes(x=prior_mental_illness,y=sum_victims)) +
  geom_col()+
  labs(x="Is mental illness?", y="Total Victims") +
  ggtitle("Relationship between Mental illness and Total victims")


mass_shootings %>% 
  # Filter out empty data of prior mental illness
  filter(!is.na(prior_mental_illness)) %>% 
  
  # Group by mental illness and summarise total victims in each type
  group_by(prior_mental_illness,location_type) %>% 
  summarise(sum_victims_by_location = sum(total_victims)) %>% 
  
  # Plot bar chart
  ggplot(aes(x=location_type,y=sum_victims_by_location)) +
  geom_col()+
  labs(x="Is mental illness?", y="Total Victims") +
  ggtitle("Relationship between Mental illness and Total victims") + 
  # Facet graph by prior mental illness
  facet_wrap(~prior_mental_illness)

Make sure to provide a couple of sentences of written interpretation of your tables/figures. Graphs and tables alone will not be sufficient to answer this question.

Exploring credit card fraud

We will be using a dataset with credit card transactions containing legitimate and fraud transactions. Fraud is typically well below 1% of all transactions, so a naive model that predicts that all transactions are legitimate and not fraudulent would have an accuracy of well over 99%– pretty good, no? (well, not quite as we will see later in the course)

You can read more on credit card fraud on Credit Card Fraud Detection Using Weighted Support Vector Machine

The dataset we will use consists of credit card transactions and it includes information about each transaction including customer details, the merchant and category of purchase, and whether or not the transaction was a fraud.

Obtain the data

The dataset is too large to be hosted on Canvas or Github, so please download it from dropbox https://www.dropbox.com/sh/q1yk8mmnbbrzavl/AAAxzRtIhag9Nc_hODafGV2ka?dl=0 and save it in your dsb repo, under the data folder

{r}
#| echo: false
#| message: false
#| warning: false

card_fraud <- read_csv(here::here("data", "card_fraud.csv"))

glimpse(card_fraud)

The data dictionary is as follows

column(variable)

description

trans_date_trans_time

Transaction DateTime

trans_year

Transaction year

category

category of merchant

amt

amount of transaction

city

City of card holder

state

State of card holder

lat

Latitude location of purchase

long

Longitude location of purchase

city_pop

card holder's city population

job

job of card holder

dob

date of birth of card holder

merch_lat

Latitude Location of Merchant

merch_long

Longitude Location of Merchant

is_fraud

Whether Transaction is Fraud (1) or Not (0)

In this dataset, how likely are fraudulent transactions? Generate a table that summarizes the number and frequency of fraudulent transactions per year.

{r}
fraud_cases_by_year <- card_fraud %>% 
  
  # Filter only fraud cases
  filter(is_fraud == "1") %>% 
  
  # Group by year and summarise fraud case in each year
  group_by(trans_year) %>% 
  summarise(count_fraud_cases = n())

fraud_cases_by_year

How much money (in US$ terms) are fraudulent transactions costing the company? Generate a table that summarizes the total amount of legitimate and fraudulent transactions per year and calculate the % of fraudulent transactions, in US$ terms.

{r}
summary <- card_fraud %>% 
  
  # Group by fraud cases and summarise total amount of each case
  group_by(trans_year,is_fraud) %>% 
  summarise(sum_amt = sum(amt)) %>% 
  
  # Find % of each transaction type in US$ terms
  mutate (percent_fraud = 100*(sum_amt/sum(sum_amt,trans_year = trans_year)))

summary

Generate a histogram that shows the distribution of amounts charged to credit card, both for legitimate and fraudulent accounts. Also, for both types of transactions, calculate some quick summary statistics.

{r}
card_fraud %>% 
  
  # Group by fraud cases and amount and summarise total amount of each case
  group_by(is_fraud,amt) %>% 
  summarise(count = n()) %>% 
  
  # Plot histogram and splited facet by whether is fraud or not
  ggplot(aes(x=amt)) +
  geom_histogram() +
  labs(x= "Amount charged", y= "# of amount charged") +
  ggtitle("Distribution of amounts charged to credit card") +
  facet_wrap((~is_fraud)) 

  # Find no.of cases, summary of amount, mean, median, and sd for fraud and not fraud cases
card_fraud %>% 
  group_by(is_fraud) %>% 
  summarise(count_cases = n(), sum_amount = sum(amt), mean_amount = mean(amt), median_amount = median(amt), sd_amount = sd(amt))

What types of purchases are most likely to be instances of fraud? Consider category of merchants and produce a bar chart that shows % of total fraudulent transactions sorted in order.

{r}
card_fraud %>% 

# Group by category and count total case for each category
  group_by(category) %>% 
  summarise(count = n()) %>% 
  
  # Find % of each category's cases per total cases
  mutate(percent = (count/sum(count))*100) %>% 
  arrange(desc(percent)) %>% 
  
  # Plot bar chart by order of percentage
  ggplot(aes(x=reorder(category,-percent),y=percent)) +
  geom_col()+
  labs(x="Category",y="% of total fraudulent transactions") +
  ggtitle ("% of total fraudulent transactions by category")

When is fraud more prevalent? Which days, months, hours? To create new variables to help you in your analysis, we use the lubridate package and the following code

mutate(
  date_only = lubridate::date(trans_date_trans_time),
  month_name = lubridate::month(trans_date_trans_time, label=TRUE),
  hour = lubridate::hour(trans_date_trans_time),
  weekday = lubridate::wday(trans_date_trans_time, label = TRUE)
  )

Are older customers significantly more likely to be victims of credit card fraud? To calculate a customer's age, we use the lubridate package and the following code

  mutate(
   age = interval(dob, trans_date_trans_time) / years(1),
    )

{r}
new_card_fraud <- card_fraud %>% 
 
  # Change date/month/hour/weekday format
  mutate(
  date_only = lubridate::date(trans_date_trans_time),
  month_name = lubridate::month(trans_date_trans_time, label=TRUE),
  hour = lubridate::hour(trans_date_trans_time),
  weekday = lubridate::wday(trans_date_trans_time, label = TRUE)) %>% 
  
  # Calculate age
  mutate(age = interval(dob, trans_date_trans_time) / years(1),) 

# View no. of fraud cases by year
new_card_fraud %>% 
  group_by(trans_year) %>% 
  summarise(count = n())
#ANS: 2019 has far more fraud cases than 2020's

# View no. of fraud cases by month
new_card_fraud %>% 
  group_by(month_name) %>% 
  summarise(count = n()) %>% 
  arrange(desc(count))
#ANS: There are different amount of cases for each month, but no trend can be obseved from the data. However, Mar and May have the most fraud cases.

# View no. of fraud cases by date
new_card_fraud %>% 
  group_by(date_only) %>% 
  summarise(count = n())

# View no. of fraud cases by hour
new_card_fraud %>% 
  group_by(hour) %>% 
  summarise(count = n()) %>% 
  arrange(desc(count))
#ANS: fraud acses are tended to occur at night more than day. The most cases are occured at hour 22.

# View no. of fraud cases by weekday
new_card_fraud %>% 
  group_by(weekday) %>% 
  summarise(count = n()) %>% 
  arrange(desc(count))
#ANS: fraud acses are tended to occur on weekend more than weekday except for Monday, which has the most fraud cases

# View no. of fraud cases by age
new_card_fraud %>% 
  group_by(age) %>% 
  summarise(count = n())
#ANS: older people seems to not be victims of fraud cases

Is fraud related to distance? The distance between a card holder's home and the location of the transaction can be a feature that is related to fraud. To calculate distance, we need the latidue/longitude of card holders's home and the latitude/longitude of the transaction, and we will use the Haversine formula to calculate distance. I adapted code to calculate distance between two points on earth which you can find below

{r}
# distance between card holder's home and transaction
# code adapted from https://www.geeksforgeeks.org/program-distance-two-points-earth/amp/


fraud <- card_fraud %>%
  mutate(
    
    # convert latitude/longitude to radians
    lat1_radians = lat / 57.29577951,
    lat2_radians = merch_lat / 57.29577951,
    long1_radians = long / 57.29577951,
    long2_radians = merch_long / 57.29577951,
    
    # calculate distance in miles
    distance_miles = 3963.0 * acos((sin(lat1_radians) * sin(lat2_radians)) + cos(lat1_radians) * cos(lat2_radians) * cos(long2_radians - long1_radians)),

    # calculate distance in km
    distance_km = 6377.830272 * acos((sin(lat1_radians) * sin(lat2_radians)) + cos(lat1_radians) * cos(lat2_radians) * cos(long2_radians - long1_radians))

  ) %>% 

  # Filter only fraud cases
  filter(is_fraud == "1") %>% 
  
  # Plot boxplot
  ggplot(aes(x= is_fraud,y=distance_km)) +
  geom_boxplot() +
  ggtitle("Distance between card holder's home and transaction")

fraud

#ANS: fraud cases seems to relate to the distance sincemost cases are within 50-100km


Plot a boxplot or a violin plot that looks at the relationship of distance and is_fraud. Does distance seem to be a useful feature in explaining fraud?

Exploring sources of electricity production, CO2 emissions, and GDP per capita.

There are many sources of data on how countries generate their electricity and their CO2 emissions. I would like you to create three graphs:

1. A stacked area chart that shows how your own country generated its electricity since 2000.

You will use

geom_area(colour="grey90", alpha = 0.5, position = "fill")

2. A scatter plot that looks at how CO2 per capita and GDP per capita are related

3. A scatter plot that looks at how electricity usage (kWh) per capita/day GDP per capita are related

We will get energy data from the Our World in Data website, and CO2 and GDP per capita emissions from the World Bank, using the wbstatspackage.

{r}
#| message: false
#| warning: false

# Download electricity data
url <- "https://nyc3.digitaloceanspaces.com/owid-public/data/energy/owid-energy-data.csv"

energy <- read_csv(url) %>% 
  filter(year >= 1990) %>% 
  drop_na(iso_code) %>% 
  select(1:3,
         biofuel = biofuel_electricity,
         coal = coal_electricity,
         gas = gas_electricity,
         hydro = hydro_electricity,
         nuclear = nuclear_electricity,
         oil = oil_electricity,
         other_renewable = other_renewable_exc_biofuel_electricity,
         solar = solar_electricity,
         wind = wind_electricity, 
         electricity_demand,
         electricity_generation,
         net_elec_imports,	# Net electricity imports, measured in terawatt-hours
         energy_per_capita,	# Primary energy consumption per capita, measured in kilowatt-hours	Calculated by Our World in Data based on BP Statistical Review of World Energy and EIA International Energy Data
         energy_per_gdp,	# Energy consumption per unit of GDP. This is measured in kilowatt-hours per 2011 international-$.
         per_capita_electricity, #	Electricity generation per capita, measured in kilowatt-hours
  ) 

# Download data for C02 emissions per capita https://data.worldbank.org/indicator/EN.ATM.CO2E.PC
co2_percap <- wb_data(country = "countries_only", 
                      indicator = "EN.ATM.CO2E.PC", 
                      start_date = 1990, 
                      end_date = 2022,
                      return_wide=FALSE) %>% 
  filter(!is.na(value)) %>% 
  #drop unwanted variables
  select(-c(unit, obs_status, footnote, last_updated)) %>% 
  rename(year = date,
         co2percap = value)


# Download data for GDP per capita  https://data.worldbank.org/indicator/NY.GDP.PCAP.PP.KD
gdp_percap <- wb_data(country = "countries_only", 
                      indicator = "NY.GDP.PCAP.PP.KD", 
                      start_date = 1990, 
                      end_date = 2022,
                      return_wide=FALSE) %>% 
  filter(!is.na(value)) %>% 
  #drop unwanted variables
  select(-c(unit, obs_status, footnote, last_updated)) %>% 
  rename(year = date,
         GDPpercap = value)

#Q1: A stacked area chart that shows how your own country generated its electricity since 2000.
energy %>%
  
  # Filter country Thailand that the info is not empty
  filter(country == "Thailand" & !is.na(electricity_generation)) %>% 
  
  # Plot a stacked area chart
  ggplot(aes(x=year,y=electricity_generation)) +
  geom_area() +
  labs(x="Years",y="Electricity Generation") +
  ggtitle("Electricity Generation in Thailand over years")

#ANS: the generated electricity is increaseing over years


#Q2: A scatter plot that looks at how CO2 per capita and GDP per capita are related
gdp_percap %>% 
  
   # Merge CO2 and GDP data
  left_join(co2_percap,by = c("iso3c","year")) %>% 
  
  # Plot a chart
  ggplot(aes(x=GDPpercap,y=co2percap)) + 
  geom_point() + 
  labs(x="GDP per capita",y="CO2 per capita") +
  ggtitle("Relationship between GDP per capita and CO2 per capita")

#ANS: the more GDP, the more CO2 released


#Q3: A scatter plot that looks at how electricity usage (kWh) per capita/day GDP per capita are related 
energy %>% 
  
  # Merge Energy and GDP data
  mutate(iso3c = iso_code) %>% 
  left_join(gdp_percap,by = c("iso3c","year")) %>% 
  
  # Plot a chart
  ggplot(aes(x=GDPpercap,y=per_capita_electricity)) + 
  geom_point() + 
  labs(x="GDP per capita",y="Electricity usage(kWh) per capita") +
  ggtitle("Relationship between GDP per capita and CO2 per capita")

# Specific Questions
#1 How would you turn energy to long, tidy format?
energy %>% 
  pivot_longer(cols = 4:18, names_to = "Data", values_to = "Value")

#3 Write a function that takes as input any country's name and returns all three graphs. You can use the patchwork package to arrange the three graphs as shown below

generate_energy_charts <- function(country) {
   country_energy <- energy %>% 
    filter(country == {{country}})
  
# Plot an electricity generation chart
  electricity_production_chart <- country_energy %>% 
    ggplot(aes(x = year, y = electricity_generation)) +
    geom_area() +
    labs(title = "Electricity Production Mix")
  
# Plot a chart for CO2 per capita and GDP per capita
  gdp_co2_percap_charts <- co2_percap %>% 
    left_join(gdp_percap, by = "iso3c") %>% 
    filter(country == {{country}}) %>% 
    ggplot(aes(x = co2percap, y = GDPpercap)) +
    geom_point() + 
    labs(title = "Relationship between CO2 per capita and GDP per capita")
  
# Plot a chart for electricity usage per capita and GDP per capita
  energy_gdp_percap_charts <- energy_gdp_percap %>% 
    mutate(iso3c = iso_code) %>% 
    left_join(gdp_percap, by = "iso3c") %>% 
    filter(country == {{country}}) %>% 
    ggplot(aes(x = per_capita_electricity, y = GDPpercap)) +
    geom_point() + 
    labs(title = "Relationship between Electricity Usage and GDP per capita")
  
#Return
return(list(electricity_production_chart, gdp_co2_percap_charts, energy_gdp_percap_charts))
}


Specific questions:

How would you turn energy to long, tidy format?

You may need to join these data frames

Use left_join from dplyr to join the tables

To complete the merge, you need a unique key to match observations between the data frames. Country names may not be consistent among the three dataframes, so please use the 3-digit ISO code for each country

An aside: There is a great package called countrycode that helps solve the problem of inconsistent country names (Is it UK? United Kingdon? Great Britain?). countrycode() takes as an input a country's name in a specific format and outputs it using whatever format you specify.

Write a function that takes as input any country's name and returns all three graphs. You can use the patchwork package to arrange the three graphs as shown below
