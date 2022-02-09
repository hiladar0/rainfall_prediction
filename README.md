# rainfall_prediction

### Problem definition <a class="anchor" id="1.1"></a>

**goal**: 
Predict next-day rainfall in Australia using location specific data. 

**data**: 
10-years dailly weather obersavation from different locations. These observations have been taken from the Bureau of Meteorology's "real time" system. Most of the data are generated and handled 

<img width="500" alt="data_map" src="https://github.com/hiladar0/rainfall_prediction/blob/main/images/Screen%20Shot%202022-02-08%20at%2014.59.00.png">


<img width="1000" alt="data_map" src="https://github.com/hiladar0/rainfall_prediction/blob/main/images/Features%20over%20time%20%26%2030-days%20average.png">




**model:** 
random forest

**summary:** 

1. Econding Wind direction: I had to consider how to encode/represent angle degree values (0-360 degress) in a continuous and periodic way  (epsilon degrees should be near -epsilon degress). I chose to map the angle to the pair:
$f: f(\theta) = (\sin(\theta), \cos(\theta))$

<img width="800" alt="data_map" src="https://github.com/hiladar0/rainfall_prediction/blob/main/images/wind%20direction%20and%20speed.png">


2. Morning and afternoon: The data contains morning and afternoon features, which are inherently correlated. If $x_{AM}$ and $x_{PM}$ are the morning the afternoon fields, I chose to apply the following map:
$(x_{AM}, x_{PM}) \to (x_{PM}, x_{PM}-x_{AM})$. In that sense, I only used the uncorrealted component of the morning fields.


3. Missing Values: I used three-stage process to fill missing values. First, using the strong auto-correaltion of the fields, I completed the missing values by the n-previous-days mean (if possible). Next, using the seasonality of the fields, I filled the remaining missing values by the mean value of the field in the same month and location. Finally I dropped the remaining nan values.

4. objective function - I care more about precision (predicting Rain when it's raining), and hence optimize for f1-score (rather than accuracy).

5. Training & Selection - I trained a random forest using randomized grid search. I used oversamppling to overcome imbalance problems. I chose optimal paramers for the model using CV and chose the prediciton threshold that maximizes the f1 score.

<img width="1000" alt="data_map" src="https://github.com/hiladar0/rainfall_prediction/blob/main/images/model_performance.png">


==== Test ==== 

              precision    recall  f1-score   support

     no_rain       0.92      0.88      0.90      8857
        rain       0.63      0.72      0.67      2463

    accuracy                           0.85     11320
