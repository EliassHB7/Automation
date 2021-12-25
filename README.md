# Automation of data cleanup

# Importing all necessary libraries
from google.colab import auth
auth.authenticate_user()
import pandas as pd
import gspread
import matplotlib.pyplot as plt
from oauth2client.client import GoogleCredentials

# Connecting with my Google account
gc = gspread.authorize(GoogleCredentials.get_application_default())

# Importing the Google sheets API
ws = gc.open_by_url('https://docs.google.com/spreadsheets/url').worksheet('sheetname')

# get_all_values gives a list of rows.
rows = ws.get_all_values()

# Converting to a DataFrame and selecting all rows starting from the 1st one
df = pd.DataFrame.from_records(rows[1:],columns=rows[0])

# Deleting the dots "." in col1
df["col1"] = df["col1"].apply(lambda x: x.replace(".", ""))

# Delete unwanted columns 4 and 5
to_drop = ['col4','col5']
df.drop(to_drop, inplace = True, axis = 1)

# Reordering the columns in the desired order
df = df[['col2','col1','col3']]

# Displaying the dataframe
display(df)
