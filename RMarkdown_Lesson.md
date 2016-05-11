# Creating Dynamic Documents with RMarkdown and Knitr
Marian L. Schmidt, `@micro_marian`  
May 11th, 2016  







**While you're waiting for the workshop to start, please do the following:**

1. Open up **RStudio**.  
2. Install and download the **devtools** R package by running the following in the R console. 


```r
install.packages("devtools") # To include the session information
library("devtools") # To include the session information  
```

3. Check that you have the correct versions of **R** and **RStudio** by running `devtools::session_info()` in the R console.  
Here, `devtools::` tells **R** to use the `session_info()` function within the **devtools** package rather than the `sessionInfo()` function within the **utils** package.  Running `devtools::session_info()` allows us to see the version of **R** and **RStudio**. 

Do you have the following versions of **R** and **RStudio**?  

- **R:** `Version 3.3.0 (2016-05-03)`  
- **RStudio:** `0.99.1172`  

  - If you **do** then you are good to go!  
  - If you **do not** have the correct versions of **R** and **RStudio**, please follow the 
<a href="http://umswc.github.io/2016-05-11-RMarkdown/" target="_blank">Setup</a> directions on our course page.  

4. Install other R packages necessary for the workshop.


```r
## Install the correct packages
install.packages("rmarkdown") # Make cool dynamic documents
install.packages("knitr") # Run R Code Chunks
install.packages("ggplot2") # For plotting
install.packages("DT") # Interactive HTML tables
```


```r
## Add these packages to the current session to make sure they load correctly
library("rmarkdown") # Make cool dynamic documents
library("knitr") # Run R Code Chunks
library("ggplot2") # For plotting
library("DT") # Interactive HTML tables
```

5. If your packages loaded without any errors, then you are ready for the workshop!  

If there were issues with the above packages please place a pink sticky to get help from a helper!


******************************************************************************************

# Welcome!

This tutorial was constructed as a part of Dr. C Titus Brown's <a href="http://dib-training.readthedocs.io/en/pub/index.html" target="_blank">Data Intensive Biology (DIB)</a> training program at the University of California, Davis.  The DIB training program is hosting several local+remote workshops covering topics in bioinformatic toos.

Today, there is are three remote classrooms tuning in:  

1. University of California, Davis  
2. Simon Fraser University in British Columbia, Canada  
3. Ontario Institute for Cancer Research

In addition, the audio of this tutorial and the screen of the main instructor will be shared on YouTube.

The Github repository for this website can <a href="https://github.com/marschmi/RMarkdown_May11_2016" target="_blank">be found here</a>.  



******************************************************************************************

# Amazing Resources

I could **not** have made this tutorial without these amazing resources:  

1. The <a href="http://rmarkdown.rstudio.com/index.html" target="_blank">RMarkdown</a> website hosted by RStudio.  
2. Dr. Yuhui Xie's book:  <a href="http://www.amazon.com/Dynamic-Documents-knitr-Chapman-Hall/dp/1482203537" target="_blank">Dynamic Documents with R and Knitr</a> 2^nd^ Edition [@Xie2015] and his <a href="http://yihui.name/knitr/" target="_blank">Knitr</a> website.
3. Dr. Karl Broman's <a href="http://kbroman.org/knitr_knutshell/" target="_blank">"Knitr in a Knutshell"</a>.  
4. <a href="https://www.rstudio.com/resources/cheatsheets/" target="_blank">Cheatsheets</a> released by RStudio.


******************************************************************************************

# Dynamic Documents  

<a href="https://en.wikipedia.org/wiki/Literate_programming">Literate programming</a> is the basic idea behind dynamic documents and was proposed by Donald Knuth in 1984.  Originally, it was for mixing the source code and documentation of software development together.  Today, we will create dynamic documents in which program or analysis code is run to produce output (e.g. tables, plots, models, etc) and then are explained through narrative writing.

The 3 steps of **Literate Programming**:  

1. **Parse** the source document and separate code from narratives.  
2. **Execute** source code and return results.  
3. **Mix** results from the source code with the original narratives.  


So that leaves 2 steps for us which includes writing:  

1. Analysis code  
2. A narrative to explain the results from the analysis code.  

Traditionally, people used comments to include narrative in their code file (for **R** that would be a `.R` file). For example, the file may include the following:


```r
# Title:  Relationship of Car Weight and Gas Efficiency 
# By:  Marian Schmidt  
# Date:  May 11th, 2016

# I predict that there will be a relationship between weight and miles per gallon.  
# I will test this by running a linear model with the mtcars dataset is within the R datasets

# What does the data look like?
datatable(mtcars) # Interactive table 

# Is there a relationship between the weight of a car and the miles per gallon?
lm_mpg <- lm(mpg ~ wt, data = mtcars) # Run linear model predicting mpg based on wt
coef_lm_mpg <- coef(summary(lm_mpg)) # Extract coefficients to table 
kable(coef_lm_mpg) # Produce non-interactive table - function in knitr

# Plot the relationship between weight and miles per gallon  
plot <- ggplot(mtcars, aes(x = wt, y = mpg)) + geom_point() + # 
  geom_smooth(method = "lm") + theme_bw() + # add linear model and make black and white
  xlab("Weight (1000lbs)") + ylab("Miles per Gallon") # Add axis labels
ggplotly(plot) # Make the plot interactive  

# It appears that with a gain in every 1000 pounds that there will be a decrease in fuel efficiency by 5.34 miles per gallon
# The end
```

Then the user would have to read the comments and run the code themselves.  

However, literate programming allows us to run the code and describe the code all within one document that we could share.  So for example we could do the following:  

***

**Relationship of Car Weight and Gas Efficiency**  
*By:* Marian Schmidt  
*Date:* May 11th, 2016  

I predict that there will be a relationship between weight and miles per gallon.  

I will test this by running a linear model with the mtcars dataset is within the R datasets.


```r
# What does the data look like?
datatable(mtcars) # Interactive table 
```

<!--html_preserve--><div id="htmlwidget-8729" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-8729">{"x":{"data":[["Mazda RX4","Mazda RX4 Wag","Datsun 710","Hornet 4 Drive","Hornet Sportabout","Valiant","Duster 360","Merc 240D","Merc 230","Merc 280","Merc 280C","Merc 450SE","Merc 450SL","Merc 450SLC","Cadillac Fleetwood","Lincoln Continental","Chrysler Imperial","Fiat 128","Honda Civic","Toyota Corolla","Toyota Corona","Dodge Challenger","AMC Javelin","Camaro Z28","Pontiac Firebird","Fiat X1-9","Porsche 914-2","Lotus Europa","Ford Pantera L","Ferrari Dino","Maserati Bora","Volvo 142E"],[21,21,22.8,21.4,18.7,18.1,14.3,24.4,22.8,19.2,17.8,16.4,17.3,15.2,10.4,10.4,14.7,32.4,30.4,33.9,21.5,15.5,15.2,13.3,19.2,27.3,26,30.4,15.8,19.7,15,21.4],[6,6,4,6,8,6,8,4,4,6,6,8,8,8,8,8,8,4,4,4,4,8,8,8,8,4,4,4,8,6,8,4],[160,160,108,258,360,225,360,146.7,140.8,167.6,167.6,275.8,275.8,275.8,472,460,440,78.7,75.7,71.1,120.1,318,304,350,400,79,120.3,95.1,351,145,301,121],[110,110,93,110,175,105,245,62,95,123,123,180,180,180,205,215,230,66,52,65,97,150,150,245,175,66,91,113,264,175,335,109],[3.9,3.9,3.85,3.08,3.15,2.76,3.21,3.69,3.92,3.92,3.92,3.07,3.07,3.07,2.93,3,3.23,4.08,4.93,4.22,3.7,2.76,3.15,3.73,3.08,4.08,4.43,3.77,4.22,3.62,3.54,4.11],[2.62,2.875,2.32,3.215,3.44,3.46,3.57,3.19,3.15,3.44,3.44,4.07,3.73,3.78,5.25,5.424,5.345,2.2,1.615,1.835,2.465,3.52,3.435,3.84,3.845,1.935,2.14,1.513,3.17,2.77,3.57,2.78],[16.46,17.02,18.61,19.44,17.02,20.22,15.84,20,22.9,18.3,18.9,17.4,17.6,18,17.98,17.82,17.42,19.47,18.52,19.9,20.01,16.87,17.3,15.41,17.05,18.9,16.7,16.9,14.5,15.5,14.6,18.6],[0,0,1,1,0,1,0,1,1,1,1,0,0,0,0,0,0,1,1,1,1,0,0,0,0,1,0,1,0,0,0,1],[1,1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,1,1,0,0,0,0,0,1,1,1,1,1,1,1],[4,4,4,3,3,3,3,4,4,4,4,3,3,3,3,3,3,4,4,4,3,3,3,3,3,4,5,5,5,5,5,4],[4,4,1,1,2,1,4,2,2,4,4,3,3,3,4,4,4,1,2,1,1,2,2,4,2,1,2,2,4,6,8,2]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>mpg\u003c/th>\n      <th>cyl\u003c/th>\n      <th>disp\u003c/th>\n      <th>hp\u003c/th>\n      <th>drat\u003c/th>\n      <th>wt\u003c/th>\n      <th>qsec\u003c/th>\n      <th>vs\u003c/th>\n      <th>am\u003c/th>\n      <th>gear\u003c/th>\n      <th>carb\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4,5,6,7,8,9,10,11]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false},"callback":null,"filter":"none"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
# Is there a relationship between the weight of a car and the miles per gallon?
lm_mpg <- lm(mpg ~ wt, data = mtcars) # Run linear model predicting mpg based on wt
coef_lm_mpg <- coef(summary(lm_mpg)) # Extract coefficients to table 
kable(coef_lm_mpg) # Produce non-interactive table - function in knitr
```

                Estimate   Std. Error     t value   Pr(>|t|)
------------  ----------  -----------  ----------  ---------
(Intercept)    37.285126     1.877627   19.857575          0
wt             -5.344472     0.559101   -9.559044          0

```r
# Plot the relationship between weight and miles per gallon  
plot <- ggplot(mtcars, aes(x = wt, y = mpg)) + geom_point() + # 
  geom_smooth(method = "lm") + theme_bw() + # add linear model and make black and white
  xlab("Weight (1000lbs)") + ylab("Miles per Gallon") # Add axis labels
