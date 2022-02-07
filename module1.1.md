module1.1
================

## Importing tidyverse

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.6     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.4     ✓ stringr 1.4.0
    ## ✓ readr   2.1.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

## Importing data

``` r
df <- read.csv("~/Desktop/Soybeanandlightdata.csv")
glimpse(df)
```

    ## Rows: 1,292
    ## Columns: 13
    ## $ Experiment     <chr> "Expt3", "Expt3", "Expt3", "Expt3", "Expt3", "Expt3", "…
    ## $ Treatment      <chr> "FS", "FS", "FS", "FS", "FS", "FS", "FS", "FS", "FS", "…
    ## $ pot            <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, …
    ## $ block          <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2…
    ## $ Genotype       <chr> "OHIO FG5", "Desha", "JIM", "Glacier", "NE1900", "Willi…
    ## $ Maturity.Class <chr> "MM", "LM", "NM", "NM", "MM", "MM", "MM", "TM", "NM", "…
    ## $ Stage          <chr> "R1", "R1", "R1", "R1", "R1", "R1", "R1", "R1", "R1", "…
    ## $ DAP            <chr> "28", "26", ".", ".", "26", "27", ".", "28", ".", ".", …
    ## $ Height         <chr> "33", "53", "63", "60", "27", "56", "29", "59", "25", "…
    ## $ SPAD           <chr> "29.6", "28.1", ".", ".", "23.3", ".", ".", "24", ".", …
    ## $ Pods.plant     <chr> ".", ".", ".", ".", ".", ".", ".", ".", ".", ".", ".", …
    ## $ Seeds.plant    <chr> ".", ".", ".", ".", ".", ".", ".", ".", ".", ".", ".", …
    ## $ Biomass        <chr> ".", ".", ".", ".", ".", ".", ".", ".", ".", ".", ".", …

## Grouping

Group so that you can see the mean and standard deviation of height by
genotype

``` r
df$Height <- as.numeric(df$Height)
```

    ## Warning: NAs introduced by coercion

``` r
df2<-df%>%
  group_by(Genotype) %>%
  summarise(mean.Height = mean(Height, na.rm = TRUE),
            sd.Height = sd(Height, na.rm = TRUE))
glimpse(df2)
```

    ## Rows: 19
    ## Columns: 3
    ## $ Genotype    <chr> "BRS 326", "BRS 60", "BRS MG 753C", "Cook", "Council", "De…
    ## $ mean.Height <dbl> 66.90476, 72.31707, 67.17073, 58.75000, 27.37500, 54.92500…
    ## $ sd.Height   <dbl> 36.039145, 26.813652, 27.963282, 17.599170, 5.260825, 21.7…

## Split-Apply-Combine

We want to see how genotype and treatment combined yield height

``` r
df3 <-df %>% 
  group_by(Genotype, Treatment) %>%
  summarise(mean.Height = mean(Height, na.rm = TRUE),
            sd.Height = sd(Height, na.rm = TRUE), .groups = "drop")
head(df3)
```

    ## # A tibble: 6 × 4
    ##   Genotype    Treatment mean.Height sd.Height
    ##   <chr>       <chr>           <dbl>     <dbl>
    ## 1 BRS 326     FS               88.3      37.1
    ## 2 BRS 326     RB               43.4      13.1
    ## 3 BRS 60      FS               80.6      29.1
    ## 4 BRS 60      RB               63.6      21.5
    ## 5 BRS MG 753C FS               72.0      23.9
    ## 6 BRS MG 753C RB               62.0      31.5

## Plot
Use a bar graph to visualize the height differences comparing the genotype’s different treatments
``` r
p <- ggplot(data=df3, aes(x=Genotype, y=mean.Height, fill=Treatment)) +
  geom_bar(stat="identity", position="dodge") +
  theme_bw(base_size = 6)
p
```
<img width="677" alt="Screen Shot 2022-02-07 at 8 00 35 AM" src="https://user-images.githubusercontent.com/98765581/152792831-52fb5761-0681-4edd-8d13-a323b4bb59d2.png">

