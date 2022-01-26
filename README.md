# Automation of Data Cleanup on Python Colab

## Importing all the necessary libraries
from google.colab import auth
auth.authenticate_user()
import pandas as pd
import gspread
import matplotlib.pyplot as plt
from oauth2client.client import GoogleCredentials

## Connecting with your Google account
gc = gspread.authorize(GoogleCredentials.get_application_default())

## Importing the Google sheets API
ws = gc.open_by_url('https://docs.google.com/spreadsheets/url').worksheet('sheetname')

## From the list of rows
rows = ws.get_all_values()

## Convert all rows to a DataFrame
df = pd.DataFrame.from_records(rows[1:],columns=rows[0])

## Delete all the dots "." from a column x
df["colx"] = df["colx"].apply(lambda x: x.replace(".", ""))

## Delete unwanted columns 4 and 5
to_drop = ['col4','col5']
df.drop(to_drop, inplace = True, axis = 1)

## Reordering the columns in the desired order
df = df[['col2','col1','col3']]

## Displaying the dataframe
display(df)

## Your clean Datafram is now ready to work with