ggplotly(plot) # Make the plot interactive
```

<!--html_preserve--><div id="htmlwidget-3605" style="width:432px;height:432px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-3605">{"x":{"data":[{"x":[2.62,2.875,2.32,3.215,3.44,3.46,3.57,3.19,3.15,3.44,3.44,4.07,3.73,3.78,5.25,5.424,5.345,2.2,1.615,1.835,2.465,3.52,3.435,3.84,3.845,1.935,2.14,1.513,3.17,2.77,3.57,2.78],"y":[21,21,22.8,21.4,18.7,18.1,14.3,24.4,22.8,19.2,17.8,16.4,17.3,15.2,10.4,10.4,14.7,32.4,30.4,33.9,21.5,15.5,15.2,13.3,19.2,27.3,26,30.4,15.8,19.7,15,21.4],"text":["wt: 2.62<br>mpg: 21","wt: 2.88<br>mpg: 21","wt: 2.32<br>mpg: 22.8","wt: 3.21<br>mpg: 21.4","wt: 3.44<br>mpg: 18.7","wt: 3.46<br>mpg: 18.1","wt: 3.57<br>mpg: 14.3","wt: 3.19<br>mpg: 24.4","wt: 3.15<br>mpg: 22.8","wt: 3.44<br>mpg: 19.2","wt: 3.44<br>mpg: 17.8","wt: 4.07<br>mpg: 16.4","wt: 3.73<br>mpg: 17.3","wt: 3.78<br>mpg: 15.2","wt: 5.25<br>mpg: 10.4","wt: 5.42<br>mpg: 10.4","wt: 5.34<br>mpg: 14.7","wt: 2.2<br>mpg: 32.4","wt: 1.62<br>mpg: 30.4","wt: 1.84<br>mpg: 33.9","wt: 2.46<br>mpg: 21.5","wt: 3.52<br>mpg: 15.5","wt: 3.44<br>mpg: 15.2","wt: 3.84<br>mpg: 13.3","wt: 3.85<br>mpg: 19.2","wt: 1.94<br>mpg: 27.3","wt: 2.14<br>mpg: 26","wt: 1.51<br>mpg: 30.4","wt: 3.17<br>mpg: 15.8","wt: 2.77<br>mpg: 19.7","wt: 3.57<br>mpg: 15","wt: 2.78<br>mpg: 21.4"],"key":null,"type":"scatter","mode":"markers","marker":{"autocolorscale":false,"color":"rgb(0,0,0)","opacity":1,"size":5.66929133858268,"symbol":"circle","line":{"width":1.88976377952756,"color":"rgb(0,0,0)"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","name":""},{"x":[1.513,1.56250632911392,1.61201265822785,1.66151898734177,1.7110253164557,1.76053164556962,1.81003797468354,1.85954430379747,1.90905063291139,1.95855696202532,2.00806329113924,2.05756962025316,2.10707594936709,2.15658227848101,2.20608860759494,2.25559493670886,2.30510126582278,2.35460759493671,2.40411392405063,2.45362025316456,2.50312658227848,2.55263291139241,2.60213924050633,2.65164556962025,2.70115189873418,2.7506582278481,2.80016455696203,2.84967088607595,2.89917721518987,2.9486835443038,2.99818987341772,3.04769620253165,3.09720253164557,3.14670886075949,3.19621518987342,3.24572151898734,3.29522784810127,3.34473417721519,3.39424050632911,3.44374683544304,3.49325316455696,3.54275949367089,3.59226582278481,3.64177215189873,3.69127848101266,3.74078481012658,3.79029113924051,3.83979746835443,3.88930379746835,3.93881012658228,3.9883164556962,4.03782278481013,4.08732911392405,4.13683544303798,4.1863417721519,4.23584810126582,4.28535443037975,4.33486075949367,4.3843670886076,4.43387341772152,4.48337974683544,4.53288607594937,4.58239240506329,4.63189873417722,4.68140506329114,4.73091139240506,4.78041772151899,4.82992405063291,4.87943037974684,4.92893670886076,4.97844303797468,5.02794936708861,5.07745569620253,5.12696202531646,5.17646835443038,5.2259746835443,5.27548101265823,5.32498734177215,5.37449367088608,5.424],"y":[29.1989406778126,28.9343555091934,28.6697703405742,28.4051851719549,28.1406000033357,27.8760148347165,27.6114296660973,27.3468444974781,27.0822593288588,26.8176741602396,26.5530889916204,26.2885038230012,26.023918654382,25.7593334857627,25.4947483171435,25.2301631485243,24.9655779799051,24.7009928112859,24.4364076426666,24.1718224740474,23.9072373054282,23.642652136809,23.3780669681898,23.1134817995705,22.8488966309513,22.5843114623321,22.3197262937129,22.0551411250937,21.7905559564744,21.5259707878552,21.261385619236,20.9968004506168,20.7322152819976,20.4676301133783,20.2030449447591,19.9384597761399,19.6738746075207,19.4092894389015,19.1447042702822,18.880119101663,18.6155339330438,18.3509487644246,18.0863635958054,17.8217784271861,17.5571932585669,17.2926080899477,17.0280229213285,16.7634377527093,16.49885258409,16.2342674154708,15.9696822468516,15.7050970782324,15.4405119096132,15.1759267409939,14.9113415723747,14.6467564037555,14.3821712351363,14.1175860665171,13.8530008978978,13.5884157292786,13.3238305606594,13.0592453920402,12.794660223421,12.5300750548017,12.2654898861825,12.0009047175633,11.7363195489441,11.4717343803249,11.2071492117056,10.9425640430864,10.6779788744672,10.413393705848,10.1488085372288,9.88422336860955,9.61963819999033,9.35505303137111,9.09046786275189,8.82588269413267,8.56129752551345,8.29671235689423],"text":["wt: 1.51<br>mpg: 29.2","wt: 1.56<br>mpg: 28.93","wt: 1.61<br>mpg: 28.67","wt: 1.66<br>mpg: 28.41","wt: 1.71<br>mpg: 28.14","wt: 1.76<br>mpg: 27.88","wt: 1.81<br>mpg: 27.61","wt: 1.86<br>mpg: 27.35","wt: 1.91<br>mpg: 27.08","wt: 1.96<br>mpg: 26.82","wt: 2.01<br>mpg: 26.55","wt: 2.06<br>mpg: 26.29","wt: 2.11<br>mpg: 26.02","wt: 2.16<br>mpg: 25.76","wt: 2.21<br>mpg: 25.49","wt: 2.26<br>mpg: 25.23","wt: 2.31<br>mpg: 24.97","wt: 2.35<br>mpg: 24.7","wt: 2.4<br>mpg: 24.44","wt: 2.45<br>mpg: 24.17","wt: 2.5<br>mpg: 23.91","wt: 2.55<br>mpg: 23.64","wt: 2.6<br>mpg: 23.38","wt: 2.65<br>mpg: 23.11","wt: 2.7<br>mpg: 22.85","wt: 2.75<br>mpg: 22.58","wt: 2.8<br>mpg: 22.32","wt: 2.85<br>mpg: 22.06","wt: 2.9<br>mpg: 21.79","wt: 2.95<br>mpg: 21.53","wt: 3<br>mpg: 21.26","wt: 3.05<br>mpg: 21","wt: 3.1<br>mpg: 20.73","wt: 3.15<br>mpg: 20.47","wt: 3.2<br>mpg: 20.2","wt: 3.25<br>mpg: 19.94","wt: 3.3<br>mpg: 19.67","wt: 3.34<br>mpg: 19.41","wt: 3.39<br>mpg: 19.14","wt: 3.44<br>mpg: 18.88","wt: 3.49<br>mpg: 18.62","wt: 3.54<br>mpg: 18.35","wt: 3.59<br>mpg: 18.09","wt: 3.64<br>mpg: 17.82","wt: 3.69<br>mpg: 17.56","wt: 3.74<br>mpg: 17.29","wt: 3.79<br>mpg: 17.03","wt: 3.84<br>mpg: 16.76","wt: 3.89<br>mpg: 16.5","wt: 3.94<br>mpg: 16.23","wt: 3.99<br>mpg: 15.97","wt: 4.04<br>mpg: 15.71","wt: 4.09<br>mpg: 15.44","wt: 4.14<br>mpg: 15.18","wt: 4.19<br>mpg: 14.91","wt: 4.24<br>mpg: 14.65","wt: 4.29<br>mpg: 14.38","wt: 4.33<br>mpg: 14.12","wt: 4.38<br>mpg: 13.85","wt: 4.43<br>mpg: 13.59","wt: 4.48<br>mpg: 13.32","wt: 4.53<br>mpg: 13.06","wt: 4.58<br>mpg: 12.79","wt: 4.63<br>mpg: 12.53","wt: 4.68<br>mpg: 12.27","wt: 4.73<br>mpg: 12","wt: 4.78<br>mpg: 11.74","wt: 4.83<br>mpg: 11.47","wt: 4.88<br>mpg: 11.21","wt: 4.93<br>mpg: 10.94","wt: 4.98<br>mpg: 10.68","wt: 5.03<br>mpg: 10.41","wt: 5.08<br>mpg: 10.15","wt: 5.13<br>mpg: 9.88","wt: 5.18<br>mpg: 9.62","wt: 5.23<br>mpg: 9.36","wt: 5.28<br>mpg: 9.09","wt: 5.32<br>mpg: 8.83","wt: 5.37<br>mpg: 8.56","wt: 5.42<br>mpg: 8.3"],"type":"scatter","mode":"lines","name":"fitted values","line":{"width":3.77952755905512,"color":"rgb(51,102,255)","dash":"solid"},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text"},{"x":[1.513,1.56250632911392,1.61201265822785,1.66151898734177,1.7110253164557,1.76053164556962,1.81003797468354,1.85954430379747,1.90905063291139,1.95855696202532,2.00806329113924,2.05756962025316,2.10707594936709,2.15658227848101,2.20608860759494,2.25559493670886,2.30510126582278,2.35460759493671,2.40411392405063,2.45362025316456,2.50312658227848,2.55263291139241,2.60213924050633,2.65164556962025,2.70115189873418,2.7506582278481,2.80016455696203,2.84967088607595,2.89917721518987,2.9486835443038,2.99818987341772,3.04769620253165,3.09720253164557,3.14670886075949,3.19621518987342,3.24572151898734,3.29522784810127,3.34473417721519,3.39424050632911,3.44374683544304,3.49325316455696,3.54275949367089,3.59226582278481,3.64177215189873,3.69127848101266,3.74078481012658,3.79029113924051,3.83979746835443,3.88930379746835,3.93881012658228,3.9883164556962,4.03782278481013,4.08732911392405,4.13683544303798,4.1863417721519,4.23584810126582,4.28535443037975,4.33486075949367,4.3843670886076,4.43387341772152,4.48337974683544,4.53288607594937,4.58239240506329,4.63189873417722,4.68140506329114,4.73091139240506,4.78041772151899,4.82992405063291,4.87943037974684,4.92893670886076,4.97844303797468,5.02794936708861,5.07745569620253,5.12696202531646,5.17646835443038,5.2259746835443,5.27548101265823,5.32498734177215,5.37449367088608,5.424,5.424,5.424,5.37449367088608,5.32498734177215,5.27548101265823,5.2259746835443,5.17646835443038,5.12696202531646,5.07745569620253,5.02794936708861,4.97844303797468,4.92893670886076,4.87943037974684,4.82992405063291,4.78041772151899,4.73091139240506,4.68140506329114,4.63189873417722,4.58239240506329,4.53288607594937,4.48337974683544,4.43387341772152,4.3843670886076,4.33486075949367,4.28535443037975,4.23584810126582,4.1863417721519,4.13683544303798,4.08732911392405,4.03782278481013,3.9883164556962,3.93881012658228,3.88930379746835,3.83979746835443,3.79029113924051,3.74078481012658,3.69127848101266,3.64177215189873,3.59226582278481,3.54275949367089,3.49325316455696,3.44374683544304,3.39424050632911,3.34473417721519,3.29522784810127,3.24572151898734,3.19621518987342,3.14670886075949,3.09720253164557,3.04769620253165,2.99818987341772,2.9486835443038,2.89917721518987,2.84967088607595,2.80016455696203,2.7506582278481,2.70115189873418,2.65164556962025,2.60213924050633,2.55263291139241,2.50312658227848,2.45362025316456,2.40411392405063,2.35460759493671,2.30510126582278,2.25559493670886,2.20608860759494,2.15658227848101,2.10707594936709,2.05756962025316,2.00806329113924,1.95855696202532,1.90905063291139,1.85954430379747,1.81003797468354,1.76053164556962,1.7110253164557,1.66151898734177,1.61201265822785,1.56250632911392,1.513,1.513],"y":[31.4341217313206,31.1204993874088,30.8072470380303,30.4943905399572,30.1819579750846,29.8699798618842,29.5584893857579,29.2475226490581,28.9371189411946,28.6273210287383,28.3181754647066,28.0097329152165,27.7020485003464,27.3951821442752,27.0891989274822,26.7841694309033,26.4801700583702,26.1772833193625,25.8755980490886,25.5752095372801,25.276219531088,24.9787360715631,24.6828731181045,24.3887499120139,24.096490030255,23.8062200853351,23.5180680386105,23.2321611137817,22.9486233256676,22.6675726760022,22.3891181105935,22.1133563760938,21.8403689531932,21.5702192684414,21.3029503917649,21.0385834062175,20.7771165901197,20.5185254843377,20.2627638386563,20.0097653533643,19.7594460674074,19.5117072017912,19.266438250352,19.023520118358,18.7828281372767,18.5442348238948,18.307612296075,18.0728342993708,17.8397778342311,17.6083244005099,17.3783608941716,17.1497802013801,16.9224815391307,16.6963705909696,16.4713594827389,16.2473666380091,16.0243165469005,15.8021394760335,15.5807711417929,15.3601523641871,15.1402287143902,14.9209501655981,14.7022707540356,14.4841482547477,14.266543875108,14.0494219676868,13.8327497631713,13.6164971233358,13.400636313583,13.185141794249,12.96999002966,12.7551593138153,12.5406296115153,12.3263824137488,12.1124006061819,11.8986683496337,11.685170971489,11.4718948670624,11.2588274100015,11.0459568708893,5.5474678428992,5.5474678428992,5.86376764102542,6.17987052120298,6.49576475401476,6.81143771310854,7.1268757937988,7.44206432347029,7.75698746294225,8.07162809788066,8.38596771927446,8.69998629192387,9.01366210982825,9.32697163731397,9.6398893347169,9.95238746743981,10.2644358972571,10.5760018548558,10.8870496928064,11.1975406184823,11.5074324069286,11.8166790943701,12.1252306540028,12.4330326570007,12.740025923372,13.0461461695019,13.3513236620105,13.6554828910183,13.9585422800956,14.2604139550846,14.5610035995316,14.8602104304317,15.157927333949,15.4540412060478,15.748433546582,16.0409813560006,16.3315583798572,16.6200367360143,16.9062889412588,17.190190327058,17.4716217986802,17.7504728499618,18.0266447019082,18.3000533934652,18.5706326249216,18.8383361460623,19.1031394977534,19.3650409583153,19.624061610802,19.8802445251398,20.1336531278785,20.3843688997083,20.6324885872813,20.8781211364057,21.1213845488153,21.3624028393291,21.6013032316477,21.8382136871272,22.073260818275,22.3065682020549,22.5382550797684,22.7684354108148,22.9972172362447,23.2247023032092,23.4509859014399,23.6761568661453,23.9002977068049,24.1234848272503,24.3457888084175,24.5672747307859,24.7880025185342,25.0080272917409,25.2273997165231,25.4461663458981,25.6643699464367,25.8820498075489,26.0992420315869,26.3159798039527,26.532293643118,26.748211630978,26.9637596243046,31.4341217313206],"text":["wt: 1.51<br>mpg: 29.2","wt: 1.56<br>mpg: 28.93","wt: 1.61<br>mpg: 28.67","wt: 1.66<br>mpg: 28.41","wt: 1.71<br>mpg: 28.14","wt: 1.76<br>mpg: 27.88","wt: 1.81<br>mpg: 27.61","wt: 1.86<br>mpg: 27.35","wt: 1.91<br>mpg: 27.08","wt: 1.96<br>mpg: 26.82","wt: 2.01<br>mpg: 26.55","wt: 2.06<br>mpg: 26.29","wt: 2.11<br>mpg: 26.02","wt: 2.16<br>mpg: 25.76","wt: 2.21<br>mpg: 25.49","wt: 2.26<br>mpg: 25.23","wt: 2.31<br>mpg: 24.97","wt: 2.35<br>mpg: 24.7","wt: 2.4<br>mpg: 24.44","wt: 2.45<br>mpg: 24.17","wt: 2.5<br>mpg: 23.91","wt: 2.55<br>mpg: 23.64","wt: 2.6<br>mpg: 23.38","wt: 2.65<br>mpg: 23.11","wt: 2.7<br>mpg: 22.85","wt: 2.75<br>mpg: 22.58","wt: 2.8<br>mpg: 22.32","wt: 2.85<br>mpg: 22.06","wt: 2.9<br>mpg: 21.79","wt: 2.95<br>mpg: 21.53","wt: 3<br>mpg: 21.26","wt: 3.05<br>mpg: 21","wt: 3.1<br>mpg: 20.73","wt: 3.15<br>mpg: 20.47","wt: 3.2<br>mpg: 20.2","wt: 3.25<br>mpg: 19.94","wt: 3.3<br>mpg: 19.67","wt: 3.34<br>mpg: 19.41","wt: 3.39<br>mpg: 19.14","wt: 3.44<br>mpg: 18.88","wt: 3.49<br>mpg: 18.62","wt: 3.54<br>mpg: 18.35","wt: 3.59<br>mpg: 18.09","wt: 3.64<br>mpg: 17.82","wt: 3.69<br>mpg: 17.56","wt: 3.74<br>mpg: 17.29","wt: 3.79<br>mpg: 17.03","wt: 3.84<br>mpg: 16.76","wt: 3.89<br>mpg: 16.5","wt: 3.94<br>mpg: 16.23","wt: 3.99<br>mpg: 15.97","wt: 4.04<br>mpg: 15.71","wt: 4.09<br>mpg: 15.44","wt: 4.14<br>mpg: 15.18","wt: 4.19<br>mpg: 14.91","wt: 4.24<br>mpg: 14.65","wt: 4.29<br>mpg: 14.38","wt: 4.33<br>mpg: 14.12","wt: 4.38<br>mpg: 13.85","wt: 4.43<br>mpg: 13.59","wt: 4.48<br>mpg: 13.32","wt: 4.53<br>mpg: 13.06","wt: 4.58<br>mpg: 12.79","wt: 4.63<br>mpg: 12.53","wt: 4.68<br>mpg: 12.27","wt: 4.73<br>mpg: 12","wt: 4.78<br>mpg: 11.74","wt: 4.83<br>mpg: 11.47","wt: 4.88<br>mpg: 11.21","wt: 4.93<br>mpg: 10.94","wt: 4.98<br>mpg: 10.68","wt: 5.03<br>mpg: 10.41","wt: 5.08<br>mpg: 10.15","wt: 5.13<br>mpg: 9.88","wt: 5.18<br>mpg: 9.62","wt: 5.23<br>mpg: 9.36","wt: 5.28<br>mpg: 9.09","wt: 5.32<br>mpg: 8.83","wt: 5.37<br>mpg: 8.56","wt: 5.42<br>mpg: 8.3","wt: 5.42<br>mpg: 8.3","wt: 5.42<br>mpg: 8.3","wt: 5.37<br>mpg: 8.56","wt: 5.32<br>mpg: 8.83","wt: 5.28<br>mpg: 9.09","wt: 5.23<br>mpg: 9.36","wt: 5.18<br>mpg: 9.62","wt: 5.13<br>mpg: 9.88","wt: 5.08<br>mpg: 10.15","wt: 5.03<br>mpg: 10.41","wt: 4.98<br>mpg: 10.68","wt: 4.93<br>mpg: 10.94","wt: 4.88<br>mpg: 11.21","wt: 4.83<br>mpg: 11.47","wt: 4.78<br>mpg: 11.74","wt: 4.73<br>mpg: 12","wt: 4.68<br>mpg: 12.27","wt: 4.63<br>mpg: 12.53","wt: 4.58<br>mpg: 12.79","wt: 4.53<br>mpg: 13.06","wt: 4.48<br>mpg: 13.32","wt: 4.43<br>mpg: 13.59","wt: 4.38<br>mpg: 13.85","wt: 4.33<br>mpg: 14.12","wt: 4.29<br>mpg: 14.38","wt: 4.24<br>mpg: 14.65","wt: 4.19<br>mpg: 14.91","wt: 4.14<br>mpg: 15.18","wt: 4.09<br>mpg: 15.44","wt: 4.04<br>mpg: 15.71","wt: 3.99<br>mpg: 15.97","wt: 3.94<br>mpg: 16.23","wt: 3.89<br>mpg: 16.5","wt: 3.84<br>mpg: 16.76","wt: 3.79<br>mpg: 17.03","wt: 3.74<br>mpg: 17.29","wt: 3.69<br>mpg: 17.56","wt: 3.64<br>mpg: 17.82","wt: 3.59<br>mpg: 18.09","wt: 3.54<br>mpg: 18.35","wt: 3.49<br>mpg: 18.62","wt: 3.44<br>mpg: 18.88","wt: 3.39<br>mpg: 19.14","wt: 3.34<br>mpg: 19.41","wt: 3.3<br>mpg: 19.67","wt: 3.25<br>mpg: 19.94","wt: 3.2<br>mpg: 20.2","wt: 3.15<br>mpg: 20.47","wt: 3.1<br>mpg: 20.73","wt: 3.05<br>mpg: 21","wt: 3<br>mpg: 21.26","wt: 2.95<br>mpg: 21.53","wt: 2.9<br>mpg: 21.79","wt: 2.85<br>mpg: 22.06","wt: 2.8<br>mpg: 22.32","wt: 2.75<br>mpg: 22.58","wt: 2.7<br>mpg: 22.85","wt: 2.65<br>mpg: 23.11","wt: 2.6<br>mpg: 23.38","wt: 2.55<br>mpg: 23.64","wt: 2.5<br>mpg: 23.91","wt: 2.45<br>mpg: 24.17","wt: 2.4<br>mpg: 24.44","wt: 2.35<br>mpg: 24.7","wt: 2.31<br>mpg: 24.97","wt: 2.26<br>mpg: 25.23","wt: 2.21<br>mpg: 25.49","wt: 2.16<br>mpg: 25.76","wt: 2.11<br>mpg: 26.02","wt: 2.06<br>mpg: 26.29","wt: 2.01<br>mpg: 26.55","wt: 1.96<br>mpg: 26.82","wt: 1.91<br>mpg: 27.08","wt: 1.86<br>mpg: 27.35","wt: 1.81<br>mpg: 27.61","wt: 1.76<br>mpg: 27.88","wt: 1.71<br>mpg: 28.14","wt: 1.66<br>mpg: 28.41","wt: 1.61<br>mpg: 28.67","wt: 1.56<br>mpg: 28.93","wt: 1.51<br>mpg: 29.2","wt: 1.51<br>mpg: 29.2"],"type":"scatter","mode":"lines","line":{"width":3.77952755905512,"color":"transparent","dash":"solid"},"fill":"tozerox","fillcolor":"rgba(153,153,153,0.4)","hoverinfo":"x+y","showlegend":false,"xaxis":"x","yaxis":"y","name":""}],"layout":{"margin":{"b":40.6475716064757,"l":40.6475716064757,"t":23.9701120797011,"r":7.97011207970112},"plot_bgcolor":"rgb(255,255,255)","paper_bgcolor":"rgb(255,255,255)","font":{"color":"rgb(0,0,0)","family":"","size":15.9402241594022},"xaxis":{"type":"linear","autorange":false,"tickmode":"array","range":[1.31745,5.61955],"ticktext":["2","3","4","5"],"tickvals":[2,3,4,5],"ticks":"outside","tickcolor":"rgb(0,0,0)","ticklen":3.98505603985056,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgb(0,0,0)","family":"","size":12.7521793275218},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0,1],"gridcolor":"rgb(229,229,229)","gridwidth":0.265670402656704,"zeroline":false,"anchor":"y","hoverformat":".2f"},"annotations":[{"text":"Weight (1000lbs)","x":0.5,"y":-0.10008763433421,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgb(0,0,0)","family":"","size":15.9402241594022},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"center","yanchor":"middle"},{"text":"Miles per Gallon","x":-0.10008763433421,"y":0.5,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgb(0,0,0)","family":"","size":15.9402241594022},"xref":"paper","yref":"paper","textangle":-90,"xanchor":"center","yanchor":"middle"}],"yaxis":{"type":"linear","autorange":false,"tickmode":"array","range":[4.12984123504415,35.317626607855],"ticktext":["10","20","30"],"tickvals":[10,20,30],"ticks":"outside","tickcolor":"rgb(0,0,0)","ticklen":3.98505603985056,"tickwidth":0.66417600664176,"showticklabels":true,"tickfont":{"color":"rgb(0,0,0)","family":"","size":12.7521793275218},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"domain":[0,1],"gridcolor":"rgb(229,229,229)","gridwidth":0.265670402656704,"zeroline":false,"anchor":"x","hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":"transparent","line":{"color":"rgb(127,127,127)","width":0.66417600664176,"linetype":"solid"},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":"rgb(255,255,255)","bordercolor":"transparent","borderwidth":1.88976377952756,"font":{"color":"rgb(0,0,0)","family":"","size":12.7521793275218}},"hovermode":"closest"},"source":"A","config":{"modeBarButtonsToRemove":["sendDataToCloud"]},"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

It appears that with a gain in every **1000** pounds that there will be a **decrease** in fuel efficiency by 5.3444716 miles per gallon

The end


***

If we use literate programming, we could also:  

1. **Tangle**:  Extract the source code out of the document.  
2. **Weave**:  Execute the code to get the compiled results.  


******************************************************************************************

## Reproducible Research  

Reproducible research is one possible product of dynamic documents, however, it is not guaranteed!  Good practices for reproducible research include:  

1. Encapsulate the full project into one directory that is supported with version control.  
2. Release your code and data.  
3. Document everything and use code as documentation!  
4. Make figures, tables, and statistics the results of scripts and `inline code`.  
5. Write code that uses relative paths.  
6. Always Set your seed.  
7. Always include session information in the code file.  For example, you can use `devtools::session_info()`.

To read more about reproducibility and data management check out Vince Buffalo's Book[@Buffalo2015].


******************************************************************************************

# Markdown

To fully understand RMarkdown, we first need to cover <a href="https://daringfireball.net/projects/markdown/">Markdown</a>, which is a system for writing simple, readable text that is easily converted to HTML.  Markdown essentially is two things:  

1. A plain text formatting syntax  
2. A software tool written in Perl.  
    - Converts the plain text formatting into HTML.  
    
>**Main goal of Markdown:**  
> Make the syntax of the raw (pre-HTML) document as readable possible. 

Would you rather read this code in HTML?  
```html
<body>
  <section>
    <h1>Rock Climbing Packing List</h1>
    <ul>
      <li>Climbing Shoes</li>
      <li>Harness</li>
      <li>Backpack</li>
      <li>Rope</li>
      <li>Belayer</li>
    </ul>
  </section>
