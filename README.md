P8105 Homework 2
================
Mengfan Luo (ml4701)

### Problem 1

Let’s load the Mr. Trash Wheel sheet as `mtw_df` and clean it:

``` r
mtw_df = 
  read_excel("data/Trash-Wheel-Collection-Totals-7-2020-2.xlsx",
             sheet = "Mr. Trash Wheel",range = "A2:N534") %>% 
  drop_na() %>% 
  janitor::clean_names() %>% 
  mutate(sports_balls = round(sports_balls)) 
```

Let’s take a look at the first and last 3 lines of `mtw_df`.

``` r
knitr::kable(head(mtw_df,3))
```

| dumpster | month | year | date       | weight\_tons | volume\_cubic\_yards | plastic\_bottles | polystyrene | cigarette\_butts | glass\_bottles | grocery\_bags | chip\_bags | sports\_balls | homes\_powered |
|---------:|:------|-----:|:-----------|-------------:|---------------------:|-----------------:|------------:|-----------------:|---------------:|--------------:|-----------:|--------------:|---------------:|
|        1 | May   | 2014 | 2014-05-16 |         4.31 |                   18 |             1450 |        1820 |           126000 |             72 |           584 |       1162 |             7 |              0 |
|        2 | May   | 2014 | 2014-05-16 |         2.74 |                   13 |             1120 |        1030 |            91000 |             42 |           496 |        874 |             5 |              0 |
|        3 | May   | 2014 | 2014-05-16 |         3.45 |                   15 |             2450 |        3100 |           105000 |             50 |          1080 |       2032 |             6 |              0 |

``` r
knitr::kable(tail(mtw_df,3))
```

| dumpster | month     | year | date       | weight\_tons | volume\_cubic\_yards | plastic\_bottles | polystyrene | cigarette\_butts | glass\_bottles | grocery\_bags | chip\_bags | sports\_balls | homes\_powered |
|---------:|:----------|-----:|:-----------|-------------:|---------------------:|-----------------:|------------:|-----------------:|---------------:|--------------:|-----------:|--------------:|---------------:|
|      451 | Decemeber | 2020 | 2020-12-30 |         2.73 |                   15 |             1800 |         780 |             4200 |             14 |           270 |        280 |            14 |       45.50000 |
|      452 | Decemeber | 2020 | 2020-12-30 |         2.12 |                   15 |             1440 |         600 |             3600 |             21 |           420 |        360 |            15 |       35.33333 |
|      453 | January   | 2021 | 2021-01-04 |         2.81 |                   15 |             1600 |         840 |             3400 |             24 |           320 |        540 |            12 |       46.83333 |

Let’s load and clean precipitation data for 2018 and 2019, and then
combine them into one dataframe `prec_2018_2019_df`.

``` r
prec_2018_df = 
  read_excel("data/Trash-Wheel-Collection-Totals-7-2020-2.xlsx",
             sheet = "2018 Precipitation",range = "A2:B14") %>% 
  janitor::clean_names() %>% 
  mutate(year = 2018)

prec_2019_df = 
  read_excel("data/Trash-Wheel-Collection-Totals-7-2020-2.xlsx",
             sheet = "2019 Precipitation",range = "A2:B14") %>% 
  janitor::clean_names() %>% 
  mutate(year = 2019)

prec_2018_2019_df = 
  bind_rows(prec_2018_df,prec_2019_df) %>% 
  mutate(month = month.name[month])
```

Let’s take a look at the first and last 3 lines of `prec_2018_2019_df`.

``` r
knitr::kable(head(prec_2018_2019_df,3))
```

| month    | total | year |
|:---------|------:|-----:|
| January  |  0.94 | 2018 |
| February |  4.80 | 2018 |
| March    |  2.69 | 2018 |

``` r
knitr::kable(tail(prec_2018_2019_df,3))
```

| month    | total | year |
|:---------|------:|-----:|
| October  |  5.45 | 2019 |
| November |  1.86 | 2019 |
| December |  3.57 | 2019 |

For `mtw_df` which is tidied from Mr. Trash Wheel sheet, there are `453`
observations and `14` variables (including serial number of the dumpster
`dumpster`). Other variables are
`month, year, date, weight_tons, volume_cubic_yards, plastic_bottles, polystyrene, cigarette_butts, glass_bottles, grocery_bags, chip_bags, sports_balls, homes_powered`.

