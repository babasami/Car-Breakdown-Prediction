# Car-Breakdown-Prediction:
This predictive maintenance template focuses on the techniques used to predict when an in-service machine will fail, so that maintenance can be planned in advance.

The input data consists of "train", "test", and "RUL" in the original data source.
The training data ("train") consists of multiple multivariate time series with "days" as the time unit, together with 21 sensor readings for each cycle. Each time series can be assumed as being generated from a different engine of the same type. Each engine is assumed to start with different degrees of initial wear and manufacturing variation, and this information is unknown to the user. In this simulated data, the engine is assumed to be operating normally at the start of each time series. It starts to degrade at some point during the series of the operating cycles. The degradation progresses and grows in magnitude. When a predefined threshold is reached, then the engine is considered unsafe for further operation. In other words, the last cycle in each time series can be considered as the failure point of the corresponding engine. Taking the sample training data shown in the following table as an example, the engine with id=1 fails at day 192, and engine with id=2 fails at day 287.

The testing data ("test") has the same data schema as the training data. The only difference is that the data does not indicate when the failure occurs (in other words, the last time period does NOT represent the failure point). Taking the sample testing data shown in the following table as an example, the engine with id=1 runs from day 1 through day 30 It is not shown how many more days s engine can last before it fails.

The ground truth data ("RUL") provides the number of remaining working cycles for the engines in the testing data. Taking the sample ground truth data shown in the following table as an example, the engine with id=1 in the testing data can run another 112 days it fails.

#Feature engineering:
Another important task is to generate training and testing features

The features that are included or created in the training data can be grouped into two categories. More types of features can be created based on different use cases and data.

Aggregate features:
Selected raw features The raw features are those that are included in the original input data. In order to decide which raw features should be included in the training data, both the detailed data field description and domain knowledge is helpful. In this template, all the sensor measurements (s1-s21) are included in the training data. Other raw features get used are: days,ecoMode,cityMode,sportMode.

Aggregate features These features summarize the historical activity of each asset. In the template, two types of aggregate features are created for each of the 21 sensors. The description of these features are shown below.

a1-a21: the moving average of sensor values in the most w recent days
sd1-sd21: the standard deviation of sensor values in the most w recent days
The time window parameter w is set w=5 in the template, but it can be adjusted for different use cases.

These aggregate features serve as examples of a large set of potential features that could be created. Other features such as change in the sensor values within a window, change from the initial value, velocity of change, number over a defined threshold, etc. could be included as well.

Also, depending on the size of the data, it may prove most useful to aggregate all of the data itself rather than including any raw data. In other words, only include aggregated features for the time-varying features; for example, it may be best to aggergate sensor readings that occur every second - if failures occur only once every couple of months for example - to day or week values before modeling (to get a more balanced dataset between rows that represent failure and rows that represent non-failure). It is best to try different levels of aggregation and evaluate model performance in order to determine the optimal aggregation level.

Prepare the testing data:
In this section we will summarize the process of preparing the testing data and explain relevant issues that were not previously discussed. The testing data is prepared mainly from two datasets, where the data from the second Reader module ("test") is used to generate aggregate features, and the data from the third Reader module ("RUL") is used to generate labels (RUL, label1, and label2) for three learning models.

Similar to the training data, the time series data in the testing data helps to generate aggregate features during the feature engineering process. Instead of having all time series data in the testing data, we only keep the the record with the maximum cycle for each engine id. In other words, one record is retained for each engine. Eventually, the testing data contains 100 records, which matches the RUL in the ground truth data.

After feature engg. I check for few of time series model and came up with LSTM model as its showing better result compare to other model.
Reshape the data set required by LSTM(Long short term memory) then use time stamp of 30 days and generated X_train,y_train,X_test,y_test
and then applied the LSTM model with activation function as Sigmoid ,loss function as binary_crossentropy and optimizer as Adam
Applied early stoping function so that if the max. result is achieved then the model can be stop in earlier state only.



