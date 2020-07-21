Modeling and prediction for movies
Setup
Load packages
library(ggplot2)
## Warning: package 'ggplot2' was built under R version 3.3.2
library(dplyr)
## Warning: package 'dplyr' was built under R version 3.3.2
library(statsr)
Load data
Make sure your data and R Markdown files are in the same directory. When loaded your data file will be called movies. Delete this note when before you submit your work.

load("C:/Users/CHISARA1/Desktop/projectdata/movies.Rdata.rdata")
Part 1: Data
The observations in the experiment were data collected for randomly selected movies on both Rotten Tomato and IMDB.Both webstes(i.e Rotten Tomatoes and IMDB) offer searchable database of million of movies, TV and entertainmentprograms and cast and crew members.(see “www.rottentomatoes.com and www.imdb.com”). Hence, the data obtained can be used for generalization and not for causalty.

Part 2: Research question
Is there a relationshp between Critics_score and Audience_score?

It is important to know how audience views for movies or popularity of movies is affected by the opinions or ratings of professional critics. Especially, to find out whether views for a movies will increase if it gets a high critics score.

Part 3: Exploratory data analysis
ggplot(data = movies, aes(x = audience_score)) +
  geom_histogram()
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.


summary(movies$audience_score)
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   11.00   46.00   65.00   62.36   80.00   97.00
Top 25% of the movies considered got 80% and above audience score. The average audience score for the movies was 62.36.
ggplot(data = movies, aes(x = critics_score)) +
  geom_histogram()
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.


summary(movies$critics_score)
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    1.00   33.00   61.00   57.69   83.00  100.00
top 25% of the selected movies received a critics score of 83 and above, while the average critics score was 57.69.
```

Part 4: Modeling
# linear modeling
M1 <- lm(audience_score ~ critics_score, data = movies)
ggplot(data = M1, aes(x = audience_score)) +
  geom_histogram()
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.


 summary(M1)
## 
## Call:
## lm(formula = audience_score ~ critics_score, data = movies)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -37.043  -9.571   0.504  10.422  43.544 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   33.43551    1.27561   26.21   <2e-16 ***
## critics_score  0.50144    0.01984   25.27   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 14.37 on 649 degrees of freedom
## Multiple R-squared:  0.496,  Adjusted R-squared:  0.4952 
## F-statistic: 638.7 on 1 and 649 DF,  p-value: < 2.2e-16
ggplot(data = M1, aes(x = critics_score, y = audience_score)) +
  geom_jitter()+
  geom_smooth(method = "lm", se = FALSE)


```

ggplot(data = M1, aes(x = .fitted, y = .resid)) +
  geom_point() +
  geom_hline(yintercept = 0, linetype = "dashed") +
  xlab("Fitted values") +
  ylab("Residuals")


m_audience_score <- lm( audience_score ~ critics_score  + imdb_rating + audience_rating + best_pic_nom  + best_actress_win, data = movies )
summary(m_audience_score)$adj.r.squared
## [1] 0.8829205
Part 5: Prediction
Captain_America <- data.frame(critics_score = 90, imdb_rating = 8.0, audience_rating = "Upright", best_pic_nom = "no", best_actress_win = "no")
predict(m_audience_score, Captain_America, interval = "prediction", level = 0.95)
##        fit      lwr     upr
## 1 85.57703 71.95437 99.1997
The model predicts, with 95% confidence, that the movie “Captain America” with “critics_score = 90, imdb_rating = 8.0, audience_rating =”Upright“, best_pic_nom =”no“, best_actress_win =”no“” is expected to have an audience score between 71.954 to 99.199.
Part 6: Conclusion
Comparing the result of predicting the audience score for “Captain America” (85.57) to what was actually obtained on the RottenTomato website (89) one can safely conclude that the model predicts correctly audience score, which i assume is a fuction of the popularity of the movie in question considering all the variables in the model.The high correlation between audience_score and critics_score also answers the research question of possible relationship between the two, which was found to be true.