For `prec_2018_2019_df` which is combined from cleaned precipitation
data for 2018 and 2019, there are there are `24` observations and `3`
variables, inclusing `month, total, year`. We can calculate from this
dataframe that the total precipitation in 2018 was `70.33`, and the
median number of sports balls in a dumpster in 2019 was `3.335`.

### Problem 2

1.  Clean and tidy the data in pols-month.csv. First 6 lines of the
    dataframe will be shown.

``` r
pols_df = read_csv("data/fivethirtyeight_datasets/pols-month.csv") %>% 
  separate(mon,into = c("year","month","day"),sep = "-") %>% 
  mutate(year = as.numeric(year)) %>% 
  mutate(month = as.numeric(month)) %>% 
  mutate(month = month.name[month]) %>% 
  pivot_longer(cols = starts_with("prez"),names_to = "president",
               names_prefix = "prez_",values_to = "prez_value") %>% 
  filter(prez_value==1) %>% 
  select(-day,-prez_value) 

knitr::kable(head(pols_df))
```

| year | month    | gov\_gop | sen\_gop | rep\_gop | gov\_dem | sen\_dem | rep\_dem | president |
|-----:|:---------|---------:|---------:|---------:|---------:|---------:|---------:|:----------|
| 1947 | January  |       23 |       51 |      253 |       23 |       45 |      198 | dem       |
| 1947 | February |       23 |       51 |      253 |       23 |       45 |      198 | dem       |
| 1947 | March    |       23 |       51 |      253 |       23 |       45 |      198 | dem       |
| 1947 | April    |       23 |       51 |      253 |       23 |       45 |      198 | dem       |
| 1947 | May      |       23 |       51 |      253 |       23 |       45 |      198 | dem       |
| 1947 | June     |       23 |       51 |      253 |       23 |       45 |      198 | dem       |

2.  Clean, tidy and organize the data in snp.csv. First 6 lines of the
    dataframe will be shown.

``` r
snp_df = read_csv("data/fivethirtyeight_datasets/snp.csv") %>% 
  mutate(date = mdy(date)) %>% 
  separate(date,into = c("year","month","day"),sep = "-") %>% 
  select(-day) %>% 
  mutate(year = as.numeric(year)) %>% 
  mutate(year = ifelse(year > 2049,year -100,year)) %>% 
  arrange(year,month) %>% 
  mutate(month = as.numeric(month)) %>% 
  mutate(month = month.name[month])
knitr::kable(head(snp_df))
```

| year | month    | close |
|-----:|:---------|------:|
| 1950 | January  | 17.05 |
| 1950 | February | 17.22 |
| 1950 | March    | 17.29 |
| 1950 | April    | 17.96 |
| 1950 | May      | 18.78 |
| 1950 | June     | 17.69 |

3.  Tidy the unemployment data so that it can be merged with the
    previous datasets. First 6 lines of the dataframe will be shown.

``` r
unemploy_df = read_csv("data/fivethirtyeight_datasets/unemployment.csv") 
colnames(unemploy_df)=c("year","1","2","3","4","5","6","7","8","9","10","11","12")

unemploy_df =
  pivot_longer(unemploy_df,`1`:`12`,
               names_to = "month",values_to = "unemploy_percent") %>% 
  mutate(month = as.numeric(month)) %>% 
  mutate(month = month.name[month])
knitr::kable(head(unemploy_df))
```

| year | month    | unemploy\_percent |
|-----:|:---------|------------------:|
| 1948 | January  |               3.4 |
| 1948 | February |               3.8 |
| 1948 | March    |               4.0 |
| 1948 | April    |               3.9 |
| 1948 | May      |               3.5 |
| 1948 | June     |               3.6 |

4.  Join the datasets by merging snp into pols, and merging unemployment
    into the result. First and last 6 rows of the resulted dataframe
    `merge3_df` are shown below.

``` r
merge2_df = left_join(pols_df,snp_df,by = c("year","month"))
merge3_df = left_join(merge2_df,unemploy_df,by = c("year","month"))
knitr::kable(head(merge3_df))
```

