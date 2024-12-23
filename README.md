# COVID-19-TOP-TEN-Santa-Clara-Cases-2020
      In this project, I set out to use the data from Santa Clara's County Public Health data for the COVID-19 cases across Santa Clara County from 2020. Using data science libraries in python, and the help of the public records accessed over SCCPH databases. I wrote the code and cleaned and visualized it using Matplotlib, pandas, and plotly. To minimize and clean up the charts used, I took the top ten zip codes across Santa Clara to visualize the cases by zip codes. With the help and collaboration from fellow programmers and LLM machines.

########################################Beginning of code####################################################

import pandas as pd
import matplotlib.pyplot as plt
import plotly.express as px

#Load the CSV file
file_path = "/Users/jerryjasso/Desktop/COVID_zip_FC.csv"
df = pd.read_csv(file_path)

#Rename 'zipcode' to 'ZIP' for consistency
df.rename(columns={'zipcode': 'ZIP'}, inplace=True)

#Remove rows with missing or NaN ZIP codes
df = df.dropna(subset=['ZIP'])

#Ensure ZIP codes are treated as integers(after dropping NaN values)
df['ZIP'] = df['ZIP'].astype(int)

#Summarize COVID-19 cases by ZIP code
zip_cases = df.groupby('ZIP')['Cases'].sum()

#Get the top 10 ZIP codes with the most reported cases
top_10_zip_cases = zip_cases.nlargest(10)

#######################
#Pie Chart: Top 10 ZIP Codes with the Most COVID-19 Cases
#######################

#Plot the pie chart for the top 10 ZIP codes
plt.figure(figsize=(10, 8))
colors = plt.cm.Paired(range(len(top_10_zip_cases))) #Distinct color palette
top_10_zip_cases.plot(kind='pie', autopct='%1.1f%%', startangle=140, cmap='Set3', colors=colors)
plt.title("Top 10 ZIP Codes with the Most COVID-19 Cases")
plt.ylabel('') #Hide ylabel for better appearance
plt.show() #shows the pie chart after the code runs the data

# Plot the bar chart for top 10 ZIP codes using Matplotlib
plt.figure(figsize=(12, 6))
top_10_zip_cases.plot(kind='bar', color=plt.cm.Paired(range(len(top_10_zip_cases))))
plt.title("Top 10 COVID-19 Cases by ZIP Code")
plt.xlabel("ZIP Code")
plt.ylabel("Number of Cases")
plt.xticks(rotation=45, ha="right") #Rotate ZIP codes for better readability
plt.tight_layout() #Adjust layout to make it fit nicely
plt.show() #This will display the bar chart inline(no browser)


#######################
#Bar Chart: COVID-19 Cases by ZIP Code(using Plotly)
#######################

#Plot the bar chart using Plotly for top 10 ZIP codes
fig = px.bar(top_10_zip_cases, x=top_10_zip_cases.index.astype(str), y=top_10_zip_cases.values,
             title="Top 10 COVID-19 Cases by ZIP Code",
             labels={'x': 'ZIP Code', 'y': 'Number of Cases'},
             color=top_10_zip_cases.values, color_continuous_scale='Viridis')

#Make sure the X-axis uses full ZIP codes as integers (no decimals)
fig.update_layout(
    xaxis=dict(tickmode='array', tickvals=top_10_zip_cases.index.astype(str)),
    xaxis_title='ZIP Code'
)

fig.show(renderer='browser') #This will open the chart in your browser

########################################End of code####################################################
