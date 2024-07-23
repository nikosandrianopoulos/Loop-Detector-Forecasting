
# Loop Detector Forecasting using PEMS-BAY

Training a Long Short Term Memory (LSTM) model using open source data in order to predict detected speed from loop detectors on a part of the road network in Bay Area, California. 



## Resources Used
- **Language:** Python 3.9
- **Environment:** Jupyter Notebook
- **Data Manipulation:** pandas, numpy, holiday
- **Data Visualization:** matplotlib, seaborn
- **Machine Learning:** tensorflow keras 
## Data
The dataset that was used in this project is an open source dataset called PEMS-BAY (version 3.0). It is conducted by 3 files

- PEMS-BAY.csv : Contains the time-series data from 325 loop detectors in parts of Bay-Area, CA. The data is being recorded every 5 minutes for a span of 6 months (from 01-01-2017 00:00:00 to 30-06-2017 23:55:00).

- PEMS-BAY-META.csv :  Contains information about each loop detector such as coordinates (longtitude & lattitude), direction (north, south, east, west), county, regional code etc.

- adj_mx_bay.pkl : pickle file containing meta data 

Data source link: [here](https://zenodo.org/records/4264005)
## Project Structure

The project files are being stored in folders with the according naming

- plots: all plots that where created in the timeline of the project
- notebooks: all notebooks that where written in the timeline of the project with the correct naming (0-data-preprocess, 1-feature-engineering, 2-seasonal behavior, 3-data-forecast)
- data-sci.pdf : full project documentation (in Greek)
## Workflow

#### Data Preprocess
After importing the data in the notebook the first thing being checked is potential empty cells. After that check is done outliers are being detected using the Turkey's Fence method. Potential outliers are being "neutered" by replacing them with the median value of each column. After that the data is being normalized using the MinMaxScaler, bringing all record values in the range [0,1].

#### Feature Engineering
Using both holiday & pandas library features are being added to the dataframe containing the following characteristics:
- hour: hour of the recorded data (range from 0 to 23)
- dayofweek: numbered day of the week in which the data was recorded (0-Monday, 1-Tuesday,...,6-Sunday)
- week: numbered week of year in which the data was recorded. (Because the date 01-01-2017 was a Sunday it is also considered the first week of the year, meaning that the first week of data is also the first day of data)
- month: numbered month in which the data was recorded (1-January,..., 6-June)
- is_holiday: boolean value that determines if the date was considered a holiday in the US in the year 2017

#### Trend-Seasonal Behavior
Using the matplotlib & seaborn libraries for plotting as well as the pandas library for resampling the data in different timestamps different trends and behavior of the data where discovered giving insights about the traffic on several hours of the day and days of the 6 month period.

#### Forecasting Model
The final step of the project is developing a forecasting model that can provide future predictions on speed detected by all loop detectors in the next hour using 5 minute records. The model of choice is a Long Short Term Memory (LSTM). LSTM is a sequential recurrent neural network (RNN) that is often being used for time-series data. After, spliting the dataset in train, validation and test sets as seen below the LSTM model is being defined. 

- Training set: 01-01-2017 to 31-03-2017
- Validation set: 01-04-2017 to 30-04-2017
- Testing set: 01-05-2017 to 30-06-2017

The units for the Dense layer of the model are set at 325 (as many as the loop detectors), when the LSTM units are being set at 75. The number of epochs is 100 with a 64 batch size. At last, there is an early stopping method being implemented with a patience of 8, meaning if there are no reductions on loss percentage for 8 epochs the training is being stopped. After plotting the training & validation loss values in the same line plot there is no overfitting or underfitting, meaning that the model work fine. Finally the model was reviewed with the the metrics MSE, MAPE, RMSE and R^2 score. These metrics where counted every 5 minutes and the median of each metric across all loop detector data in the test set and stored in a dataframe.


## License
This project was built using an open-source dataset and can be found [here](https://zenodo.org/records/4264005).

This project was built as part of a course named "Data Science Topics" in the undergraduate study program of the Computer Science department at the University of Piraeus. The outline of the project was conducted by the Data Science Lab @ Univ. of Piraeus.