| year | month    | gov\_gop | sen\_gop | rep\_gop | gov\_dem | sen\_dem | rep\_dem | president | close | unemploy\_percent |
|-----:|:---------|---------:|---------:|---------:|---------:|---------:|---------:|:----------|------:|------------------:|
| 1947 | January  |       23 |       51 |      253 |       23 |       45 |      198 | dem       |    NA |                NA |
| 1947 | February |       23 |       51 |      253 |       23 |       45 |      198 | dem       |    NA |                NA |
| 1947 | March    |       23 |       51 |      253 |       23 |       45 |      198 | dem       |    NA |                NA |
| 1947 | April    |       23 |       51 |      253 |       23 |       45 |      198 | dem       |    NA |                NA |
| 1947 | May      |       23 |       51 |      253 |       23 |       45 |      198 | dem       |    NA |                NA |
| 1947 | June     |       23 |       51 |      253 |       23 |       45 |      198 | dem       |    NA |                NA |

``` r
knitr::kable(tail(merge3_df))
```

| year | month    | gov\_gop | sen\_gop | rep\_gop | gov\_dem | sen\_dem | rep\_dem | president |   close | unemploy\_percent |
|-----:|:---------|---------:|---------:|---------:|---------:|---------:|---------:|:----------|--------:|------------------:|
| 2015 | January  |       31 |       54 |      245 |       18 |       44 |      188 | dem       | 1994.99 |               5.7 |
| 2015 | February |       31 |       54 |      245 |       18 |       44 |      188 | dem       | 2104.50 |               5.5 |
| 2015 | March    |       31 |       54 |      245 |       18 |       44 |      188 | dem       | 2067.89 |               5.5 |
| 2015 | April    |       31 |       54 |      244 |       18 |       44 |      188 | dem       | 2085.51 |               5.4 |
| 2015 | May      |       31 |       54 |      245 |       18 |       44 |      188 | dem       | 2107.39 |               5.5 |
| 2015 | June     |       31 |       54 |      246 |       18 |       44 |      188 | dem       | 2063.11 |               5.3 |

In this problem, 3 dataframe was tidied and merged together.

Dataframe 1 `pols_df` has `817` rows and `9` columns. The variables in
the dataframe are
`year, month, gov_gop, sen_gop, rep_gop, gov_dem, sen_dem, rep_dem, president`.

Dataframe 2 `snp_df` has `787` rows and `3` columns. The variables in
the dataframe are `year, month, close`.

Dataframe 3 `unemploy_df` has `816` rows and `3` columns. The variables
in the dataframe are `year, month, unemploy_percent`.

The resulting dataset `merge3_df` is merged by the three above dataset
according to the same year and month. It has `817` rows and `11`
columns. The variables in the dataframe are
`year, month, gov_gop, sen_gop, rep_gop, gov_dem, sen_dem, rep_dem, president, close, unemploy_percent`.
The range of years of the merged table is `68` years,from `1947` to
`2015`.

### Problem 3

Load dataset as `babyname_df` and clean column names

``` r
babyname_df = read_csv("data/Popular_Baby_Names.csv") %>% 
  janitor::clean_names()
```

    ## Rows: 19418 Columns: 6

    ## -- Column specification --------------------------------------------------------
    ## Delimiter: ","
    ## chr (3): Gender, Ethnicity, Child's First Name
    ## dbl (3): Year of Birth, Count, Rank

    ## 
    ## i Use `spec()` to retrieve the full column specification for this data.
    ## i Specify the column types or set `show_col_types = FALSE` to quiet this message.

Looking into the values of the dataset, we found the ethnicity may have
some inconsistent value imput. For example, `BLACK NON HISPANIC` and
`BLACK NON HISP` obviously stand for the same ethnicity, while have two
different imput value.

``` r
unique(pull(babyname_df,ethnicity))
```

    ## [1] "ASIAN AND PACIFIC ISLANDER" "BLACK NON HISPANIC"        
    ## [3] "HISPANIC"                   "WHITE NON HISPANIC"        
    ## [5] "ASIAN AND PACI"             "BLACK NON HISP"            
    ## [7] "WHITE NON HISP"

