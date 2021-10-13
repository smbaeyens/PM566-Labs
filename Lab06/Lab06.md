Lab 06
================
Sylvia Baeyens
8/1/2021

Reading in Data

``` r
rawData= "mtsamples.csv"
if(!file.exists(rawData))
  download.file("https://raw.githubusercontent.com/USCbiostats/data-science-data/master/00_mtsamples/mtsamples.csv", destfile = rawData)

mtsamples= read.csv(rawData) 
mtsamples= as_tibble(mtsamples)
```

## 1. What specialties do we have?

First, we count the number of specialties

``` r
Nspecialties = mtsamples %>%
  count(medical_specialty)

Nspecialties %>%
  arrange(desc(n)) %>%
  top_n(15) %>%
  knitr::kable()
```

    ## Selecting by n

| medical\_specialty            |    n |
|:------------------------------|-----:|
| Surgery                       | 1103 |
| Consult - History and Phy.    |  516 |
| Cardiovascular / Pulmonary    |  372 |
| Orthopedic                    |  355 |
| Radiology                     |  273 |
| General Medicine              |  259 |
| Gastroenterology              |  230 |
| Neurology                     |  223 |
| SOAP / Chart / Progress Notes |  166 |
| Obstetrics / Gynecology       |  160 |
| Urology                       |  158 |
| Discharge Summary             |  108 |
| ENT - Otolaryngology          |   98 |
| Neurosurgery                  |   94 |
| Hematology - Oncology         |   90 |

There are 40 specialties.

Next, we look at distribution:

``` r
#using method 2 from class:

ggplot(Nspecialties, aes(x = n, y = fct_reorder(medical_specialty, n))) +
  geom_col()
```

![](Lab06_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

There is no even distribution among the 40 specialties. Surgery has the
highest count, while hospice-pallative care has the lowest count.

## 2. Tokenizing the words in the transcription column
