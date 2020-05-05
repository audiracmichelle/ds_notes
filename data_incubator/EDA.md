It is clear that lockdown has mitigated the spread of the 2019 novel
coronavirus, but there is no certainty by how much. During the first
wave of Covid-19 there was an immediate need for models to determine the
potential extent of the spread with stay at home orders in place. Now
that we are leaving the first peak behind us, the priority of policy
makers and modelers in the US has shifted towards quantifying the effect
of lifting restrictions to maintain the epidemic’s second wave under
control. Understanding the anatomy of mobility in each US county and
measuring the real (causal) effect of lockdown and other interventions
on Covid-19 spread is key in guiding quick responses and identifying
their risk while reopening the economy during this pandemic and others
that might come.

    library(tidyverse)

    ## ── Attaching packages ──────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   1.0.0     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ─────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

    library(magrittr)

    ## 
    ## Attaching package: 'magrittr'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     set_names

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     extract

    library(feather)

Covid deaths per county
=======================

Data pulled from New York Times and Johns Hopkins github repositories
provide:

-   up to date covid case and death counts in each US county
-   county descriptive data
-   effective dates for local governments’ policies and interventions
    put in place to contain the epidemic

<!-- -->

    county_features <- read_feather("./county_features.feather")

A county is considered in the analysis once the number of COVID-19
deaths reaches a a threshold of 3 deaths per 10,000,000 people.
`days_since_thesh` records the number of days elapsed between the
threshold and a given timestamp.

Let’s find the number of county records for each value of
`days_since_thresh`.

    county_features %>% 
      ggplot(aes(x = days_since_thresh)) +
      geom_histogram()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 345 rows containing non-finite values (stat_bin).

![](EDA_files/figure-markdown_strict/unnamed-chunk-3-1.png)

For each county, what are the number of available records to date?

    county_features %>% 
      group_by(fips) %>% 
      summarize(days_since_thresh = n()) %>% 
      ggplot(aes(x = days_since_thresh)) +
      geom_histogram()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](EDA_files/figure-markdown_strict/unnamed-chunk-4-1.png)

What is the mean death rate per reported case for all US counties given
the number of days since the threshold was reached?

    county_features %>% 
      group_by(days_since_thresh) %>% 
      summarise(mean_death_case_rate = mean(cum_deaths / cum_cases)) %>% 
      ggplot(aes(x = days_since_thresh, y = mean_death_case_rate)) + 
      geom_line()

    ## Warning: Removed 1 rows containing missing values (geom_path).

![](EDA_files/figure-markdown_strict/unnamed-chunk-5-1.png)

Population Density and Deaths
-----------------------------

Because it is intuitive to believe that population density has an effect
on the number of the deaths ocurring per sq mile in a given county, we
will explore that relationship.

    county_features %>% 
      filter(days_since_thresh == 17) %>% 
      select(county, state, deaths_per_sq_mi, popdensity) %>% 
      arrange(desc(deaths_per_sq_mi))

    ## # A tibble: 1,203 x 4
    ##    county        state         deaths_per_sq_mi popdensity
    ##    <chr>         <chr>                    <dbl>      <dbl>
    ##  1 New York City New York                7.97       69468.
    ##  2 Hudson        New Jersey              0.671      13731.
    ##  3 Union         New Jersey              0.194       5216.
    ##  4 Nassau        New York                0.151       4705.
    ##  5 Philadelphia  Pennsylvania            0.149      11380.
    ##  6 Essex         New Jersey              0.127       6212.
    ##  7 Middlesex     New Jersey              0.126       2622.
    ##  8 Westchester   New York                0.0906      2205.
    ##  9 Orleans       Louisiana               0.0885      2029.
    ## 10 Suffolk       Massachusetts           0.0688     12416.
    ## # … with 1,193 more rows

The plot gives some evidence that there is a linear (log) relationship.

    county_features %>% 
      filter(days_since_thresh %in% c(5,25)) %>% 
      ggplot(aes(x=log(popdensity), y=log(cum_deaths_per_sq_mi), col=days_since_thresh)) +
      geom_point()

![](EDA_files/figure-markdown_strict/unnamed-chunk-7-1.png)

There is reason to believe that, at *t* days since the threshold, risk
assessments on county death per sq mile are attainable using the
counties population density (and probably an autoregression on previous
daily death counts).

    lm(log(1 + cum_deaths_per_sq_mi) ~ log(popdensity), data=filter(county_features, days_since_thresh==25))

    ## 
    ## Call:
    ## lm(formula = log(1 + cum_deaths_per_sq_mi) ~ log(popdensity), 
    ##     data = filter(county_features, days_since_thresh == 25))
    ## 
    ## Coefficients:
    ##     (Intercept)  log(popdensity)  
    ##        -0.28145          0.06531

Therefore, cumulative deaths per square mile and population density in
counties appear to have a relationship of the form:

    cum_deaths_per_sq_mi = 0.75 * popdensity ^ 0.07 

    x = seq(100, 5000, 100)
    y = 0.75 * x ^ 0.065
    plot(x,y)

![](EDA_files/figure-markdown_strict/unnamed-chunk-9-1.png)

The next two plots compare the deaths per sq mile in `30661`New York
City and `48453` Travis County (Austin City, TX). It looks like
population density accelerates the deaths per sq mile growth rate.

    county_features %>% 
      filter(fips %in% c(48453, 36061)) %>% 
      ggplot(aes(x=days_since_thresh, y=log(cum_deaths_per_sq_mi), col=fips)) +
      geom_point() + geom_line()

