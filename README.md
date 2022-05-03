# Automation of Data Cleanup on Python

## Part 1: Importing and cleaning your database

### Importing all the necessary libraries
from google.colab import auth
auth.authenticate_user()
import pandas as pd
import gspread
import matplotlib.pyplot as plt
from oauth2client.client import GoogleCredentials
import numpy as np
from google.colab import files

### Connecting with your Google account
gc = gspread.authorize(GoogleCredentials.get_application_default())

### Importing the Google sheets API
ws = gc.open_by_url('https://docs.google.com/spreadsheets/url').worksheet('sheet1')

### From the list of rows
rows = ws.get_all_values()

### Convert all rows to a DataFrame
df = pd.DataFrame.from_records(rows[1:],columns=rows[0])

### Delete all the dots "." from a column x
df["colx"] = df["colx"].apply(lambda x: x.replace(".", ""))

### Delete unwanted columns 4 and 5
to_drop = ['col4','col5']
df.drop(to_drop, inplace = True, axis = 1)

### Reordering the columns in the desired order
df = df[['col2','col1','col3']]

### Displaying your cleaned data
display(df)

## Part 2: Performing a vlookup from another google sheets

### Importing a mapping sheet
ws2 = gc.open_by_url('https://docs.google.com/spreadsheets/url').worksheet('sheet2')

### Get all the values in the sheet as a list
rows2 = ws2.get_all_values()

### Convert this list to a dictionary: This is now the base of the mapping for the vlookup
mapping = {x[0]:x[1] for x in rows2}

### Performing the vlookup (note that there are other ways to perform a vlookup such as joins)
df['Col1'] = df['Col1'].map(mapping)

### Replacing / Deleting any values from certain columns:

df["Col1"] = df["Col1"].apply(lambda x: x.replace("blabla", "blibli"))
df["Col2"] = df["Col2"].apply(lambda x: x.replace("bleble", ""))

### Concatenating 2 columns: 
df["Col4"] = df["Col4"].astype(str) + df["Col5"].astype(str)

### Deleting any unnecessary columns:
to_drop = ['Col5','Col7','Col8']
df.drop(to_drop, inplace = True, axis = 1)
display(df)

### Your data is now clean, with the vlookup rendered properly and unnecessary columns deleted

## Part 3: Download the new clean database:

### Downloading the file as a CSV (note that it could be any other format)
df.to_csv('automated python.csv',encoding = 'utf-8-sig', index=False) 
files.download('automated python.csv')

## The file is now downloaded on your hard drive computer as CSV