</body>
```

Or this code in Markdown?  
```markdown
# Rock Climbing Packing List

* Climbing Shoes
* Harness
* Backpack  
* Rope
* Belayer
```
If you are human, the Markdown code is definitely easier to read!

We will talk more about the syntax of Markdown after we introduce RMarkdown but let us take a moment to soak in how much easier our lives are/will be because Markdown exists!  Thank you <a href="https://en.wikipedia.org/wiki/John_Gruber">John Gruber</a> and <a href="https://en.wikipedia.org/wiki/Aaron_Swartz">Aaron Swartz</a> (RIP) for creating Markdown in 2004! 

******************************************************************************************

# RMarkdown
<a href="http://rmarkdown.rstudio.com/">RMarkdown</a> is a variant of Markdown that makes it easy to create dynamic documents, presentations and reports within RStudio.  It has embedded R code chunks to be used with **knitr** to make it easy to create reproducible (web-based) reports in the sense that they can be automatically regnerated when the underlying code it modified.    

- RMarkdown lets you combine **Markdown** with images, links, tables, LaTeX, and actual code.
- RStudio makes creating documents from RMarkdown easy  
- RStudio (like R) is free and runs on any operating system.


**RMarkdown renders many different types of files including:**  

- <a href="http://rmarkdown.rstudio.com/html_document_format.html">HTML</a>    
- <a href="http://rmarkdown.rstudio.com/pdf_document_format.html">PDF</a>  
- Markdown  
- <a href="http://rmarkdown.rstudio.com/word_document_format.html">Microsoft Word</a>   
- Presentations:  
    - Fancy HTML5 presentations:  
        - <a href="http://rmarkdown.rstudio.com/ioslides_presentation_format.html">ioslides</a>
        - <a href="http://rmarkdown.rstudio.com/slidy_presentation_format.html">Slidy</a>  
        - <a href="http://slidify.org/index.html">Slidify</a>
    - PDF Presentations:  
        - <a href="http://rmarkdown.rstudio.com/beamer_presentation_format.html">Beamer</a>  
    - Handouts:  
        - <a href="http://rmarkdown.rstudio.com/tufte_handout_format.html">Tufte Handouts</a> 
- <a href="http://rmarkdown.rstudio.com/package_vignette_format.html">HTML R Package Vignettes</a>  
- <a href="http://rmarkdown.rstudio.com/rmarkdown_websites.html">Even Entire Websites!</a>   

![](Images/Rmd_output.png)

While there are a lot of different types of rendered documents in RMarkdown, today we will focus primarily on HTML output files, as I have found these files to be the most useful and flexible for my research.

## Why R Markdown?
A convenient tool for reproducible and dynamic reports!       

- While it was created for R, it now accepts many programming languages.  For simplicity, we will only work with R today.  
- Execute code in a few ways:  
    1. **Inline Code:**  Brief code that takes place during the written part of the document. 
    2. **Code Chunks:**  Parts of the document that includes several lines of program or analysis code.  It may render a plot or table, calculate summary statistics, load packages, etc.  
- It is easy to:  
    - Embed images.  
    - Learn Markdown syntax.  
    - Include LaTeX equations.  
    - Include interactive tables.
    - Use version control with **Git**.  
        - Even easier to share and collaborate on analyses, projects and publications!
    - Add external links - Rmarkdown even understands some html code!  
    - Make beautifully formatted documents.
- Do not need to worry about page breaks or figure placement.  
- Consolidate your code and write up into a single file:  
    + Slideshows, pdfs, html documents, word files  

## Simple Workflow  

Briefly, to make a report:  

1. **Open** a `.Rmd` file.  
    - Create a YAML header (more on this in a minute!)
2. **Write** the content with RMarkdown syntax.  
3. **Embed** the R code in code chunks or inline code.  
4. **Render** the document output.  

![Workflow for creating a report](Images/rmd_workflow_cheatsheet.png)



**Overview of the steps RMarkdown takes to get to the rendered document:**  

1. Create `.Rmd` report that includes R code chunks and and markdown narratives (as indicated in steps above.).  
2. Give the `.Rmd` file to `knitr` to execute the R code chunks and create a new `.md` file.  
    - <a href="http://yihui.name/knitr/" target="_blank">Knitr</a> is a package within R that allows the integration of R code into rendered RMarkdown documents such as HTML, latex, pdf, word, among other document types.  
3. Give the `.md` file to **pandoc**, which will create the final rendered document (e.g. html, microsoft word, pdf, etc.).  
    - <a href="http://pandoc.org/" target="_blank">Pandoc</a> is a universal document converter and enables the conversion of one document type (in this case: `.Rmd`) to another (in this case: HTML)

![How an Rmd document is rendered](Images/Rmd_workflow.png)

While this may seem complicated, we can hit the "Knit" button at the top of the page like this:  
![](Images/knit_button.png)

or we can run the following code:  

```r
rmarkdown::render("RMarkdown_Lesson.Rmd", "html_document")
```

## Creating a `.Rmd` File  

It's go time!  Let's start working with RMarkdown!

1.  In the menu bar, click **File -> New File -> RMarkdown**  
    - Or simply click on the green plus sign in the top left corner of RStudio. 
    
![](Images/create_rmd.png)

2. The window below will pop up.  
- Inside of this window, choose the type of output by selecting the radio buttons.  **Note:** this output can be easily changed later!  

![](Images/new_rmd_yaml.png)

3. Click **OK**  
  


## YAML Headers

YAML stands for "YAML Ain't Markup Language" and is basically a nested list structure that includes the metadata of the document.  It is enclosed between two lines of three dashes `---` and as we saw above is automatically written by RStudio.  A simple example:  

```
---
title:  "Analysis Report"  
Author:  "Marian L. Schmidt"  
date: "May 11th, 2016"  
output:  html_document
---
```

The above example will create an HTML document.  However, the following options are also available.  

- `html_document`  
- `pdf_document`  
- `word_document`  
- `beamer_presentation` (pdf slideshow)  
- `ioslides_presentation` (HTML slideshow)  
- and more...  

Today, we will be focused on HTML files.  However, please be welcome to play around with creating word and pdf documents.  Presentation slides take on a slightly different syntax (e.g. to specify when one slide ends and the next one starts) and so there is a bit of markdown syntax specific to presentations that are beyond the focus of todays workshop.  

We will build on the details of YAML headers throughout the workshop.  


## Markdown Basics  

Check out the <a href="http://www.rstudio.com/wp-content/uploads/2015/03/rmarkdown-reference.pdf">RMarkdown Reference Guide</a>

To list a few from the <a href="http://www.rstudio.com/wp-content/uploads/2016/03/rmarkdown-cheatsheet-2.0.pdf">RMarkdown Cheatsheet</a>:  

![Markdown Basics from RStudio's RMarkdown Cheatsheet](Images/markdown_basics.png)


**Helpful Hints:**  

- End a line with two spaces to start a new paragraph.  
- Words formatted like code should be surrounded by back ticks on both sides: `  
- To make something superscript surround it with `^` on each side.  Super^script^ was created by typing `Super^script^`.  
- Equations can be inline code using `$` and centered as a blocked equation within the document with `$$`.  For example $E = mc^2$ is inline while the following is a blocked equation is: $$E = mc^2$$  
    - **Note:** To make it superscript with `$` and `$$` a `^` is needed before each alphanumeric that is superscript.  
    - Other fun math stuff:  
        - Square root:  `$\sqrt{b}$` will create $\sqrt{b}$  
        - Fractions:  `$\frac{1}{2}$` = $\frac{1}{2}$  
            - - Fractional Equations:  `$f(x)=\frac{P(x)}{Q(x)}$` = $f(x)=\frac{P(x)}{Q(x)}$  
        - Binomial Coefficients:  `$\binom{k}{n}$` = $\binom{k}{n}$  
        - Integrals:  `$$\int_{a}^{b} x^2 dx$$` = $$\int_{a}^{b} x^2 dx$$
    - <a href="https://www.sharelatex.com/learn/Main_Page">ShareLaTeX</a> is an awesome source for LaTeX code.

