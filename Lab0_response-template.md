Geog4/6300: Lab 0
================

This “lab” assignment provides an opportunity to learn the basic
mechanics of Github classroom, loading data, and doing some basic
operations using the tools in the tidyverse.

We’ll be combining two datasets for this assignment: the New York Times
dataset on COVID case counts by county and the American Community
Survey’s most recent data on county population. You can load them both
using the read\_csv function and either the URL or path. For example,
you can load the ACS data using this function:

``` r
county_pop<-read_csv("data/pop_acs18.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   GEOID = col_double(),
    ##   NAME = col_character(),
    ##   variable = col_character(),
    ##   estimate = col_double(),
    ##   moe = col_double()
    ## )

In this dataset, you’ll see a county FIPS code (GEOID), county name, the
variable code for total population, a population estimate, and a margin
of error (which is blank in this case).

## Loading the data

### Question 1 (2 points):

Load these data using the read\_csv command. You can copy the code above
for the county population data. For the New York Times data, replace the
file path name with this URL:

  - <https://github.com/nytimes/covid-19-data/raw/master/us-counties.csv>

<!-- end list -->

``` r
##Your code goes here.
county_pop<-read_csv("data/pop_acs18.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   GEOID = col_double(),
    ##   NAME = col_character(),
    ##   variable = col_character(),
    ##   estimate = col_double(),
    ##   moe = col_double()
    ## )

``` r
nytimesdata<-read_csv("https://github.com/nytimes/covid-19-data/raw/master/us-counties.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   date = col_date(format = ""),
    ##   county = col_character(),
    ##   state = col_character(),
    ##   fips = col_character(),
    ##   cases = col_double(),
    ##   deaths = col_double()
    ## )

## Connecting the data

Next you’ll need to join these two datasets using the left\_join
function (discussed in this week’s lectures/scripts). To do so you’ll
need to have two fields with the same name in each dataset. The fips
codes are in both datasets, but they have different names (GEOID and
fips) and formats (numeric and character). The following code creates a
new variable called fips in the ACS data that’s a character using the
mutate function.

``` r
county_pop<-county_pop %>%
  mutate(fips=as.character(GEOID))
```

Call the function above to rename your data. Now you’re ready to join
the data.

``` r
county_pop
```

    ## # A tibble: 159 x 6
    ##    GEOID NAME                     variable   estimate   moe fips 
    ##    <dbl> <chr>                    <chr>         <dbl> <dbl> <chr>
    ##  1 13001 Appling County, Georgia  B01001_001    18454    NA 13001
    ##  2 13003 Atkinson County, Georgia B01001_001     8265    NA 13003
    ##  3 13005 Bacon County, Georgia    B01001_001    11228    NA 13005
    ##  4 13007 Baker County, Georgia    B01001_001     3189    NA 13007
    ##  5 13009 Baldwin County, Georgia  B01001_001    45286    NA 13009
    ##  6 13011 Banks County, Georgia    B01001_001    18510    NA 13011
    ##  7 13013 Barrow County, Georgia   B01001_001    76887    NA 13013
    ##  8 13015 Bartow County, Georgia   B01001_001   103620    NA 13015
    ##  9 13017 Ben Hill County, Georgia B01001_001    17154    NA 13017
    ## 10 13019 Berrien County, Georgia  B01001_001    19025    NA 13019
    ## # … with 149 more rows

### Question 2 (2 points)

Using inner\_join, join the New York Times dataset to the county
population one.

``` r
joined_data<-inner_join(county_pop, nytimesdata)
```

    ## Joining, by = "fips"

``` r
county_pop
```

    ## # A tibble: 159 x 6
    ##    GEOID NAME                     variable   estimate   moe fips 
    ##    <dbl> <chr>                    <chr>         <dbl> <dbl> <chr>
    ##  1 13001 Appling County, Georgia  B01001_001    18454    NA 13001
    ##  2 13003 Atkinson County, Georgia B01001_001     8265    NA 13003
    ##  3 13005 Bacon County, Georgia    B01001_001    11228    NA 13005
    ##  4 13007 Baker County, Georgia    B01001_001     3189    NA 13007
    ##  5 13009 Baldwin County, Georgia  B01001_001    45286    NA 13009
    ##  6 13011 Banks County, Georgia    B01001_001    18510    NA 13011
    ##  7 13013 Barrow County, Georgia   B01001_001    76887    NA 13013
    ##  8 13015 Bartow County, Georgia   B01001_001   103620    NA 13015
    ##  9 13017 Ben Hill County, Georgia B01001_001    17154    NA 13017
    ## 10 13019 Berrien County, Georgia  B01001_001    19025    NA 13019
    ## # … with 149 more rows

### Question 3 (3 points)

Find some documentation on the inner\_join function online or using help
in R. How is it different from left\_join or full\_join? List the source
you used for your answer.

{The inner\_join differs from the left\_join in that it only retains
rows that are shared by both data sets as opposed to retaining all rows
from the first data set and then matching the corresponding rows from
the second data set. The full\_join matches corresponding rows from both
data sets and joins them but unlike the left\_join, it does not exclude
rows from the second data set that do not correspondingly match to any
rows in the first data set. I used the cheat sheet for
data-transformation provided within the rstudio cloud environment under
the learning resources section. Source:
“<https://rstudio.cloud/learn/cheat-sheets>”}

## Calculating a rate

Now you can calculate a case rate by county, also using mutate. These
are usually reported as cases per 100,000 population You’ll need to set
up some math that follows the following pattern:

  - newvar = cases / population \* 100000

### Question 4 (2 points)

Use mutate to calculate a case rate per 100,000 people.

``` r
case_rate<-joined_data %>%
  mutate(Case_rate=cases/estimate * 100000)
```

### Question 5 (1 point)

Which Georgia county has the *highest* case rate according to your
analysis? You can open the dataset and click on the column name for the
rate to arrange from high to low. Give the county name, the rate, and
the date when this rate was calculated.

{Chattahoochee County appears to have the highest case rate with a rate
of 9900.622}

That’s it\! When you’re done with this lab, use the Knit command to
render it as a Github markdown document. Then push it up to Github using
the procedure outlined in this week’s videos.
