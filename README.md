# covid19 and vaccination data analysis of India- Python Project

## Contents 📖
- [Introduction](#introduction)
- [Import Required Libraries](#import-required-libraries)
- [Explored the dataset](#explored-the-dataset)
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