**Some more mathy stuff:**
		
| Description |         Code        |      Examples          |
|------------:|---------------------|:----------------------:|
|   Greek letters  |  `$\alpha$` `$\beta$` `$\gamma$` `$\rho$` `$\sigma$` `$\delta$` `$\epsilon$` `$mu$`  |    $\alpha$ $\beta$ $\gamma$ $\rho$ $\sigma$ $\delta$ $\epsilon$ $\mu$   |
|  Binary operators  |  `$\times$` `$\otimes$` `$\oplus$` `$\cup$` `$\cap$`|   $\times$ $\otimes$ $\oplus$ $\cup$ $\cap$	$\times$|
|    Relation operators  |    `$< >$` `$\subset$` `$\supset$` `$\subseteq$` `$\supseteq$` |     $< >$ $\subset$ $\supset$ $\subseteq$ $\supseteq$ |
|    Others  |    `$\int$` `$\oint$` `$\sum$` `$\prod$` |     $\int$ $\oint$ $\sum$ $\prod$ |


******************************************************************************************


> **Challenge:** 
> Try to mimic the output of the following:  

1.  Today, I am **bold** and am learning *RMarkdown*.

2.  *honey* is **very** _sweet_.  

3. YAS!!!^!!!^  

4. R^2^ values are **informative**!  

