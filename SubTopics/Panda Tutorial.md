## Pandas
Pandas is arguably the most important Python package for data analysis. With over 100 million downloads per month, it is the de facto standard package for data manipulation and exploratory data analysis. Its ability to read from and write to an extensive list of formats makes it a versatile tool for data science practitioners. Its data manipulation functions make it a highly accessible and practical tool for aggregating, analyzing, and cleaning data. 

In our blog post on how to learn pandas, we discussed the learning path you may take to master this package. This beginner-friendly tutorial will cover all the basic concepts and illustrate pandas' different functions. You can also check out our course on pandas Foundations for further details. 

This article is aimed at beginners with basic knowledge of Python and no prior experience with pandas to help you get started.

### What is pandas?
pandas is a data manipulation package in Python for tabular data. That is, data in the form of rows and columns, also known as DataFrames. Intuitively, you can think of a DataFrame as an Excel sheet. 

pandas’ functionality includes data transformations, like sorting rows and taking subsets, to calculating summary statistics such as the mean, reshaping DataFrames, and joining DataFrames together. pandas works well with other popular Python data science packages, often called the PyData ecosystem, including

### 1. NumPy for numerical computing
### 2. Matplotlib, Seaborn, Plotly, and other data visualization packages
### 3. scikit-learn for machine learning

## What is pandas used for?
pandas is used throughout the data analysis workflow. With pandas, you can:

Import datasets from databases, spreadsheets, comma-separated values (CSV) files, and more.
Clean datasets, for example, by dealing with missing values.
Tidy datasets by reshaping their structure into a suitable format for analysis.
Aggregate data by calculating summary statistics such as the mean of columns, correlation between them, and more.
Visualize datasets and uncover insights.
pandas also contains functionality for time series analysis and analyzing text data.

### Key benefits of the pandas package
Undoubtedly, pandas is a powerful data manipulation tool packaged with several benefits, including:

Made for Python: Python is the world's most popular language for machine learning and data science.
Less verbose per unit operations: Code written in pandas is less verbose, requiring fewer lines of code to get the desired output. 
Intuitive view of data: pandas offers exceptionally intuitive data representation that facilitates easier data understanding and analysis.
Extensive feature set: It supports an extensive set of operations from exploratory data analysis, dealing with missing values, calculating statistics, visualizing univariate and bivariate data, and much more.
Works with large data: pandas handles large data sets with ease. It offers speed and efficiency while working with datasets of the order of millions of records and hundreds of columns, depending on the machine.

## Install pandas
Installing pandas is straightforward; just use the pip install command in your terminal. 
```python
pip install pandas
```

## Importing data in pandas
To begin working with pandas, import the pandas Python package as shown below. When importing pandas, the most common alias for pandas is pd.

```python
import pandas as pd
```
Use read_csv() with the path to the CSV file to read a comma-separated values file

```python
df = pd.read_csv("diabetes.csv")
```

This read operation loads the CSV file diabetes.csv to generate a pandas Dataframe object df.

### Importing text files

Reading text files is similar to CSV files. The only nuance is that you need to specify a separator with the sep argument, as shown below. The separator argument refers to the symbol used to separate rows in a DataFrame. Comma (sep = ","), whitespace(sep = "\s"), tab (sep = "\t"), and colon(sep = ":") are the commonly used separators. Here \s represents a single white space character.


```python
df = pd.read_csv("diabetes.txt", sep="\s")
```
### Importing Excel files (single sheet)
Reading excel files (both XLS and XLSX) is as easy as the read_excel() function, using the file path as an input.
```python
df = pd.read_excel('diabetes.xlsx')
```
You can also specify other arguments, such as header for to specify which row becomes the DataFrame's header. It has a default value of 0, which denotes the first row as headers or column names. You can also specify column names as a list in the names argument. The index_col (default is None) argument can be used if the file contains a row index.

### Importing Excel files (multiple sheets)
Reading Excel files with multiple sheets is not that different. You just need to specify one additional argument, sheet_name, where you can either pass a string for the sheet name or an integer for the sheet position (note that Python uses 0-indexing, where the first sheet can be accessed with sheet_name = 0)

```python
# Extracting the second sheet since Python uses 0-indexing
df = pd.read_excel('diabetes_multi.xlsx', sheet_name=1)
```

### Importing JSON file
Similar to the read_csv() function, you can use read_json() for JSON file types with the JSON file name as the argument. The below code reads a JSON file from disk and creates a DataFrame object df.

```python
df = pd.read_json("diabetes.json")
```
## Outputting data in pandas
Just as pandas can import data from various file types, it also allows you to export data into various formats. This happens especially when data is transformed using pandas and needs to be saved locally on your machine. Below is how to output pandas DataFrames into various formats.

### Outputting a DataFrame into a CSV file
A pandas DataFrame (here we are using df) is saved as a CSV file using the .to_csv() method. The arguments include the filename with path and index – where index = True implies writing the DataFrame’s index.

```python
df.to_csv("diabetes_out.csv", index=False)
```
**Code Explanation**

. This code saves a pandas DataFrame df to a CSV file named "diabetes_out.csv" in the current working directory.
• The to_csv() method is used to write the DataFrame to a CSV file.
• The index=False argument specifies that the index column should not be included in the output file.
• This is useful when the index is not meaningful or when it is already included as a separate column in the DataFrame.

## Outputting a DataFrame into a JSON file
Export DataFrame object into a JSON file by calling the .to_json() method.
```python
df.to_json("diabetes_out.json")
```

**Note:** A JSON file stores a tabular object like a DataFrame as a key-value pair. Thus you would observe repeating column headers in a JSON file.

## Outputting a DataFrame into a text file
As with writing DataFrames to CSV files, you can call .to_csv(). The only differences are that the output file format is in .txt, and you need to specify a separator using the sep argument.

```python
df.to_csv('diabetes_out.txt', header=df.columns, index=None, sep=' ')
```
## Outputting a DataFrame into an Excel file
Call .to_excel() from the DataFrame object to save it as a “.xls” or “.xlsx” file.
```python
df.to_excel("diabetes_out.xlsx", index=False)
```

## Viewing and understanding DataFrames using pandas 
After reading tabular data as a DataFrame, you would need to have a glimpse of the data. You can either view a small sample of the dataset or a summary of the data in the form of summary statistics.

## How to view data using .head() and .tail()

You can view the first few or last few rows of a DataFrame using the .head() or .tail() methods, respectively. You can specify the number of rows through the n argument (the default value is 5).

```python
df.head()
```
**Code Explanation**
This code is written in Python and it calls the head() method on a Pandas DataFrame object named df.
• The head() method is used to display the first few rows of the DataFrame.
• By default, it displays the first 5 rows, but you can pass an integer argument to display a different number of rows.
• This code is useful for quickly inspecting the contents of a DataFrame and getting a sense of what kind of data it contains.

![image](https://github.com/naveen9596/Datascience-AI-Topics/assets/108785228/f653f88b-e36f-4950-858d-553b5a4a3ce7)

                                    First five rows of the DataFrame 