Here we try to make the values for `ethnicity` consistent.

``` r
ethnicity_list = pull(babyname_df,ethnicity)
n = length(ethnicity_list)


for (i in 1:n ){
  if (ethnicity_list[i] == "ASIAN AND PACI") {
    ethnicity_list[i] = "ASIAN AND PACIFIC ISLANDER"
  }else if (ethnicity_list[i] == "BLACK NON HISP") {
    ethnicity_list[i] = "BLACK NON HISPANIC"
  }else if (ethnicity_list[i] == "WHITE NON HISP") {
    ethnicity_list[i] = "WHITE NON HISPANIC"
  }
}
  
babyname_df = mutate(babyname_df,ethnicity = ethnicity_list)

unique(pull(babyname_df,ethnicity))
```

    ## [1] "ASIAN AND PACIFIC ISLANDER" "BLACK NON HISPANIC"        
    ## [3] "HISPANIC"                   "WHITE NON HISPANIC"

Also the `childs_first_name` are inconsistent in terms of cases. We make
it consistent with an upper case first letter and lower case for the
rest letters in one name.

``` r
babyname_df = 
  mutate(babyname_df,childs_first_name = str_to_lower(childs_first_name))%>%
  mutate(childs_first_name = Hmisc::capitalize(childs_first_name))  
```

Now, we have made the values in columns consistent. We then remove the
duplicate rows.

``` r
babyname_df = distinct(babyname_df)
```

`7237` duplicated rows are removed

The following table shows **the rank in popularity of the name “Olivia”
as a female baby name over year 2013-2016 for different ethnicities**.

``` r
olivia_df = filter(babyname_df,childs_first_name == "Olivia") %>% 
  arrange(year_of_birth) %>% 
  select(-gender,-count,-childs_first_name) %>% 
  pivot_wider(names_from = "year_of_birth",values_from = "rank")
knitr::kable(olivia_df)
```

| ethnicity                  | 2011 | 2012 | 2013 | 2014 | 2015 | 2016 |
|:---------------------------|-----:|-----:|-----:|-----:|-----:|-----:|
| ASIAN AND PACIFIC ISLANDER |    4 |    3 |    3 |    1 |    1 |    1 |
| BLACK NON HISPANIC         |   10 |    8 |    6 |    8 |    4 |    8 |
| HISPANIC                   |   18 |   22 |   22 |   16 |   16 |   13 |
| WHITE NON HISPANIC         |    2 |    4 |    1 |    1 |    1 |    1 |

The following table shows **the most popular name among male children
over year 2011-2016 for different ethnicities**.

``` r
male_df = filter(babyname_df,gender == "MALE",rank == 1) %>% 
  arrange(year_of_birth) %>% 
  select(-gender,-count,-rank) %>% 
  pivot_wider(names_from = "year_of_birth",values_from = "childs_first_name")
knitr::kable(male_df)
```

| ethnicity                  | 2011    | 2012   | 2013   | 2014   | 2015   | 2016   |
|:---------------------------|:--------|:-------|:-------|:-------|:-------|:-------|
| ASIAN AND PACIFIC ISLANDER | Ethan   | Ryan   | Jayden | Jayden | Jayden | Ethan  |
| BLACK NON HISPANIC         | Jayden  | Jayden | Ethan  | Ethan  | Noah   | Noah   |
| HISPANIC                   | Jayden  | Jayden | Jayden | Liam   | Liam   | Liam   |
| WHITE NON HISPANIC         | Michael | Joseph | David  | Joseph | David  | Joseph |

Scatter plot showing the number of children with a name against the rank
in popularity of that name, for male, white non-hispanic children born
in 2016 are shown as belowed:

``` r
male2016_df = filter(babyname_df,gender == "MALE",
                     ethnicity == "WHITE NON HISPANIC",year_of_birth == 2016) %>%
  select(childs_first_name,count,rank)
ggplot(male2016_df,aes(x = rank, y = count))+ geom_point()+
  labs(title = "Scatter plot of the number of children with a name against the rank of the name" ,
  subtitle = "White non-hispanic children born in 2016",
  x = "Rank in popularity of the name",
  y = "Number of children with the name")
```

![](p8105_hw2_ml4701_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->