5. $R^{2}$ describe the *variance* explained in the model.  

6.  ~~I do not know RMarkdown~~  `Today I learned RMarkdown`   

7.  [RStudio link](www.rstudio.com)  

8.  Output from the following:  
```
# RMarkdown   
## R   
### Knitr   
#### Pandoc  
##### HTML  
```

9.  $\sqrt{b^2 - 4ac}$

10. $$\sqrt{b^2 - 4ac}$$  

11.  $X_{i,j}$  

> 12.  Today is the day I will make a dynamic document!  

13.  The following list:  

**Chocolate Chip cookie Recipe** 

1. butter
2. sugar  
    - A mix of brown & white sugar makes it more delicious
        - mix with butter before you add the *eggs*
3. eggs 
4. vanilla  
5. Mix dry ingredients: 
    - flour, salt, baking soda  
6. chocolate chips  


******************************************************************************************


**Fun Fact!**  The table of contents of this website was created from headers with 1-3 pound symbols! (More on this later)



## Embed Code

There are 2 ways to embed code within an RMarkdown document.  


1. **Inline Code:**  Brief code that takes place during the written part of the document.  

2. **Code Chunks:**  Parts of the document that includes several lines of program or analysis code.  It may render a plot or table, calculate summary statistics, load packages, etc.  
    
    
### Inline R Code  

