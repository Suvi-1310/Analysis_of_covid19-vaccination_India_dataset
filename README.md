# covid19 and vaccination data analysis of India- Python Project

## Contents 📖
- [Introduction](#introduction)
- [Import Required Libraries](#import-required-libraries)
- [Explored the dataset](#explored-the-dataset)
- [Dataset cleaning](#dataset-cleaning)
- [Dataset analysis](#dataset-analysis)
- [Extracting Insights from the dataset through visualization](#extracting-insights-from-the-dataset-through-visualization)

## Introduction
This project provides a detailed analysis of COVID-19 trends and vaccination progress in India. Using reliable datasets and analytical tools, we explore key metrics such as case trends, vaccination rates, and their correlation with pandemic management efforts.

**The aim of this analysis** is to present actionable insights through data visualization and statistical methods, helping to better understand the impact of COVID-19 and the effectiveness of India's vaccination campaign.

🛠️ Tools Used :
- Language : Python
- Library : Pandas, Numpy, Matplotlib, Seaborn
- IDE : Google colab

🗂️ Dataset : [Dataset]()

## Import required libraries
```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import plotly.express as px
from plotly.subplots import make_subplots
from datetime import datetime
```
- **Libraries and Tools Used 📚**
  
  - ```pandas```: For data manipulation and analysis, including reading, cleaning, and transforming datasets.
  - ```numpy```: For numerical computations and handling large datasets efficiently.
  - ```seaborn```: For creating advanced statistical visualizations with ease.
  - ```matplotlib.pyplot```: For basic plotting and visualizing data trends.
  - ```plotly.express```: For interactive and dynamic visualizations, enabling detailed exploration of the data.
  - ```plotly.subplots```: To create complex multi-plot visualizations for comparative analyses.
  - ```datetime```: For handling and processing date and time data, essential for time-series analysis.

## Explored the dataset
**1. Import the dataset**
```python
from google.colab import files
uploaded = files.upload()
```
- ```files.upload()```: This method allows us to upload files (e.g., CSV or Excel datasets) from their local machine directly into the Colab notebook.
                        Once uploaded, the dataset can be accessed and processed within the Colab runtime for further analysis.

**2. Load the dataset**
```python
covid19 = pd.read_csv('covid_19_india.csv')
print(covid19)
```
- ```pd.read_csv()```: This function reads the dataset (covid_19_india.csv) into a Pandas DataFrame, which is a tabular data structure used for analysis.

**3. Explore the dataset**
```python
#Display the columns name along with datatypes
covid19.columns

#Display the first 5 rows
covid19.head()

#Display the columns name, not-null count and data types of all column
covid19.info()

#Display the statistics calculation according to our dataset like count, mean, standard devaition, min, max etc.
covid19.describe()

```

**Repeat the all above steps for vaccination dataset**

## Dataset cleaning

**1. Drop the unwanted columns**
```python
covid19.drop(["Sno", "Time", "ConfirmedIndianNational", "ConfirmedForeignNational"], inplace = True, axis = 1)
```
- ```drop()```: This function is used to remove one or more columns (or rows) from the DataFrame.
- ```inplace=True```: Modifies the original DataFrame directly, instead of returning a new DataFrame.
- ```axis=1```: Specifies that we are dropping columns (not rows).

**2. Change the date format**
```python
covid19['Date'] = pd.to_datetime(covid19['Date'], format = '%Y-%m-%d')
```
- ```pd.to_datetime()```: This function converts the 'Date' column (which may be stored as text or an object) into a proper datetime object for easier manipulation and analysis.
- ```format='%Y-%m-%d'```: Specifies the format of the date in the dataset. Here, %Y-%m-%d indicates that the date follows the format Year-Month-Day (e.g., 2020-03-25).
  
- **Purpose:** By converting the 'Date' column to a datetime object, we can efficiently perform time-based operations, such as sorting, filtering, and aggregating data by date.

## Dataset analysis
**1. Calculate active cases**
```python
covid19['Active_cases'] = covid19['Confirmed']- (covid19['Cured'] + covid19['Deaths'])
covid19
```
- ```Confirmed```: Total number of confirmed COVID-19 cases.
- ```Cured```: Total number of recovered patients.
- ```Deaths```: Total number of deceased patients.
- ```Active_cases```: The number of active cases is calculated by subtracting the sum of Cured and Deaths from Confirmed.

- **Output : This new column, 'Active_cases', represents the current number of individuals actively battling COVID-19 in the population.**

**2. Pivot Table Calculation**
```python
statewise = pd.pivot_table(covid19, values = ["Confirmed", "Deaths", "Cured"], index = "State/UnionTerritory", aggfunc = max) 
```
- ```pd.pivot_table()```: This function creates a pivot table, a summary table that aggregates data by specified indices (in this case, by state/territory).
- ```values=["Confirmed", "Deaths", "Cured"]```: Specifies the columns for which we want to compute the aggregated values (confirmed cases, deaths, and cured cases).
- ```index="State/UnionTerritory"```: Defines the rows of the pivot table by grouping the data by the "State/UnionTerritory" column.
- ```aggfunc=max```: Aggregates the data by calculating the maximum value for each state in the selected columns (e.g., the highest number of confirmed cases, deaths, and cures). This helps to get the most up-to-date or peak values.

- **Output : statewise DataFrame gives a clear, concise view of the maximum COVID-19 statistics for each state and union territory.**

```python
#calculate_recovery_rate
statewise["Recovery_rate"] = statewise["Cured"]*100/statewise["Confirmed"]

#calculate_mortality_rate
statewise["Mortality_rate"] = statewise["Deaths"]*100/statewise["Confirmed"]

#sort_the_values
statewise = statewise.sort_values(by = "Confirmed", ascending = False)

#visualize_pivot_table
statewise.style.background_gradient(cmap = "cubehelix")
```
- ```sort_values()```: This function sorts the DataFrame based on the specified column(s).
- ```by="Confirmed"```: Specifies that the sorting should be done based on the "Confirmed" column (i.e., total confirmed COVID-19 cases).
- ```ascending=False```: Orders the data in descending order (from highest to lowest). This helps to prioritize and display the states/territories with the highest number of confirmed cases at the top of the table.
- ```style.background_gradient()```: This function applies a color gradient to the background of the DataFrame based on the numerical values in each column.
- ```cmap="cubehelix"```: Specifies the colormap to use for the gradient. "cubehelix" is a perceptually uniform color map that ranges from dark to light tones, helping to highlight differences in data effectively.

## Extracting Insights from the dataset through visualization
**1. Visualize top10 active cases**
```python
Top10_Active_cases = covid19.groupby(by = 'State/UnionTerritory').max()[['Active_cases', 'Date']].sort_values(by = ['Active_cases'], ascending = False).reset_index()
fig = plt.figure(figsize=(12,9))
plt.title("Top 10 states with most active cases in India", size = 30)
ax = sns.barplot(data = Top10_Active_cases.iloc[:10], y = "Active_cases", x = "State/UnionTerritory", linewidth = 3, edgecolor = 'white')
plt.xlabel("States")
plt.ylabel("Top 10 Active cases")
plt.show()
```
- Grouping the Data:
  - ```groupby(by='State/UnionTerritory')```: Groups the data by state/territory to aggregate COVID-19 statistics by each region.
  - ```max()```: Selects the maximum value for Active_cases and Date for each state/territory, which represents the highest number of active cases recorded.

- Sorting and Selecting Top 10 States:
  - ```sort_values(by=['Active_cases'], ascending=False)```: Sorts the data by the Active_cases column in descending order to get the states with the most active cases.
  - ```reset_index()```: Resets the index of the DataFrame to make it easier to work with for plotting.

- Plotting the Data:
  - ```plt.figure(figsize=(12,9))```: Sets the size of the figure for better readability.
  - ```plt.title()```: Adds a title to the plot, specifying that this visualization shows the top 10 states with the most active cases in India.
  - ```sns.barplot()```: Creates a bar plot with State/UnionTerritory on the x-axis and Active_cases on the y-axis. The bar width and edge color are customized for better visual appeal.
  - ```plt.xlabel() and plt.ylabel()```: Label the axes for clarity.

- Displaying the Plot:
  - ```plt.show()```: Displays the bar plot.

**2. Visualize top10 states with highest death**
```python
Top_10_states_with_highest_deaths =covid19.groupby(by = 'State/UnionTerritory').max()[['Deaths', 'Date']].sort_values(by = ['Deaths'], ascending = False).reset_index()
fig = plt.figure(figsize=(16,10))
plt.title("Top 10 states with most highest deaths in India", size = 30)
ax = sns.barplot(data = Top_10_states_with_highest_deaths.iloc[:12], y = "Deaths", x = "State/UnionTerritory", linewidth = 3, edgecolor = 'white')
plt.xlabel("States")
plt.ylabel("Total death")
plt.show()
```
- Grouping the Data:
  - ```groupby(by='State/UnionTerritory')```: Groups the data by state/territory to aggregate COVID-19 statistics by each region.
  - ```max()```: Selects the maximum values for Deaths and Date for each state/territory, representing the highest number of deaths recorded for each region.

- Sorting and Selecting the Top States:
  - ```sort_values(by=['Deaths'], ascending=False)```: Sorts the data by the Deaths column in descending order, bringing the states with the most deaths to the top of the list.
  - ```reset_index()```: Resets the DataFrame index to make it easier to work with for plotting.

- Plotting the Data:
  - ```plt.figure(figsize=(16,10))```: Sets the figure size to make the chart more readable, especially for displaying longer state names.
  - ```plt.title()```: Adds a title to the plot, indicating that this chart shows the top 10 states with the highest COVID-19 deaths in India.
  - ```sns.barplot()```: Creates a bar plot with State/UnionTerritory on the x-axis and Deaths on the y-axis. The bar width and edge color are customized for better visual aesthetics.
  - ```plt.xlabel() and plt.ylabel()```: Labels the axes for clarity—indicating that the x-axis represents states and the y-axis represents the total number of deaths.

- Displaying the Plot:
  - ```plt.show()```: Displays the bar plot on the screen.
 
**3. Visulaize top5 most affexted states of India with growth line**
```python
fig = plt.figure(figsize=(16,10))
ax = sns.lineplot (data = covid19[covid19['State/UnionTerritory'].isin (['Maharashtra', 'Karnataka', 'Tamil Nadu','Kerala', 'Uttar Pradesh'])], x = 'Date', y = 'Active_cases', hue = 'State/UnionTerritory')
ax.set_title("Top 5 most affected states of India", size = 15)
```
- Selecting the Data:
  - ```covid19[covid19['State/UnionTerritory'].isin([...])]```: Filters the dataset to include only the states of Maharashtra, Karnataka, Tamil Nadu, Kerala, and Uttar Pradesh. These are the 5 states with the highest COVID-19 impact, selected based on the project focus.

- Creating the Line Plot:
  - ```sns.lineplot()```: Creates a line plot with the following parameters:
  - ```x='Date'```: The x-axis represents the Date column, showing the timeline of the COVID-19 cases.
  - ```y='Active_cases'```: The y-axis represents the number of active COVID-19 cases in each state on a given day.
  - ```hue='State/UnionTerritory'```: Different lines are created for each state, colored differently for easy differentiation.

- Setting the Plot Title:
  - ```ax.set_title("Top 5 most affected states of India", size=15)```: Adds a title to the plot, indicating that this visualization shows the top 5 states with the most active COVID-19 cases over time. The font size is set to 15 for clarity.

- Displaying the Plot:
  - The plot is displayed with ```plt.show()```

**Multiple visualization also done according to dataset for better anlaysis, Please feel free to watch in python script**

✨ **I'm open to feedback and discussions! Reach out with any questions, suggestions, or collaboration ideas.** 

👉 [LinkedIn | Suviksha Pathariya](https://www.linkedin.com/in/suviksha-pathariya/) 

⭐ **Don’t forget to star and follow this repository if you found it useful!** 

💡 **Stay tuned for more features and updates coming soon!** 