![](EDA_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    county_features %>% 
      filter(fips %in% c(48453, 36061)) %>% 
      ggplot(aes(x=days_since_thresh, y=log(deaths_per_sq_mi), col=fips)) +
      geom_point() + geom_line()

![](EDA_files/figure-markdown_strict/unnamed-chunk-11-1.png)

The next three plots show the relationship between the population
density and the rural-urban variables in the US counties.

    county_features %>% 
      filter(days_since_thresh == 0) %>% 
      ggplot(aes(y = rural_urban, x = log(popdensity))) + 
      geom_point(alpha = 0.5)

![](EDA_files/figure-markdown_strict/unnamed-chunk-12-1.png)

    county_features %>% 
      filter(days_since_thresh == 0) %>% 
      ggplot(aes(y = urban_influence, x = log(popdensity))) + 
      geom_point(alpha = 0.5)

![](EDA_files/figure-markdown_strict/unnamed-chunk-13-1.png)

    county_features %>% 
      filter(days_since_thresh == 0) %>% 
      ggplot(aes(x = log(popdensity), y = log(rural_urban * urban_influence))) + 
      geom_point(alpha = 0.5)

![](EDA_files/figure-markdown_strict/unnamed-chunk-14-1.png)

Finally, let’s look at the rural-urban relationship with log cumulative
deaths per sq mile and population density after 25 days have passed
since the threshold was reached in a given county.

    county_features %>% 
      filter(days_since_thresh == 25) %>% 
      ggplot(aes(x=log(popdensity), y=log(cum_deaths_per_sq_mi), col=log(rural_urban * urban_influence))) +
      geom_point() + 
      labs(col = "")

![](EDA_files/figure-markdown_strict/unnamed-chunk-15-1.png)

Local policies and interventions
--------------------------------

When did the interventions occurred?

-   Closing schools

<!-- -->

    county_features %>% 
      distinct(fips, schools) %>% 
      pull(schools) %>% table()

    ## .
    ## 2020-03-16 2020-03-17 2020-03-18 2020-03-19 2020-03-20 2020-03-21 
    ##         16       1229        403        520        387        200 
    ## 2020-03-24 2020-04-03 
    ##        367         99

-   Stay at home orders

<!-- -->

    county_features %>% 
      distinct(fips, stayhome) %>% 
      pull(stayhome) %>% table()

    ## .
    ## 2020-03-20 2020-03-22 2020-03-23 2020-03-24 2020-03-25 2020-03-26 
    ##         58        124         64        245        315        147 
    ## 2020-03-27 2020-03-28 2020-03-29 2020-03-31 2020-04-01 2020-04-02 
    ##        177        109        115        360        109         64 
    ## 2020-04-03 2020-04-04 2020-04-05 2020-04-07 2020-04-08 
    ##        258        212         66        184         46

-   In how many counties did the interventions occur after the thresh

<!-- -->

    after_thresh <- county_features %>% 
      filter(days_since_thresh == 0) %>% 
      mutate(gt500_after_thresh = gt500 - date, 
             gt50_after_thresh = gt50 - date, 
             stayhome_after_thresh = stayhome - date, 
             schools_after_thresh = schools - date, 
             restaurants_after_thresh = restaurants - date, 
             entertainment_after_thresh = entertainment - date)

    print("total")

    ## [1] "total"

    length(unique(after_thresh$fips))

    ## [1] 1521

    print("gt500")

    ## [1] "gt500"

    length(unique(after_thresh$fips[after_thresh$gt500_after_thresh > 0]))

    ## [1] 79

    print("gt50")

    ## [1] "gt50"

    length(unique(after_thresh$fips[after_thresh$gt50_after_thresh > 0]))

    ## [1] 102

    print("stayhome")

    ## [1] "stayhome"

    length(unique(after_thresh$fips[after_thresh$stayhome_after_thresh > 0]))

    ## [1] 313

    print("schools")

    ## [1] "schools"

    length(unique(after_thresh$fips[after_thresh$schools_after_thresh > 0]))

    ## [1] 52

    print("restaurants")

    ## [1] "restaurants"

    length(unique(after_thresh$fips[after_thresh$restaurants_after_thresh > 0]))

    ## [1] 74

    print("entertainment")

    ## [1] "entertainment"

    length(unique(after_thresh$fips[after_thresh$entertainment_after_thresh > 0]))

    ## [1] 110

-   How many observations per county we have before the stay at home
    orders were put in place?

<!-- -->

    county_features %>%
      filter(days_since_thresh >= 0, 
             days_after_stayhome <= 0) %>% 
      group_by(fips) %>% 
      summarise(n_days_before_intervention = n()) %>% 
      ggplot(aes(x = n_days_before_intervention)) + 
      geom_histogram()

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](EDA_files/figure-markdown_strict/unnamed-chunk-20-1.png)

**Next steps**

A preliminary analysis of the effect of the interventions or policies by
building a hierarchical bayesian model:

-   that forecasts the death counts for a given county with information
    of the evolution of the epidemic from its peers
-   whose covariates include whether interventions (or policies) were in
    place

This approach is explored in the `vanilla_model` notebook (asset 2).