Inline code is created by using a back tick (`) and the letter r followed by another back tick.  

- For example:  2^11^ is 2048.  

Imagine that you're reporting a p-value and you do not want to go back and add it every time the statistical test is re-run.  Rather, the p-value is `0.0045`.  

This is really helpful when writing up the results section of a paper.  For example, you may have ran a bunch of statistics for your scientific questions and this would be a way to have **R** save that value in a variable name.  

For example:  Is the gas mileage of automatic versus manual transmissions significantly different within the `mtcars` dataset?  


```r
mpg_auto <- mtcars[mtcars$am == 0,]$mpg # automatic transmission mileage
mpg_manual <- mtcars[mtcars$am == 1,]$mpg # manual transmission mileage
transmission_ttest <- t.test(mpg_auto, mpg_manual)
```

To extract the p-value we can type `transmission_ttest$p.value` within inline code. 

The p-value is 0.0013736.


### R Code Chunks  

R code chunks can be used to render R output into documents or to display code for illustration.  

**The Anatomy of a code chunk:**  

To insert an R code chunk, you can type it manually by typing ```` ```{r} ```` followed by  ```` ``` ```` on the next line.  You can also press the `Insert a new code chunk` button or use the shortcut key. This will produce the following code chunk:

![Inserting a code chunk](Images/insert_code_chunk.png)  

    ```{r}
    n <- 10
    seq(n)
    ```


Name the code chunk something meaningful as to what it is doing.  Below I have named the code chunk `10-random-numbers`:

    ```{r 10-random-numbers}
    n <- 10
    seq(n)
    ```
    
    
The code chunk input and output is then displayed as follows:


```r
n = 10
seq(n)
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```


**************************************************************************************************


# Knitr  

**Knitr** is an R-Package that works with  

1. **Identifies code** including chunks and inline
2. **Evaluates** all the code and returns the results 
3. **Renders** a formatted results and combines with original file.   

**Knitr** runs code as if it were being run in the R console.  



Mainly **Knitr** works with code chunks.

A code chunk looks like:

    
    ```r
    x <- rnorm(100)  
    y <- 2*x + rnorm(100)
    ```
  


**Best practices regarding code chunks:**  

1. Always name/label your code chunks!  
2. Instead of specifying the chunk options in every chunk, set the global chunk options at the beginning of the document.  More on this in a minute!  


## Chunk Labels 

Chunk labels must be **unique IDs** in a document and are good for:  

- Generating external files such as images and cached documents.  
- Chunk labels often are output when errors arise (more often for line of code).     
- **Navigating throughout long `.Rmd` documents.**  

![A method of navigating through `.Rmd` files](Images/label_navigation.png)



When naming the code chunk:  Use `-` or `_` in between words for code chunks labels instead of spaces.  This will help you and other users of your document to navigate through.  

Chunk labels must be unique throughout the document - otherwise there will be an error!    
  
  

## Chunk Options  

Pressing tab when inside the braces will bring up code chunk options.


![Some Knitr Chunk Options](Images/chunk_options.png)


- `results = "asis"` stands for "as is" and will output a non-formatted version. 
- `collapse` is another chunk option which can be helpful.  If a code chunk has many short R expressions with some output, you can collapse the output into a chunk.     

There are too many chunk options to cover here.  After the workshop take a look around at the options.

Great website for exploring <a href="http://yihui.name/knitr/options/#chunk_options">Knitr Chunk Options</a>.  



> **Challenge**  
> Run the code chunk below and play with the following knitr code chunk options:  


>- `eval = TRUE/FALSE`  
>- `echo = TRUE/FALSE`  
>- `collapse = TRUE/FALSE`  
>- `results = "asis","markup`, and `"hide` 

> In markdown, record your results.  
> **Note:**  Be sure to name your chunks!


```r
1+1
2*5
seq(1, 21, by = 3)
head(mtcars)
```

**Some examples from the chunk above**

Results from `results="markup", collapse = TRUE}`:


```r
1+1
## [1] 2
2*5
## [1] 10
seq(1, 21, by = 3)
## [1]  1  4  7 10 13 16 19
head(mtcars)
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```


Results from `results="asis", collapse = TRUE}`:

```r
1+1
```

[1] 2

```r
2*5
```

[1] 10

```r
seq(1, 21, by = 3)
```

[1]  1  4  7 10 13 16 19

```r
head(mtcars)
```

                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

## Global Options 

You may wish to have the same chunk settings throughout your document and so it might be nice to type options once instead of always re-typing it for each chunk.  To do so, you can set global chunk options at the top of the document.  

```
knitr::opts_chunk$set(echo = FALSE, 
                      eval = TRUE, 
                      message = FALSE,
                      warning = FALSE, 
                      fig.path = "Figures/",
                      fig.width = 12, 
                      fig.height = 8)
```

For example, if you're working with a collaborator who does not want to see the code - you could set `eval = TRUE` and `echo = FALSE` so the code is evaluated but not shown.  In addition, you may want to use `message = FALSE` and `warning = FALSE` so your collaborator does not see any messages or warnings from R.  

If you would like to save and store figures within a subdirectory within the project, `fig.path = "Figures/"`.  Here, the `"Figures/"` denotes a folder named *Figures* within the current directory where the figures produced within the document will be stored.  **Note:** by default figures are not saved.  

Global chunk options will be set for the rest of the document.  If you would like to have a particular chunk be different from the global options, specify at the beginning of that particular chunk.



## Figures  

**Knitr** makes producing figures really easy.  If analysis code within a chunk is supposed to produce a figure, it will just print out into the document.  

Some knitr chunk options that relate to figures:  

- `fig.width` and `fig.height`  
    - *Default:* `fig.width = 7`, `fig.height = 7`  
- `fig.align`:  How to align the figure  
    - *Options include:* `"left"`, `"right"`, and `"center"`  
- `fig.path`: A file path to the directory to where knitr should store the graphic output created by the chunk.  
    - *Default:* `'figure/'`  
- There's even a `fig.retina`(only for HTML output)




Making a single figure:  

With `fig.align = "center"`


```r
ggplot(mtcars, aes(x = mpg)) + xlab("Miles per Gallon") +
    geom_histogram(bins = 30, fill = "cornflowerblue", color = "black") +
    geom_vline(xintercept=mean(mtcars$mpg), col="red") 
```

<img src="Figures/single-fig-center-1.png" style="display: block; margin: auto;" />

With `fig.align = "right"`


```r
ggplot(mtcars, aes(x = mpg)) + xlab("Miles per Gallon") +
    geom_histogram(bins = 30, fill = "cornflowerblue", color = "black") +
    geom_vline(xintercept=mean(mtcars$mpg), col="red") 
```

<img src="Figures/single-fig-right-1.png" style="display: block; margin: auto 0 auto auto;" />


With `fig.align = "left"`


```r
ggplot(mtcars, aes(x = mpg)) + xlab("Miles per Gallon") +
    geom_histogram(bins = 30, fill = "cornflowerblue", color = "black") +
    geom_vline(xintercept=mean(mtcars$mpg), col="red") 
```

<img src="Figures/single-fig-left-1.png" style="display: block; margin: auto auto auto 0;" />

With `fig.width = 2, fig.height = 2`


```r
ggplot(mtcars, aes(x = mpg)) + xlab("Miles per Gallon") +
    geom_histogram(bins = 30, fill = "cornflowerblue", color = "black") +
    geom_vline(xintercept=mean(mtcars$mpg), col="red") 
```

<img src="Figures/single-fig-size2-1.png" style="display: block; margin: auto;" />

With `fig.width = 10, fig.height = 10`


```r
ggplot(mtcars, aes(x = mpg)) + xlab("Miles per Gallon") +
    geom_histogram(bins = 30, fill = "cornflowerblue", color = "black") +
    geom_vline(xintercept=mean(mtcars$mpg), col="red") 
