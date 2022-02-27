---
title: 'Weekly Exercises #3'
author: "Jason Whitelaw"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for graphing and data cleaning
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(ggthemes)      # for even more plotting themes
library(geofacet)      # for special faceting with US map layout
theme_set(theme_minimal())       # My favorite ggplot() theme :)
```


```r
# Lisa's garden data
data("garden_harvest")

# Seeds/plants (and other garden supply) costs
data("garden_spending")

# Planting dates and locations
data("garden_planting")

# Tidy Tuesday dog breed data
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')

# Tidy Tuesday data for challenge problem
kids <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-09-15/kids.csv')
```

## Setting up on GitHub!

Before starting your assignment, you need to get yourself set up on GitHub and make sure GitHub is connected to R Studio. To do that, you should read the instruction (through the "Cloning a repo" section) and watch the video [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md). Then, do the following (if you get stuck on a step, don't worry, I will help! You can always get started on the homework and we can figure out the GitHub piece later):

* Create a repository on GitHub, giving it a nice name so you know it is for the 3rd weekly exercise assignment (follow the instructions in the document/video).  
* Copy the repo name so you can clone it to your computer. In R Studio, go to file --> New project --> Version control --> Git and follow the instructions from the document/video.  
* Download the code from this document and save it in the repository folder/project on your computer.  
* In R Studio, you should then see the .Rmd file in the upper right corner in the Git tab (along with the .Rproj file and probably .gitignore).  
* Check all the boxes of the files in the Git tab and choose commit.  
* In the commit window, write a commit message, something like "Initial upload" would be appropriate, and commit the files.  
* Either click the green up arrow in the commit window or close the commit window and click the green up arrow in the Git tab to push your changes to GitHub.  
* Refresh your GitHub page (online) and make sure the new documents have been pushed out.  
* Back in R Studio, knit the .Rmd file. When you do that, you should have two (as long as you didn't make any changes to the .Rmd file, in which case you might have three) files show up in the Git tab - an .html file and an .md file. The .md file is something we haven't seen before and is here because I included `keep_md: TRUE` in the YAML heading. The .md file is a markdown (NOT R Markdown) file that is an interim step to creating the html file. They are displayed fairly nicely in GitHub, so we want to keep it and look at it there. Click the boxes next to these two files, commit changes (remember to include a commit message), and push them (green up arrow).  
* As you work through your homework, save and commit often, push changes occasionally (maybe after you feel finished with an exercise?), and go check to see what the .md file looks like on GitHub.  
* If you have issues, let me know! This is new to many of you and may not be intuitive at first. But, I promise, you'll get the hang of it! 



## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.


## Warm-up exercises with garden data

These exercises will reiterate what you learned in the "Expanding the data wrangling toolkit" tutorial. If you haven't gone through the tutorial yet, you should do that first.

  1. Summarize the `garden_harvest` data to find the total harvest weight in pounds for each vegetable and day of week (HINT: use the `wday()` function from `lubridate`). Display the results so that the vegetables are rows but the days of the week are columns.


```r
garden_harvest %>%
  mutate(dayOfWeek = wday(date)) %>%
  group_by(vegetable, dayOfWeek) %>%
  summarize(weightByDay = sum(weight))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["dayOfWeek"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["weightByDay"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"apple","2":"7","3":"156"},{"1":"asparagus","2":"7","3":"20"},{"1":"basil","2":"2","3":"30"},{"1":"basil","2":"3","3":"50"},{"1":"basil","2":"5","3":"12"},{"1":"basil","2":"6","3":"212"},{"1":"basil","2":"7","3":"186"},{"1":"beans","2":"1","3":"868"},{"1":"beans","2":"2","3":"2952"},{"1":"beans","2":"3","3":"1990"},{"1":"beans","2":"4","3":"1852"},{"1":"beans","2":"5","3":"1539"},{"1":"beans","2":"6","3":"692"},{"1":"beans","2":"7","3":"2136"},{"1":"beets","2":"1","3":"146"},{"1":"beets","2":"2","3":"305"},{"1":"beets","2":"3","3":"72"},{"1":"beets","2":"4","3":"83"},{"1":"beets","2":"5","3":"5394"},{"1":"beets","2":"6","3":"11"},{"1":"beets","2":"7","3":"172"},{"1":"broccoli","2":"1","3":"571"},{"1":"broccoli","2":"2","3":"372"},{"1":"broccoli","2":"4","3":"321"},{"1":"broccoli","2":"6","3":"75"},{"1":"carrots","2":"1","3":"1332"},{"1":"carrots","2":"2","3":"395"},{"1":"carrots","2":"3","3":"160"},{"1":"carrots","2":"4","3":"2523"},{"1":"carrots","2":"5","3":"1213"},{"1":"carrots","2":"6","3":"970"},{"1":"carrots","2":"7","3":"1057"},{"1":"chives","2":"4","3":"8"},{"1":"cilantro","2":"3","3":"2"},{"1":"cilantro","2":"6","3":"33"},{"1":"cilantro","2":"7","3":"17"},{"1":"corn","2":"1","3":"661"},{"1":"corn","2":"2","3":"344"},{"1":"corn","2":"3","3":"330"},{"1":"corn","2":"4","3":"2405"},{"1":"corn","2":"6","3":"1564"},{"1":"corn","2":"7","3":"597"},{"1":"cucumbers","2":"1","3":"1408"},{"1":"cucumbers","2":"2","3":"2166"},{"1":"cucumbers","2":"3","3":"4557"},{"1":"cucumbers","2":"4","3":"2407"},{"1":"cucumbers","2":"5","3":"1500"},{"1":"cucumbers","2":"6","3":"3370"},{"1":"cucumbers","2":"7","3":"4373"},{"1":"edamame","2":"3","3":"636"},{"1":"edamame","2":"7","3":"2127"},{"1":"hot peppers","2":"2","3":"571"},{"1":"hot peppers","2":"3","3":"64"},{"1":"hot peppers","2":"4","3":"31"},{"1":"jalapeño","2":"1","3":"119"},{"1":"jalapeño","2":"2","3":"2519"},{"1":"jalapeño","2":"3","3":"249"},{"1":"jalapeño","2":"4","3":"218"},{"1":"jalapeño","2":"5","3":"102"},{"1":"jalapeño","2":"6","3":"587"},{"1":"jalapeño","2":"7","3":"684"},{"1":"kale","2":"1","3":"375"},{"1":"kale","2":"2","3":"938"},{"1":"kale","2":"3","3":"128"},{"1":"kale","2":"4","3":"280"},{"1":"kale","2":"5","3":"127"},{"1":"kale","2":"6","3":"173"},{"1":"kale","2":"7","3":"676"},{"1":"kohlrabi","2":"5","3":"191"},{"1":"lettuce","2":"1","3":"665"},{"1":"lettuce","2":"2","3":"1115"},{"1":"lettuce","2":"3","3":"416"},{"1":"lettuce","2":"4","3":"538"},{"1":"lettuce","2":"5","3":"1112"},{"1":"lettuce","2":"6","3":"817"},{"1":"lettuce","2":"7","3":"597"},{"1":"onions","2":"1","3":"118"},{"1":"onions","2":"2","3":"231"},{"1":"onions","2":"3","3":"321"},{"1":"onions","2":"5","3":"273"},{"1":"onions","2":"6","3":"33"},{"1":"onions","2":"7","3":"868"},{"1":"peas","2":"1","3":"933"},{"1":"peas","2":"2","3":"2102"},{"1":"peas","2":"3","3":"938"},{"1":"peas","2":"4","3":"490"},{"1":"peas","2":"5","3":"1541"},{"1":"peas","2":"6","3":"425"},{"1":"peas","2":"7","3":"1294"},{"1":"peppers","2":"1","3":"228"},{"1":"peppers","2":"2","3":"1146"},{"1":"peppers","2":"3","3":"655"},{"1":"peppers","2":"4","3":"1108"},{"1":"peppers","2":"5","3":"322"},{"1":"peppers","2":"6","3":"152"},{"1":"peppers","2":"7","3":"627"},{"1":"potatoes","2":"2","3":"440"},{"1":"potatoes","2":"4","3":"2073"},{"1":"potatoes","2":"5","3":"5376"},{"1":"potatoes","2":"6","3":"1697"},{"1":"potatoes","2":"7","3":"1271"},{"1":"pumpkins","2":"2","3":"13662"},{"1":"pumpkins","2":"3","3":"14450"},{"1":"pumpkins","2":"7","3":"42043"},{"1":"radish","2":"1","3":"37"},{"1":"radish","2":"2","3":"89"},{"1":"radish","2":"3","3":"43"},{"1":"radish","2":"5","3":"67"},{"1":"radish","2":"6","3":"88"},{"1":"radish","2":"7","3":"105"},{"1":"raspberries","2":"2","3":"59"},{"1":"raspberries","2":"3","3":"152"},{"1":"raspberries","2":"5","3":"131"},{"1":"raspberries","2":"6","3":"259"},{"1":"raspberries","2":"7","3":"242"},{"1":"rutabaga","2":"1","3":"8738"},{"1":"rutabaga","2":"6","3":"1623"},{"1":"rutabaga","2":"7","3":"3129"},{"1":"spinach","2":"1","3":"221"},{"1":"spinach","2":"2","3":"67"},{"1":"spinach","2":"3","3":"225"},{"1":"spinach","2":"4","3":"97"},{"1":"spinach","2":"5","3":"106"},{"1":"spinach","2":"6","3":"89"},{"1":"spinach","2":"7","3":"118"},{"1":"squash","2":"2","3":"11038"},{"1":"squash","2":"3","3":"8377"},{"1":"squash","2":"7","3":"25502"},{"1":"strawberries","2":"1","3":"37"},{"1":"strawberries","2":"2","3":"217"},{"1":"strawberries","2":"5","3":"40"},{"1":"strawberries","2":"6","3":"221"},{"1":"strawberries","2":"7","3":"77"},{"1":"Swiss chard","2":"1","3":"566"},{"1":"Swiss chard","2":"2","3":"487"},{"1":"Swiss chard","2":"3","3":"32"},{"1":"Swiss chard","2":"4","3":"412"},{"1":"Swiss chard","2":"5","3":"1012"},{"1":"Swiss chard","2":"6","3":"280"},{"1":"Swiss chard","2":"7","3":"333"},{"1":"tomatoes","2":"1","3":"34296"},{"1":"tomatoes","2":"2","3":"5213"},{"1":"tomatoes","2":"3","3":"22113"},{"1":"tomatoes","2":"4","3":"26429"},{"1":"tomatoes","2":"5","3":"15657"},{"1":"tomatoes","2":"6","3":"38590"},{"1":"tomatoes","2":"7","3":"15933"},{"1":"zucchini","2":"1","3":"5550"},{"1":"zucchini","2":"2","3":"5532"},{"1":"zucchini","2":"3","3":"7470"},{"1":"zucchini","2":"4","3":"926"},{"1":"zucchini","2":"5","3":"15708"},{"1":"zucchini","2":"6","3":"8492"},{"1":"zucchini","2":"7","3":"1549"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  2. Summarize the `garden_harvest` data to find the total harvest in pound for each vegetable variety and then try adding the plot from the `garden_planting` table. This will not turn out perfectly. What is the problem? How might you fix it?


```r
garden_harvest %>%
  group_by(vegetable, variety) %>%
  mutate(wt_lbs = weight * 0.00220462) %>%
  summarize(weightLbsByVariety = sum(wt_lbs)) %>%
  left_join(garden_planting, by = c("vegetable", "variety"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["weightLbsByVariety"],"name":[3],"type":["dbl"],"align":["right"]},{"label":["plot"],"name":[4],"type":["chr"],"align":["left"]},{"label":["number_seeds_planted"],"name":[5],"type":["dbl"],"align":["right"]},{"label":["date"],"name":[6],"type":["date"],"align":["right"]},{"label":["number_seeds_exact"],"name":[7],"type":["lgl"],"align":["right"]},{"label":["notes"],"name":[8],"type":["chr"],"align":["left"]}],"data":[{"1":"apple","2":"unknown","3":"0.34392072","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"asparagus","2":"asparagus","3":"0.04409240","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"basil","2":"Isle of Naxos","3":"1.08026380","4":"potB","5":"40","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"M","5":"30","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"beans","2":"Bush Bush Slender","3":"22.12997556","4":"D","5":"10","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"K","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Chinese Red Noodle","3":"0.78484472","4":"L","5":"5","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"beans","2":"Classic Slenderette","3":"3.60455370","4":"E","5":"29","6":"2020-06-20","7":"TRUE","8":"NA"},{"1":"beets","2":"Gourmet Golden","3":"7.02171470","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"beets","2":"leaves","3":"0.22266662","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"beets","2":"Sweet Merlin","3":"6.38678414","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"D","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Main Crop Bravado","3":"2.13186754","4":"I","5":"7","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"broccoli","2":"Yod Fah","3":"0.82011864","4":"P","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Bolero","3":"8.29157582","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"H","5":"40","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"Dragon","3":"4.10500244","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"carrots","2":"greens","3":"0.37258078","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"H","5":"50","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"carrots","2":"King Midas","3":"4.09618396","4":"L","5":"50","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"chives","2":"perrenial","3":"0.01763696","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"potD","5":"15","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"cilantro","2":"cilantro","3":"0.11464024","4":"E","5":"20","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"corn","2":"Dorinny Sweet","3":"11.40670388","4":"A","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"corn","2":"Golden Bantam","3":"1.60275874","4":"B","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"cucumbers","2":"pickling","3":"43.60958822","4":"L","5":"20","6":"2020-05-25","7":"FALSE","8":"NA"},{"1":"edamame","2":"edamame","3":"6.09136506","4":"O","5":"25","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"hot peppers","2":"thai","3":"0.14770954","4":"potB","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"hot peppers","2":"variety","3":"1.32056738","4":"potC","5":"6","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"jalapeño","2":"giant","3":"9.87228836","4":"L","5":"4","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"P","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"kale","2":"Heirloom Lacinto","3":"5.94586014","4":"front","5":"30","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"0.42108242","4":"front","5":"10","6":"2020-05-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"C","5":"60","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Farmer's Market Blend","3":"3.80296950","4":"L","5":"60","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"lettuce","2":"Lettuce Mixture","3":"4.74875148","4":"G","5":"200","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"lettuce","2":"mustard greens","3":"0.05070626","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"reseed","3":"0.09920790","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"lettuce","2":"Tatsoi","3":"2.89466606","4":"P","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"onions","2":"Delicious Duo","3":"0.75398004","4":"P","5":"25","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"onions","2":"Long Keeping Rainbow","3":"3.31133924","4":"H","5":"40","6":"2020-04-26","7":"FALSE","8":"NA"},{"1":"peas","2":"Magnolia Blossom","3":"7.45822946","4":"B","5":"24","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peas","2":"Super Sugar Snap","3":"9.56805080","4":"A","5":"22","6":"2020-04-19","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"K","5":"12","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"green","3":"5.69232884","4":"O","5":"5","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potA","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"peppers","2":"variety","3":"3.65085072","4":"potD","5":"1","6":"2020-05-21","7":"TRUE","8":"NA"},{"1":"potatoes","2":"purple","3":"3.00930630","4":"D","5":"5","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"red","3":"4.43349082","4":"I","5":"3","6":"2020-05-22","7":"FALSE","8":"NA"},{"1":"potatoes","2":"Russet","3":"9.09185288","4":"D","5":"8","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"10","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"potatoes","2":"yellow","3":"7.40090934","4":"I","5":"8","6":"2020-05-22","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"32.87308882","4":"B","5":"3","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"New England Sugar","3":"44.85960776","4":"K","5":"4","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"pumpkins","2":"saved","3":"76.93241952","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"C","5":"20","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"G","5":"30","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"radish","2":"Garden Party Mix","3":"0.94578198","4":"H","5":"15","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"raspberries","2":"perrenial","3":"1.85849466","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"rutabaga","2":"Improved Helenor","3":"29.74032380","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"H","5":"50","6":"2020-05-16","7":"FALSE","8":"NA"},{"1":"spinach","2":"Catalina","3":"2.03486426","4":"E","5":"100","6":"2020-06-20","7":"FALSE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Blue (saved)","3":"41.52401770","4":"B","5":"8","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"delicata","3":"10.49840044","4":"K","5":"8","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"B","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Red Kuri","3":"22.73183682","4":"side","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"A","5":"4","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"squash","2":"Waltham Butternut","3":"24.27066158","4":"K","5":"6","6":"2020-05-25","7":"TRUE","8":"NA"},{"1":"strawberries","2":"perrenial","3":"1.30513504","4":"NA","5":"NA","6":"<NA>","7":"NA","8":"NA"},{"1":"Swiss chard","2":"Neon Glow","3":"6.88282364","4":"M","5":"25","6":"2020-05-02","7":"FALSE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Amish Paste","3":"65.67342518","4":"N","5":"2","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Better Boy","3":"34.00846812","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Big Beef","3":"24.99377694","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Black Krim","3":"15.80712540","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Bonny Best","3":"24.92322910","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Brandywine","3":"15.64618814","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Cherokee Purple","3":"15.71232674","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"grape","3":"32.39468628","4":"O","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Jet Star","3":"15.02448530","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"died"},{"1":"tomatoes","2":"Mortgage Lifter","3":"26.32536742","4":"N","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"Old German","3":"26.71778978","4":"J","5":"1","6":"2020-05-20","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"N","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"J","5":"1","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"front","5":"5","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"tomatoes","2":"volunteers","3":"51.61235882","4":"O","5":"2","6":"2020-06-03","7":"TRUE","8":"NA"},{"1":"zucchini","2":"Romanesco","3":"99.70834874","4":"D","5":"3","6":"2020-05-21","7":"TRUE","8":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

The problem is that there are vegetables of the same variety that were planted in different plots in the garden_planting dataa set so we get duplicate rows of the weights for the varieties planted in different plots, for example, Chinese Red Noodle beans. We could fix this by either summarizing out the plot variable in the garden_planting data set, or changing the plot variable to plots to be able to store multiple plots in one data instance, for example, "M,D"

  3. I would like to understand how much money I "saved" by gardening, for each vegetable type. Describe how I could use the `garden_harvest` and `garden_spending` datasets, along with data from somewhere like [this](https://products.wholefoodsmarket.com/search?sort=relevance&store=10542) to answer this question. You can answer this in words, referencing various join functions. You don't need R code but could provide some if it's helpful.

We could compare the price per pound of the grocery items at whole foods and multiply that by the total weight in pounds of the harvested vegetables to get the price they would cost at the store. We can then find the difference between that price and the total price spent on the seeds. However, looking at the garden_spending dataset, we don't have the total price spent on seeds so we could instead divide the total weight of the harvested vegetables by the average weight of the vegetable to get an approximation for how many of each vegetable was harvested. From there we can calculate how many seeds are needed to grow that many vegetables and multiply that by the price per seed to get the total spent on seeds.

  4. Subset the data to tomatoes. Reorder the tomato varieties from smallest to largest first harvest date. Create a barplot of total harvest in pounds for each variety, in the new order.CHALLENGE: add the date near the end of the bar. (This is probably not a super useful graph because it's difficult to read. This is more an exercise in using some of the functions you just learned.)


```r
garden_harvest %>%
  filter(vegetable == "tomatoes") %>%
  group_by(variety) %>%
  mutate(total_wt_lbs = sum(weight)*0.00220462) %>%
  slice_min(date) %>%
  arrange(date) %>%
  ggplot(aes(x = fct_reorder(variety, date), y = total_wt_lbs)) +
  geom_col() + 
  labs(title = "Total weight in pounds for each tomato variety ordered by first harvest date", x = "Variety", y = "Total Weight in Pounds") +
  theme(axis.text = element_text(size = 6))
```

![](03_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

  5. In the `garden_harvest` data, create two new variables: one that makes the varieties lowercase and another that finds the length of the variety name. Arrange the data by vegetable and length of variety name (smallest to largest), with one row for each vegetable variety. HINT: use `str_to_lower()`, `str_length()`, and `distinct()`.
  

```r
garden_harvest %>%
  mutate(variety_lower = str_to_lower(variety)) %>%
  mutate(variety_length = str_length(variety)) %>%
  select(vegetable, variety, variety_lower, variety_length) %>%
  distinct() %>%
  arrange(vegetable, variety_length)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["variety_lower"],"name":[3],"type":["chr"],"align":["left"]},{"label":["variety_length"],"name":[4],"type":["int"],"align":["right"]}],"data":[{"1":"apple","2":"unknown","3":"unknown","4":"7"},{"1":"asparagus","2":"asparagus","3":"asparagus","4":"9"},{"1":"basil","2":"Isle of Naxos","3":"isle of naxos","4":"13"},{"1":"beans","2":"Bush Bush Slender","3":"bush bush slender","4":"17"},{"1":"beans","2":"Chinese Red Noodle","3":"chinese red noodle","4":"18"},{"1":"beans","2":"Classic Slenderette","3":"classic slenderette","4":"19"},{"1":"beets","2":"leaves","3":"leaves","4":"6"},{"1":"beets","2":"Sweet Merlin","3":"sweet merlin","4":"12"},{"1":"beets","2":"Gourmet Golden","3":"gourmet golden","4":"14"},{"1":"broccoli","2":"Yod Fah","3":"yod fah","4":"7"},{"1":"broccoli","2":"Main Crop Bravado","3":"main crop bravado","4":"17"},{"1":"carrots","2":"Dragon","3":"dragon","4":"6"},{"1":"carrots","2":"Bolero","3":"bolero","4":"6"},{"1":"carrots","2":"greens","3":"greens","4":"6"},{"1":"carrots","2":"King Midas","3":"king midas","4":"10"},{"1":"chives","2":"perrenial","3":"perrenial","4":"9"},{"1":"cilantro","2":"cilantro","3":"cilantro","4":"8"},{"1":"corn","2":"Dorinny Sweet","3":"dorinny sweet","4":"13"},{"1":"corn","2":"Golden Bantam","3":"golden bantam","4":"13"},{"1":"cucumbers","2":"pickling","3":"pickling","4":"8"},{"1":"edamame","2":"edamame","3":"edamame","4":"7"},{"1":"hot peppers","2":"thai","3":"thai","4":"4"},{"1":"hot peppers","2":"variety","3":"variety","4":"7"},{"1":"jalapeño","2":"giant","3":"giant","4":"5"},{"1":"kale","2":"Heirloom Lacinto","3":"heirloom lacinto","4":"16"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"crispy colors duo","4":"17"},{"1":"lettuce","2":"reseed","3":"reseed","4":"6"},{"1":"lettuce","2":"Tatsoi","3":"tatsoi","4":"6"},{"1":"lettuce","2":"mustard greens","3":"mustard greens","4":"14"},{"1":"lettuce","2":"Lettuce Mixture","3":"lettuce mixture","4":"15"},{"1":"lettuce","2":"Farmer's Market Blend","3":"farmer's market blend","4":"21"},{"1":"onions","2":"Delicious Duo","3":"delicious duo","4":"13"},{"1":"onions","2":"Long Keeping Rainbow","3":"long keeping rainbow","4":"20"},{"1":"peas","2":"Magnolia Blossom","3":"magnolia blossom","4":"16"},{"1":"peas","2":"Super Sugar Snap","3":"super sugar snap","4":"16"},{"1":"peppers","2":"green","3":"green","4":"5"},{"1":"peppers","2":"variety","3":"variety","4":"7"},{"1":"potatoes","2":"red","3":"red","4":"3"},{"1":"potatoes","2":"purple","3":"purple","4":"6"},{"1":"potatoes","2":"yellow","3":"yellow","4":"6"},{"1":"potatoes","2":"Russet","3":"russet","4":"6"},{"1":"pumpkins","2":"saved","3":"saved","4":"5"},{"1":"pumpkins","2":"New England Sugar","3":"new england sugar","4":"17"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"cinderella's carraige","4":"21"},{"1":"radish","2":"Garden Party Mix","3":"garden party mix","4":"16"},{"1":"raspberries","2":"perrenial","3":"perrenial","4":"9"},{"1":"rutabaga","2":"Improved Helenor","3":"improved helenor","4":"16"},{"1":"spinach","2":"Catalina","3":"catalina","4":"8"},{"1":"squash","2":"delicata","3":"delicata","4":"8"},{"1":"squash","2":"Red Kuri","3":"red kuri","4":"8"},{"1":"squash","2":"Blue (saved)","3":"blue (saved)","4":"12"},{"1":"squash","2":"Waltham Butternut","3":"waltham butternut","4":"17"},{"1":"strawberries","2":"perrenial","3":"perrenial","4":"9"},{"1":"Swiss chard","2":"Neon Glow","3":"neon glow","4":"9"},{"1":"tomatoes","2":"grape","3":"grape","4":"5"},{"1":"tomatoes","2":"Big Beef","3":"big beef","4":"8"},{"1":"tomatoes","2":"Jet Star","3":"jet star","4":"8"},{"1":"tomatoes","2":"Bonny Best","3":"bonny best","4":"10"},{"1":"tomatoes","2":"Better Boy","3":"better boy","4":"10"},{"1":"tomatoes","2":"Old German","3":"old german","4":"10"},{"1":"tomatoes","2":"Brandywine","3":"brandywine","4":"10"},{"1":"tomatoes","2":"Black Krim","3":"black krim","4":"10"},{"1":"tomatoes","2":"volunteers","3":"volunteers","4":"10"},{"1":"tomatoes","2":"Amish Paste","3":"amish paste","4":"11"},{"1":"tomatoes","2":"Cherokee Purple","3":"cherokee purple","4":"15"},{"1":"tomatoes","2":"Mortgage Lifter","3":"mortgage lifter","4":"15"},{"1":"zucchini","2":"Romanesco","3":"romanesco","4":"9"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

  6. In the `garden_harvest` data, find all distinct vegetable varieties that have "er" or "ar" in their name. HINT: `str_detect()` with an "or" statement (use the | for "or") and `distinct()`.


```r
garden_harvest %>%
  select(vegetable, variety) %>%
  distinct() %>%
  filter(str_detect(variety, "er")|str_detect(variety, "ar"))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"radish","2":"Garden Party Mix"},{"1":"lettuce","2":"Farmer's Market Blend"},{"1":"peas","2":"Super Sugar Snap"},{"1":"chives","2":"perrenial"},{"1":"strawberries","2":"perrenial"},{"1":"asparagus","2":"asparagus"},{"1":"lettuce","2":"mustard greens"},{"1":"raspberries","2":"perrenial"},{"1":"beans","2":"Bush Bush Slender"},{"1":"beets","2":"Sweet Merlin"},{"1":"hot peppers","2":"variety"},{"1":"tomatoes","2":"Cherokee Purple"},{"1":"tomatoes","2":"Better Boy"},{"1":"peppers","2":"variety"},{"1":"tomatoes","2":"Mortgage Lifter"},{"1":"tomatoes","2":"Old German"},{"1":"tomatoes","2":"Jet Star"},{"1":"carrots","2":"Bolero"},{"1":"tomatoes","2":"volunteers"},{"1":"beans","2":"Classic Slenderette"},{"1":"pumpkins","2":"Cinderella's Carraige"},{"1":"squash","2":"Waltham Butternut"},{"1":"pumpkins","2":"New England Sugar"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


## Bicycle-Use Patterns

In this activity, you'll examine some factors that may influence the use of bicycles in a bike-renting program.  The data come from Washington, DC and cover the last quarter of 2014.

<center>

![A typical Capital Bikeshare station. This one is at Florida and California, next to Pleasant Pops.](https://www.macalester.edu/~dshuman1/data/112/bike_station.jpg){width="30%"}


![One of the vans used to redistribute bicycles to different stations.](https://www.macalester.edu/~dshuman1/data/112/bike_van.jpg){width="30%"}

</center>

Two data tables are available:

- `Trips` contains records of individual rentals
- `Stations` gives the locations of the bike rental stations

Here is the code to read in the data. We do this a little differently than usual, which is why it is included here rather than at the top of this file. To avoid repeatedly re-reading the files, start the data import chunk with `{r cache = TRUE}` rather than the usual `{r}`.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
Stations<-read_csv("http://www.macalester.edu/~dshuman1/data/112/DC-Stations.csv")
```

**NOTE:** The `Trips` data table is a random subset of 10,000 trips from the full quarterly data. Start with this small data table to develop your analysis commands. **When you have this working well, you should access the full data set of more than 600,000 events by removing `-Small` from the name of the `data_site`.**

### Temporal patterns

It's natural to expect that bikes are rented more at some times of day, some days of the week, some months of the year than others. The variable `sdate` gives the time (including the date) that the rental started. Make the following plots and interpret them:

  7. A density plot, which is a smoothed out histogram, of the events versus `sdate`. Use `geom_density()`.
  

```r
Trips %>%
  ggplot() +
  geom_density(aes(x = sdate)) +
  labs(title = "Bike Rentals by Date", x = "Date", y = "Density")
```

![](03_exercises_files/figure-html/unnamed-chunk-7-1.png)<!-- -->
  
More bikes are rented during the months of October and Novemeber as compared to December and January.
  
  8. A density plot of the events versus time of day.  You can use `mutate()` with `lubridate`'s  `hour()` and `minute()` functions to extract the hour of the day and minute within the hour from `sdate`. Hint: A minute is 1/60 of an hour, so create a variable where 3:30 is 3.5 and 3:45 is 3.75.
  

```r
Trips %>%
  mutate(time = hour(sdate) + minute(sdate)/60) %>%
  ggplot() + 
  geom_density(aes(x = time)) + 
  labs(title = "Bike Rentals by Time", y = "Density", x = "Time")
```

![](03_exercises_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

A majority of bikes are rented out between the hours of 8am and 8pm with large spikes at 8am and 6pm and a dip in between these times.
  
  9. A bar graph of the events versus day of the week. Put day on the y-axis.
  

```r
Trips %>%
  mutate(weekday = wday(sdate)) %>%
  ggplot() + 
  geom_bar(aes(y = weekday)) + 
  labs(title = "Bike Rentals by Day of the Week", y = "Day of the Week (1 = Sunday)")
```

![](03_exercises_files/figure-html/unnamed-chunk-9-1.png)<!-- -->
  
The least bikes are rented on Saturday and Sunday, while the days during the week have the most rentals, with Friday having the most, closely followed by Monday and Thursday.
  
  10. Facet your graph from exercise 8. by day of the week. Is there a pattern?
  

```r
Trips %>%
  mutate(time = hour(sdate) + minute(sdate)/60) %>%
  mutate(weekday = wday(sdate)) %>%
  ggplot() + 
  geom_density(aes(x = time)) +
  labs(title = "Bike Rental Times for Days of the Week (1 = Sunday)", y = "Density", x = "Time") +
  facet_wrap(vars(weekday))
```

![](03_exercises_files/figure-html/unnamed-chunk-10-1.png)<!-- -->
  
For the weekdays we see the spikes in rentals at times when people are going and leaving work, however for the weekend, we see a general trend of rentals during the middle of the day. 
  
The variable `client` describes whether the renter is a regular user (level `Registered`) or has not joined the bike-rental organization (`Causal`). The next set of exercises investigate whether these two different categories of users show different rental behavior and how `client` interacts with the patterns you found in the previous exercises. 

  11. Change the graph from exercise 10 to set the `fill` aesthetic for `geom_density()` to the `client` variable. You should also set `alpha = .5` for transparency and `color=NA` to suppress the outline of the density function.
  

```r
Trips %>%
  mutate(time = hour(sdate) + minute(sdate)/60) %>%
  mutate(weekday = wday(sdate)) %>%
  ggplot() + 
  geom_density(aes(x = time, fill = client), alpha = .5, color = NA) +
  labs(title = "Bike Rental Times for Days of the Week by Client (1 = Sunday)", y = "Density", x = "Time") +
  facet_wrap(vars(weekday))
```

![](03_exercises_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

We see that it is the registered users who are using the bike rentals for commuting to and from work, while the casual users are using the bike rentals mostly during the middle of the day, regardless of the day of the week.

  12. Change the previous graph by adding the argument `position = position_stack()` to `geom_density()`. In your opinion, is this better or worse in terms of telling a story? What are the advantages/disadvantages of each?
  

```r
Trips %>%
  mutate(time = hour(sdate) + minute(sdate)/60) %>%
  mutate(weekday = wday(sdate)) %>%
  ggplot() + 
  geom_density(aes(x = time, fill = client), alpha = .5, color = NA, position = position_stack()) +
  labs(title = "Bike Rental Times for Days of the Week by Client (1 = Sunday)", y = "Density", x = "Time") +
  facet_wrap(vars(weekday))
```

![](03_exercises_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
  
In my opinion, this graph is worse at telling the story. It is much harder to see that it is really just the registered clients commuting to and from work with the bikes while the casual users are using it throughout the day. The advantage to the previous graph is we can individually see the distribution of each client while the advantage of this graph is to see the total distribution as well, but at the sacrifice of more information regarding the two categories of client.
  
  13. In this graph, go back to using the regular density plot (without `position = position_stack()`). Add a new variable to the dataset called `weekend` which will be "weekend" if the day is Saturday or Sunday and  "weekday" otherwise (HINT: use the `ifelse()` function and the `wday()` function from `lubridate`). Then, update the graph from the previous problem by faceting on the new `weekend` variable. 
  

```r
Trips %>%
  mutate(time = hour(sdate) + minute(sdate)/60) %>%
  mutate(weekday = wday(sdate)) %>%
  mutate(weekend = weekday == 1 | weekday == 7) %>%
  ggplot() + 
  geom_density(aes(x = time, fill = client), alpha = .5, color = NA) +
  labs(title = "Bike Rental Times for Weekend vs Weekday by Client (TRUE = Weekend)", y = "Density", x = "Time") +
  facet_wrap(vars(weekend))
```

![](03_exercises_files/figure-html/unnamed-chunk-13-1.png)<!-- -->
  
Here we can see a similar conclusion to our previous graphs, but in a more compact and clear way. The registered riders are using the bikes during the week for commuting to work, but also using the bikes during the weekend during the day, while the casual riders are using the bikes during both the week and weekend for use during the day.
  
  14. Change the graph from the previous problem to facet on `client` and fill with `weekday`. What information does this graph tell you that the previous didn't? Is one graph better than the other?
  

```r
Trips %>%
  mutate(time = hour(sdate) + minute(sdate)/60) %>%
  mutate(weekday = wday(sdate)) %>%
  mutate(weekend = weekday == 1 | weekday == 7) %>%
  ggplot() + 
  geom_density(aes(x = time, fill = weekend), alpha = .5, color = NA) +
  labs(title = "Bike Rental Times for Client Type by Weekend vs Weekday (TRUE = Weekend)", y = "Density", x = "Time") +
  facet_wrap(vars(client))
```

![](03_exercises_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
  
This plot allows us to directly compare the distributions of weekend vs weekday bike rentals as supposed to more directly comparing client types. Neither graph is better than the other in my opinion. I would choose the first graph to emphasize the comparison between client type as those distributions are overlapping, while using the second graph to emphasize weekend vs weekday.
  
### Spatial patterns

  15. Use the latitude and longitude variables in `Stations` to make a visualization of the total number of departures from each station in the `Trips` data. Use either color or size to show the variation in number of departures. We will improve this plot next week when we learn about maps!
  

```r
Trips %>%
  left_join(Stations, by = c("sstation" = "name")) %>%
  group_by(lat, long) %>%
  mutate(Departures = n()) %>%
  ggplot() + 
  geom_jitter(aes(x = long, y = lat, color = Departures)) + 
  labs(title = "Latitude and longitude of stations colored by number of departures", x = "longitude", y = "latitude")
```

![](03_exercises_files/figure-html/unnamed-chunk-15-1.png)<!-- -->
  
  16. Only 14.4% of the trips in our data are carried out by casual users. Create a plot that shows which area(s) have stations with a much higher percentage of departures by casual users. What patterns do you notice? (Again, we'll improve this next week when we learn about maps).
  

```r
Trips %>%
  left_join(Stations, by = c("sstation" = "name")) %>%
  group_by(lat, long) %>%
  mutate(Departures = n()) %>%
  group_by(lat, long) %>%
  mutate(percentCasual = sum(client=="Casual")/(sum(client=="Registered")+sum(client=="Casual"))) %>%
  ggplot() + 
  geom_jitter(aes(x = long, y = lat, color = Departures, size = percentCasual)) + 
  labs(title = "Latitude and longitude of stations colored by number of departures, sized by ratio of casual users", x = "longitude", y = "latitude")
```

![](03_exercises_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
  
**DID YOU REMEMBER TO GO BACK AND CHANGE THIS SET OF EXERCISES TO THE LARGER DATASET? IF NOT, DO THAT NOW.**

## Dogs!

In this section, we'll use the data from 2022-02-01 Tidy Tuesday. If you didn't use that data or need a little refresher on it, see the [website](https://github.com/rfordatascience/tidytuesday/blob/master/data/2022/2022-02-01/readme.md).

  17. The final product of this exercise will be a graph that has breed on the y-axis and the sum of the numeric ratings in the `breed_traits` dataset on the x-axis, with a dot for each rating. First, create a new dataset called `breed_traits_total` that has two variables -- `Breed` and `total_rating`. The `total_rating` variable is the sum of the numeric ratings in the `breed_traits` dataset (we'll use this dataset again in the next problem). Then, create the graph just described. Omit Breeds with a `total_rating` of 0 and order the Breeds from highest to lowest ranked. You may want to adjust the `fig.height` and `fig.width` arguments inside the code chunk options (eg. `{r, fig.height=8, fig.width=4}`) so you can see things more clearly - check this after you knit the file to assure it looks like what you expected.


```r
breed_traits_total <- breed_traits %>%
  mutate(total_rating = rowSums(.[2:7]) + rowSums(.[10:17])) %>%
  summarize(Breed, total_rating)
breed_traits_total %>%
  filter(total_rating>0) %>%
  ggplot() + 
  geom_col(aes(x = total_rating, y = fct_reorder(Breed, total_rating))) +
  labs(title = "Dog Breeds sorted by Total Rating", y = "Breed", x = "Total Rating")
```

![](03_exercises_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

  18. The final product of this exercise will be a graph with the top-20 dogs in total ratings (from previous problem) on the y-axis, year on the x-axis, and points colored by each breed's ranking for that year (from the `breed_rank_all` dataset). The points within each breed will be connected by a line, and the breeds should be arranged from the highest median rank to lowest median rank ("highest" is actually the smallest numer, eg. 1 = best). After you're finished, think of AT LEAST one thing you could you do to make this graph better. HINTS: 1. Start with the `breed_rank_all` dataset and pivot it so year is a variable. 2. Use the `separate()` function to get year alone, and there's an extra argument in that function that can make it numeric. 3. For both datasets used, you'll need to `str_squish()` Breed before joining. 
  

```r
breed_rank_all %>%
  select(-links, -Image) %>%
  pivot_longer(cols = -Breed, names_to = "year", values_to = "rank") %>%
  separate(year, into = "Year") %>%
  left_join(breed_traits_total, by = c("Breed")) %>%
  arrange(desc(total_rating)) %>%
  filter(total_rating > 48) %>%
  ggplot(aes(y = fct_reorder(Breed, rank), x = Year, color = rank)) + 
  geom_point() +
  geom_line()
```

![](03_exercises_files/figure-html/unnamed-chunk-18-1.png)<!-- -->
  
  19. Create your own! Requirements: use a `join` or `pivot` function (or both, if you'd like), a `str_XXX()` function, and a `fct_XXX()` function to create a graph using any of the dog datasets. One suggestion is to try to improve the graph you created for the Tidy Tuesday assignment. If you want an extra challenge, find a way to use the dog images in the `breed_rank_all` file - check out the `ggimage` library and [this resource](https://wilkelab.org/ggtext/) for putting images as labels.
  

  
## GitHub link

  20. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 03_exercises.Rmd, provide a link to the 03_exercises.md file, which is the one that will be most readable on GitHub.
  
https://github.com/jawhitelaw/stat112_03_exercises/blob/main/03_exercises.Rmd

## Challenge problem! 

This problem uses the data from the Tidy Tuesday competition this week, `kids`. If you need to refresh your memory on the data, read about it [here](https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-09-15/readme.md). 

  21. In this exercise, you are going to try to replicate the graph below, created by Georgios Karamanis. I'm sure you can find the exact code on GitHub somewhere, but **DON'T DO THAT!** You will only be graded for putting an effort into this problem. So, give it a try and see how far you can get without doing too much googling. HINT: use `facet_geo()`. The graphic won't load below since it came from a location on my computer. So, you'll have to reference the original html on the moodle page to see it.
  

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
