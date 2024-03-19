---
title: "GG606: Lecture Engagement (Penguins Data)"
author: "Julia Kozak"
date: "March 21, 2024"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
    toc_depth: 4
  collapsed: no
---


________________________________________________________________________________
## 1.) Loading Things: \
### 1.1: Loading Packages \
Load in any and all packages required to manipulate, transform, and illustrate the data given.


#### 1.2: Loading Data \
Since our data sets are a pre-generated package within R, the data has already been loaded into our work space. Below, we are simply pulling from the package and registering it as a callable object. 

```r
penguins_raw=penguins_raw
penguins=penguins
```

________________________________________________________________________________
## 2.) Cleaning Data: \

Here, we will spend a bit of time transforming the object `penguins_raw` into something that appears more like the `penguins` data set.  

```r
penguins_raw_clean=penguins_raw %>%                  #create new object
  clean_names() %>%                                  #make col headers clean
  select(species, island, culmen_length_mm, culmen_depth_mm, flipper_length_mm, 
         body_mass_g, sex, date_egg) %>%            #keep and reorder these cols
  rename(bill_length_mm=culmen_length_mm,           #rename col headers 
         bill_depth_mm=culmen_depth_mm,
         year=date_egg)
```

*Side note:* \

- Under the `species` column Adelie vs. Gentoo and Chinstrap are followed by 'Penguin' and 'penguin' respectively, which was an absolute pain to troubleshoot without overwriting, so it had to be in only one line of code.\
- I had a feeling that the year column in `penguins_raw_clean` was off. Here it was registering as a character class (makes sense given method of creation), whereas in `penguins` it registered as an integer, so I went back and added in the `as.x` case during the creation to make it match. 


```r
#Start new code chunk to preserve previous code actions 
penguins_raw_clean=penguins_raw_clean %>%                 #overwrite previous data frame
  mutate(species=str_replace(species, "(?i)\\s*penguin.*", ""))%>% #wipe everything post
  mutate(sex=case_when(sex=="FEMALE"~ "female",          #change how sexes appear in col
                       sex=="MALE"~ "male")) %>%
  mutate(year=as.integer(str_extract(year, "\\d{4}"))) %>%#keep only the year, not date

#All of our columns, orders, values, and data appear as the exact same, except that they differ in the actual class type. Below we quickly convert each column to match that of the 'penguins` data frame:
  mutate(species=as.factor(species)) %>%
  mutate(island=as.factor(island)) %>%
  mutate(flipper_length_mm=as.integer(flipper_length_mm)) %>%
  mutate(body_mass_g=as.integer(body_mass_g)) %>%
  mutate(sex=as.factor(sex)) 
```

Look at each data frame so that they appear as the 'same' clean final version. 

```r
penguins_raw_clean %>%
  glimpse()
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

```r
penguins %>%
  glimpse()
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

________________________________________________________________________________
## 3.) Closing Questions

### 3.1: Lecture Engagenment Question(s) \
**Q: Were you successful in cleaning the `palmerpenguins` raw data? Comment on three places where you would do something different.[5 marks]** \
- Yes. Places that took me a second or two longer to troubleshoot: \

1. Generating the 'species' column so that it only showed the first name of each penguin species (Adelie, Gentoo, Chinstrap) tripped me up because 'penguin' was only capitalized for Adelie, not the other two. So when I checked to see if my code has worked, the other species registered as 'NA' \
2. I mean, noticing that the 'year' column was positioned to the right rather than the left like in the `penguins` data set, which made me think to check the class types. Otherwise, wasn't too big of an ordeal to change.

- I don't think I had any other issues, and in terms of three places that I would do something differently...? Nothing? This lecture engagement was pretty self explanatory and didn't really ask too much of anyone, so I don't think I would change anything too much- except maybe integrate the class type changes more elegantly throughout the code cleansing process, but some types didn't require any other mutations and would've had to undergo such a brute code line anyways.


### 3.2: Closing Comments: \
- Super cool to work kinda backwards with cleaning data, normally I'm used to being given an image that data would then have to be manipulated into. 
- If I wasn't as lazy I would've slapped in a plot, but I don't think that was a requirement for this lecture engagement, so sorry, no plots today. It was a happy coding day! :)

