```

<img src="Figures/single-fig-size10-1.png" style="display: block; margin: auto;" />



```r
myplots <- list()  # new empty list
for(i in 1:ncol(mtcars)){
  col <- names(mtcars)[i]
  ggp <- ggplot(mtcars, aes_string(x = col)) +
    geom_histogram(bins = 30, fill = "cornflowerblue", color = "black") +
    geom_vline(xintercept = mean(mtcars[[col]]), col = "red") 
  myplots[[i]] <- ggp  # add each plot into plot list
}
multiplot(plotlist = myplots, cols = 4) # must load in multiplot function from the Rcookbook see http://www.cookbook-r.com/Graphs/Multiple_graphs_on_one_page_(ggplot2)/
```

<img src="Figures/many-figs-1.png" style="display: block; margin: auto;" />


## Tables

Hand writing tables in Markdown can get tedious.  There's a few types of tables that are useful.  Here, we will use our previous example of a linear model on car weight and miles per gallon within the `mtcars` data.  

In his <a href="http://kbroman.org/knitr_knutshell/pages/figs_tables.html">Knitr in a Knutshell</a>, Dr. Karl Broman introduces:  `kable`, `pander`, and `xtable` and I have enjoyed using the first two:  

- `kable`: Within the **knitr** package - not many options but looks nice with ease. 
- `pander`: Within the **pander** package - has many more options and customization.  Useful for bolding certain values (e.g. values below a threshold).  

`kable` and `pander` tables are nice as they are useful for making non-interactive tables:



```r
# Is there a relationship between the weight of a car and the miles per gallon?
lm_mpg <- lm(mpg ~ wt, data = mtcars) # Run linear model predicting mpg based on wt
coef_lm_mpg <- coef(summary(lm_mpg)) # Extract coefficients to table 
kable(coef_lm_mpg) # kable table
```

                Estimate   Std. Error     t value   Pr(>|t|)
------------  ----------  -----------  ----------  ---------
(Intercept)    37.285126     1.877627   19.857575          0
wt             -5.344472     0.559101   -9.559044          0

```r
# Pander table
# install.packages("pander") # install pander first
library(pander)
pander(coef_lm_mpg)
```


--------------------------------------------------------------
     &nbsp;        Estimate   Std. Error   t value   Pr(>|t|) 
----------------- ---------- ------------ --------- ----------
 **(Intercept)**    37.29       1.878       19.86   8.242e-19 

     **wt**         -5.344      0.5591     -9.559   1.294e-10 
--------------------------------------------------------------


Recently, I discovered the **DT** package which makes the tables interactive in the HTML output.  Cool, huh? 


```r
# DT table = interactive
# install.packages("DT") # install DT first
library(DT)
datatable(coef_lm_mpg)
```

<!--html_preserve--><div id="htmlwidget-2673" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-2673">{"x":{"data":[["(Intercept)","wt"],[37.285126167342,-5.34447157272267],[1.87762733725589,0.559101045099323],[19.8575752640209,-9.55904414697211],[8.24179884532659e-19,1.29395870135053e-10]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>Estimate\u003c/th>\n      <th>Std. Error\u003c/th>\n      <th>t value\u003c/th>\n      <th>Pr(&gt;|t|)\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false},"callback":null,"filter":"none"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

```r
multiple_regression <- coef(summary(lm(mpg ~ ., data = mtcars))) 
datatable(multiple_regression) 
```

<!--html_preserve--><div id="htmlwidget-9340" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-9340">{"x":{"data":[["(Intercept)","cyl","disp","hp","drat","wt","qsec","vs","am","gear","carb"],[12.3033741559962,-0.111440477886862,0.0133352399133411,-0.0214821189891363,0.787110972236117,-3.71530392832747,0.821040749674628,0.317762814185421,2.52022688720842,0.655413017081793,-0.199419254856269],[18.7178844287215,1.04502336251002,0.0178575003141284,0.0217685792527032,1.63537306860509,1.89441429953271,0.730844796023449,2.10450860570774,2.05665055348227,1.4932599610728,0.828752497681768],[0.657305808402012,-0.106639221556918,0.746758486840994,-0.986840654126228,0.481303616493754,-1.96118870578834,1.12341328027775,0.150991453930671,1.22540354896034,0.438914210631434,-0.240625826666098],[0.518124396898475,0.916087375515962,0.463488650353868,0.334955314116978,0.6352778979695,0.0632521511445564,0.273941269972363,0.881423471976984,0.233989710706796,0.665206434293021,0.812178712952693]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>Estimate\u003c/th>\n      <th>Std. Error\u003c/th>\n      <th>t value\u003c/th>\n      <th>Pr(&gt;|t|)\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false},"callback":null,"filter":"none"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->



## Spell Check  

Spelling may not be our strong suit and as a result, we may need to check the spelling in our document.  There are two ways to check the spelling:  

1. Click on the "ABC checkmark" to the left of the magnifying glass button in RStudio.  ![](Images/spell_check.png)  
2. Use the `aspell()` function from the **utils** package.  However, it would be desirable to skip code chunks.  The `aspell()` function can take a filter function to skip certain lines in the files and can be used with the function `knit_filter()` which was designed to skip over code chunks in a file.  


## Knitr Themes
The knitr syntax theme can be adjusted or completely customized.  If you do not prefer the default themes, use the object `knit_theme` to change it.  There are **80 themes** contained within `knitr` and we can view the names of them by `knit_theme$get()`.

What are the first 30 `knitr` themes?


```r
head(knit_theme$get(), 30)
```

```
##  [1] "acid"          "aiseered"      "andes"         "anotherdark"  
##  [5] "autumn"        "baycomb"       "bclear"        "biogoo"       
##  [9] "bipolar"       "blacknblue"    "bluegreen"     "breeze"       
## [13] "bright"        "camo"          "candy"         "clarity"      
## [17] "dante"         "darkblue"      "darkbone"      "darkness"     
## [21] "darkslategray" "darkspectrum"  "default"       "denim"        
## [25] "dusk"          "earendel"      "easter"        "edit-anjuta"  
## [29] "edit-eclipse"  "edit-emacs"
```

We can use `knit_theme$set()` to set the theme.  For example, to set the theme to *fruit* we could run the following code:


```r
knit_theme$set("fruit")
```

Here's the link to find your favorite theme of all the <a href="http://animation.r-forge.r-project.org/knitr/">80 knitr highlight themes</a>.


## Other Programming Languages  

While knitr must be run within the R environment, it also supports many other programming languages including:  

- Python  
- Ruby  
- Haskell  
- awk/gawk  
- sed  
- shell scripts  
- Perl  
- SAS  
- TikZ  
- Graphviz  
- C++  
- And others...  

However, we have to install the corresponding software package in advance to use an engine.    

Enter the  `engine` function in Knitr.  This function allows the user to specify the language for a chunk.  

- `engine = "bash"` will load in bash and allow the user to write shell scripts within that code chunk.  


**************************************************************************************************



# Appearance  

There are many options that we can control within our `.Rmd` document.  This section helps to introduce and explore some of these customization options for HTML documents.  

## Code Folding  

As you may have noticed, each one of the code chunks in this document has an interactive ![](Images/hide.png) button.  This is controlled in the YAML header and is new as of RMarkdown v2.


When the knitr code chunk option `echo = TRUE` is specified (default = TRUE) the R code within the chunk will be included in the output document.  However, there may be times in which a user would like to exclude code entirely (`echo = FALSE`)

-`code_folding:`  
    - `code_folding: hide`:  Can inlucde R code but have it hidden by default.  
    - `code_folding: show`:  Shows R code and then the reader can click the ![](Images/hide.png) button to hide that chunk if they wish.  
    

```
output: html_document
    code_folding: show
```

## Table of Contents  

A table of contents can be added to the rendered document by using the `toc` option in the YAML header.

Options Include:  

- `toc`:  Whether to include to the table of contents:  
    - `toc: true`: Includes a table of contents  
    - **Default:**`toc: false`:  Does not include a table of contents  
- `toc_depth:`:  How many levels within the table of contents should be included in the table of contents?  
    - **Default:** `doc_depth: 3` will include headers with `###`.  
- `number_sections:` Add section numbers to the headers.  For example, this document has `number_sections: true`  
    - **Default:** `number_sections: false`  
    - Note: With each `#` there will be a decimal point added to all headers.  
- `toc_float:`  
    - 2 other optional parameters within `toc_float:`  
        - `collapsed:` Controls if the table of contents appears with only the top level.  It will expand with the cursor over it.  
            - **Default:** `collapsed: TRUE`
        - `smooth_scroll:` Controls whether the page scrolls are animated when the items within the table of contents are navigated to via clicking on them.  
            - **Default:** `smooth_scroll: true`      


For example:  

```
output:
  html_document:
    toc: true
    toc_depth: 2
---
```

> **Challenge:**
> Make the YAML header for an HTML document that includes:  

> - Table of contents
> - Float the table of contents  
> - Section headers with two hashtags (`##`)  
> - Numbered sections  
> - No smooth scrolling

## Themes

RMarkdown has several options that control the appearance of HTML documents.  Some arguments to choose from are:  

- **theme**  
- **highlight**  
- **smart**  


The HTML output themes are drawn from the <a href="http://bootswatch.com/">Bootswatch</a> library.  Valid **HTML themes** include the following:    

- `cerulean`, `cosmo`,`flatly`, `journal`, `readable`,`spacelab`, and `united`.  
    - For example, the theme of this page is `readable`.
- Pass null for no theme (in this case you can use the css parameter to add your own styles).

**Highlight** specifies the syntax highlighting style. Supported styles include the following:  

- `default`, `espresso`, `haddock`, `kate`, `monochrome`, `pygments`, `tango`, `textmate`, and `zenburn`.   
- Pass null to prevent syntax highlighting.

**Smart** indicates whether to produce typographically correct output, converting straight quotes to curly quotes, --- to em-dashes, -- to en-dashes, and ... to ellipses. **Smart** is enabled by default.

For example:

```
---
output:
  html_document:
    theme: slate
    highlight: tango
---
```

If you felt inclined, you could also produce and use your own theme.  If you did so, the output section of your YAML header would like like:  
```
output:
  html_document:
    css: styles.css
```

If you wanted to go the extra mile and write your own theme in addition to highlight, the YAML header would look like: 
```
---
output:
  html_document:
    theme: null
    highlight: null
    css: styles.css
---
```



Here's a link to learn more about the <a href="http://rmarkdown.rstudio.com/html_document_format.html#appearance_and_style">Appearance and Style</a> in HTML output.




**************************************************************************************************


# Caching  

**Problem:**  Some code chunks take a *long* time to run and may not be updated very often.  

**Solution:**  Caching!  If a code chunk has not been modified since the last rendering of the document, the old results will be directly used instead of re-running the chunk.


**Very simple solution:**  Exit knitting early -> if the rest of the document does not need to be rendered, simply put the `knit_exit()` and the rest of the document will be ignored.  Therefore, results from all previous text and code chunks will be returned.


**lazy-loading** is when an aobject will not be loaded into memory until it is used.  A "promise" is instead created, which is computationally easy (to learn more run`?promise`).


Therefore, when rendering the document, cached chunks are skipped and the output created previously from these chunks are will be (lazy-) loaded from the `cache` folder.

However, if a single change to the chunk occurs (even a whitespace counts!) then `knitr` will acknowledge this change and will actually run the chunk when the document is rendered.


`cache`=TRUE, default =FALSE

`cache.path`



Some issues with caching:  

1. R is updated every few months.  `R.version.string`  
2. External files that knitr does not track changes, may be updated and results will need to be re-run.  
3. Somtimes a cached chunk might rely on objects from other cached chunk.  This can be a serious problem!  Hence, chunk dependencies!  

`dependson` chunk option in knitr.  

## Chunk Dependencies    

### Manual   

- `dependson` specifies which other chunks the current chunk relies on by setting a vector of chunk labels.  
    - Examples with integers:  
        - `depesndson = 1`: Chunk relies on first Chunk  
        - `dependson = c(6,8)`:  Chunk relies on 6^th^ and 8^th^ chunks  
        - `dependson = -1`:  Chunk relies on previous chunk.
        - `dependson = c(-1, -2)`: Chunk relies on the two previous chunks.  
        - **Note:**  When `dependson` takes on an integer value, it cannot make a chunk depend on *later* chunks - only *previous* chunks.  Therefore, it may be useful to use the chunk names or vectors of chunk names.  
    - Examples with chunk names:  
        - `dependson = c("Chunk-1", "Chunk-2", "Chunk-3")`  
        - `dependson = c("data-generation", "data-transforamtion")`  
- As a result, each time one of the cached chunks `"Chunk-1"`, `"Chunk-2"`, and `"Chunk-3"` are re-built, the current chunk will lose its cache and also be rebuilt!  

### Automatic   

Enter:  `autodep` chunk option and function `dep_auto()`  

- `autodep` and `dep_auto()` state that if any objects in the current chunk are created in previous chunks, then the current chunk depends on a previous chunk.  

For a more conservative approach, enter `dep_prev()`.

- `dep_prev` states that a cached chunk will depend on all of its previous chunks.  Therefore, whenever previous chunks are updated, all the later chunks will also be updated.  


Knitr only tracks changes with cached chunks, **not in uncached chunks!**  Fortunately, knitr will give a warning when it sees a dependency on an uncached chunk.  

### Load Cache Manually  

Suppose you calculated a value *z* in a chunk towards the end of the document, however, you would like to use *z* in an earlier chunk.  However, this impossible because knitr compiles the document in a linear fashion and it cannot use objects created in the future.  

Enter:  `load_cache`, which takes a chunk label to find the cache database

```
load_cache(label, object, notfound = "NOT AVAILABLE", 
  path = opts_chunk$get("cache.path"), lazy = TRUE)
```

Therefore, if you use *z* in an inline R expression, it will output `NOT AVAILABLE` and since you have specified `notfound = "NOT AVAILALBE"` it will go back at the end and replace it with the value of *z*.  

So cool!


## Side Effects

A **side effect** refers to a state change that occurs outside of a function that is not the reutrned value.  

- `par()` and `options()` are side effects in the sense that they are not cached.  
- Set all the global options in the first chunk and **never** cache that chunk.  




We need to be careful with the chunk options to be sure that results from cached chunks are up-to-date.

We can also turn off lazy-loading by setting the `cache.lazy = FALSE`. 


# Bibliography  

It's also possible to inlucde a bibliography file in the YAML header.  Bibliography formats that are readable by Pandoc include the following:  

| Format | 	File extension   |
|-----+-------|
| MODS  | 	.mods |
| BibLaTeX  | .bib  |
| BibTeX  | .bibtex  |
| RIS  | .ris  |
| EndNote  | .enl  |
| EndNote XML  | .xml  |
| ISI  | .wos  |
| MEDLINE  | .medline  |
| Copac  | 	.copac  |
| JSON citeproc  | 	.json  |

To create a bibliography in RMarkdown, two files are needed:  

1. A bibliography file with the **information** about each reference.  
2. A citation style language (CSL) to describe how to **format** the reference 

An example YAML header with a bibliography and a citation style language (CSL) file:

```
output: html_document
bibliography: bibliography.bib
csl: nature.csl
```

Check out the very helpful webpage by the R Core team on <a href="http://rmarkdown.rstudio.com/authoring_bibliographies_and_citations.html" target="_blank">bibliographies and citations</a>.  

If you would like to cite R packages, **knitr** even includes a function called `write_bib()` that creates a `.bib` entries for R packages.  It will even write it to a file!  


```r
write_bib(file = "r-packages.bib") # will write all packages  
write_bib(c("knitr", "ggplot2"), file = "r-packages2.bib") # Only writes knitr and ggplot2 packages
```



## Placement 

Automatically the bibliography will be placed at the end of the document. Therefore, you should finish your `.Rmd` document with `# References` so the bibliography comes after the header for the bibliography.

```
final words...

# References
```


## Citation Styles 

**Citation Sylte Language (CSL)** is an XML-based language that identifies the format of citations and bibliographies. Reference management programs such as Zotero, Mendeley and Papers all use CSL.

Search for your favorite journal and CSL in the <a href="https://www.zotero.org/styles" target="_blank">Zotero Style Repository</a>, which currently has 8,152 CSLs.  Is there a style that you're looking for that is not there?   

```
output: html_document
bibliography: bibliography.bib
csl: nature.csl
```

In the github repo for this workshop I have included `nature.csl` and `the-isme-journal.csl` to play around with.  Otherwise, download a citation from the Zotero Style Repository!


## Citations  

Citations go inside square brackets `[ ]`and are separated by semicolons `;`. Each citation must have a key, composed of `@ + the citation identifier` from the database, and may optionally have a prefix, a locator, and a suffix.  To check what the citation key is for a reference, take a look at the `.bib` file.  Here in this file, you can also change key for each reference.  However, be careful that each ID is unique!   


Here are some examples with their code:

- Microbes contorl Earth's biogeochemical cycles  [@Falkowski2008].  
    - Code:  `Microbes contorl Earth's biogeochemical cycles  [@Falkowski2008].`  
- I love making beautiful plots with ggplot2 [@R-ggplot2]  
    - Code: `I love making beautiful plots with ggplot2 [@R-ggplot2]`  
- Dr. Yuhui Xie's book about Dynamic Documents [@Xie2015] inspired me to host this workshop.  
    - Code:  `Dr. Yuhui Xie's book about Dynamic Documents [@Xie2015] inspired me to host this workshop.`  
- A great article in *Science* regarding biogeography of microbes asks readers to imagine their Alice in Wonderland to shrink down to understand the microbial world [@Green2008].   
    - Code: `A great article in *Science* regarding biogeography of microbes asks readers to imagine their Alice in Wonderland to shrink down to understand the microbial world [@Green2008].`

The coolest thing is that it will only add the references for the articles you have cited!



# Session Info 

```r
devtools::session_info()
```

```
## Session info --------------------------------------------------------------
```

```
##  setting  value                       
##  version  R version 3.3.0 (2016-05-03)
##  system   x86_64, darwin13.4.0        
##  ui       X11                         
##  language (EN)                        
##  collate  en_US.UTF-8                 
##  tz       America/Detroit             
##  date     2016-05-11
```

```
## Packages ------------------------------------------------------------------
```

```
##  package     * version date       source        
##  assertthat    0.1     2013-12-06 CRAN (R 3.3.0)
##  base64enc     0.1-3   2015-07-28 CRAN (R 3.3.0)
##  colorspace    1.2-6   2015-03-11 CRAN (R 3.3.0)
##  DBI           0.4     2016-05-02 CRAN (R 3.3.0)
##  devtools      1.11.1  2016-04-21 CRAN (R 3.3.0)
##  digest        0.6.9   2016-01-08 CRAN (R 3.3.0)
##  dplyr       * 0.4.3   2015-09-01 CRAN (R 3.3.0)
##  DT          * 0.1     2015-06-09 CRAN (R 3.3.0)
##  evaluate      0.9     2016-04-29 CRAN (R 3.3.0)
##  formatR       1.3     2016-03-05 CRAN (R 3.3.0)
##  ggplot2     * 2.1.0   2016-03-01 CRAN (R 3.3.0)
##  gridExtra     2.2.1   2016-02-29 CRAN (R 3.3.0)
##  gtable        0.2.0   2016-02-26 CRAN (R 3.3.0)
##  highr         0.5.1   2015-09-18 CRAN (R 3.3.0)
##  htmltools     0.3.5   2016-03-21 CRAN (R 3.3.0)
##  htmlwidgets   0.6     2016-02-25 CRAN (R 3.3.0)
##  httr          1.1.0   2016-01-28 CRAN (R 3.3.0)
##  jsonlite      0.9.19  2015-11-28 CRAN (R 3.3.0)
##  knitr       * 1.13    2016-05-09 CRAN (R 3.3.0)
##  labeling      0.3     2014-08-23 CRAN (R 3.3.0)
##  magrittr      1.5     2014-11-22 CRAN (R 3.3.0)
##  memoise       1.0.0   2016-01-29 CRAN (R 3.3.0)
##  munsell       0.4.3   2016-02-13 CRAN (R 3.3.0)
##  pander      * 0.6.0   2015-11-23 CRAN (R 3.3.0)
##  plotly      * 3.4.13  2016-04-12 CRAN (R 3.3.0)
##  plyr          1.8.3   2015-06-12 CRAN (R 3.3.0)
##  R6            2.1.2   2016-01-26 CRAN (R 3.3.0)
##  Rcpp          0.12.4  2016-03-26 CRAN (R 3.3.0)
##  rmarkdown   * 0.9.6   2016-05-01 CRAN (R 3.3.0)
##  scales        0.4.0   2016-02-26 CRAN (R 3.3.0)
##  stringi       1.0-1   2015-10-22 CRAN (R 3.3.0)
##  stringr       1.0.0   2015-04-30 CRAN (R 3.3.0)
##  tidyr         0.4.1   2016-02-05 CRAN (R 3.3.0)
##  viridis       0.3.4   2016-03-12 CRAN (R 3.3.0)
##  withr         1.0.1   2016-02-04 CRAN (R 3.3.0)
##  yaml          2.1.13  2014-06-12 CRAN (R 3.3.0)
```

# References 
