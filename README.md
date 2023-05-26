# Ex-08-Data-Visualization-

## AIM
To Perform Data Visualization on the given dataset and save the data to a file. 

# Explanation
Data visualization is the graphical representation of information and data. By using visual elements like charts, graphs, and maps, data visualization tools provide an accessible way to see and understand trends, outliers, and patterns in data.

# ALGORITHM
### STEP 1
Read the given Data
### STEP 2
Clean the Data Set using Data Cleaning Process
### STEP 3
Apply Feature generation and selection techniques to all the features of the data set
### STEP 4
Apply data visualization techniques to identify the patterns of the data.


# CODE
NAME:Priyadharshini.P
REGISTER NO:212222100039
```
#DEVELOPED BY:SWETHA P 
#REGISTER NUMBER: 212222100053
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv("Superstore.csv",encoding='unicode_escape')
df
df.head()
df.info()
df.drop('Row ID',axis=1,inplace=True)
df.drop('Order ID',axis=1,inplace=True)
df.drop('Customer ID',axis=1,inplace=True)
df.drop('Customer Name',axis=1,inplace=True)
df.drop('Country',axis=1,inplace=True)
df.drop('Postal Code',axis=1,inplace=True)
df.drop('Product ID',axis=1,inplace=True)
df.drop('Product Name',axis=1,inplace=True)
df.drop('Order Date',axis=1,inplace=True)
df.drop('Ship Date',axis=1,inplace=True)
print("Updated dataset")
df
df.isnull().sum()
#detecting and removing outliers in current numeric data
import matplotlib.pyplot as plt
plt.figure(figsize=(8,8))
plt.title("Data with outliers")
df.boxplot()
plt.show()
plt.figure(figsize=(8,8))
cols = ['Sales','Quantity','Discount','Profit']
Q1 = df[cols].quantile(0.25)
Q3 = df[cols].quantile(0.75)
IQR = Q3 - Q1
df = df[~((df[cols] < (Q1 - 1.5 * IQR)) |(df[cols] > (Q3 + 1.5 * IQR))).any(axis=1)]
plt.title("Dataset after removing outliers")
df.boxplot()
plt.show()
#Which Segment has Highest sales?
sns.lineplot(x="Segment",y="Sales",data=df,marker='o')
plt.title("Segment vs Sales")
plt.xticks(rotation = 90)
plt.show()
sns.barplot(x="Segment",y="Sales",data=df)
plt.xticks(rotation = 90)
plt.show()
#Which City has Highest profit?
df.shape
df1 = df[(df.Profit >= 60)]
df1.shape
plt.figure(figsize=(30,8))
states=df1.loc[:,["City","Profit"]]
states=states.groupby(by=["City"]).sum().sort_values(by="Profit")
sns.barplot(x=states.index,y="Profit",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("City")
plt.ylabel=("Profit")
plt.show()
#Which ship mode is profitable?
sns.barplot(x="Ship Mode",y="Profit",data=df)
plt.show()
sns.lineplot(x="Ship Mode",y="Profit",data=df)
plt.show()
sns.violinplot(x="Profit",y="Ship Mode",data=df)
plt.show()
sns.pointplot(x=df["Profit"],y=df["Ship Mode"])
plt.show()
#Sales of the product based on region
states=df.loc[:,["Region","Sales"]]
states=states.groupby(by=["Region"]).sum().sort_values(by="Sales")
sns.barplot(x=states.index,y="Sales",data=states)
plt.xticks(rotation = 90)
plt.xlabel=("Region")
plt.ylabel=("Sales")
plt.show()
df.groupby(['Region']).sum().plot(kind='pie', y='Sales',figsize=
(6,9),pctdistance=1.7,labeldistance=1.2)
#Find the relation between sales and profit.
df["Sales"].corr(df["Profit"])
df_corr = df.copy()
df_corr = df_corr[["Sales","Profit"]]
df_corr.corr()
sns.pairplot(df_corr, kind="scatter")
plt.show()
#Heatmap
df4=df.copy()
#encoding
from sklearn.preprocessing import LabelEncoder,OrdinalEncoder,OneHotEncoder
le=LabelEncoder()
ohe=OneHotEncoder
oe=OrdinalEncoder()
df4["Ship Mode"]=oe.fit_transform(df[["Ship Mode"]])
df4["Segment"]=oe.fit_transform(df[["Segment"]])
df4["City"]=le.fit_transform(df[["City"]])
df4["State"]=le.fit_transform(df[["State"]])
df4['Region'] = oe.fit_transform(df[['Region']])
df4["Category"]=oe.fit_transform(df[["Category"]])
df4["Sub-Category"]=le.fit_transform(df[["Sub-Category"]])
#scaling
from sklearn.preprocessing import RobustScaler
sc=RobustScaler()
df5=pd.DataFrame(sc.fit_transform(df4),columns=['Ship Mode', 'Segment', 'City','State','Region','Category','Sub-Category','Sales','Quantity','Discount','Profit'])
plt.subplots(figsize=(12,7))
sns.heatmap(df5.corr(),cmap="PuBu",annot=True)
plt.show()
#Find the relation between sales and profit based on the following category.
#Segment
grouped_data = df.groupby('Segment')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'],
label='Profit')
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
ax.legend()
plt.show()
#City
grouped_data = df.groupby('City')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'],
label='Profit')
ax.set_xlabel('City')
ax.set_ylabel('Value')
ax.legend()
plt.show()
#States:
grouped_data = df.groupby('State')[['Sales', 'Profit']].mean()
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
ax.bar(grouped_data.index, grouped_data['Sales'], label='Sales')
ax.bar(grouped_data.index, grouped_data['Profit'], bottom=grouped_data['Sales'],
label='Profit')
ax.set_xlabel('State')
ax.set_ylabel('Value')
ax.legend()
plt.show()
#Segment and Ship Mode
grouped_data = df.groupby(['Segment', 'Ship Mode'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index='Segment', columns='Ship Mode',
values=['Sales', 'Profit'])
# Create a bar chart of the grouped data
fig, ax = plt.subplots()
pivot_data.plot(kind='bar', ax=ax)
ax.set_xlabel('Segment')
ax.set_ylabel('Value')
plt.legend(title='Ship Mode')
plt.show()
#Segment, Ship mode and Region
grouped_data = df.groupby(['Segment', 'Ship Mode','Region'])[['Sales', 'Profit']].mean()
pivot_data = grouped_data.reset_index().pivot(index=['Segment', 'Ship Mode'],
columns='Region', values=['Sales', 'Profit'])
sns.set_style("whitegrid")
sns.set_palette("Set1")
pivot_data.plot(kind='bar', stacked=True, figsize=(10, 5))
plt.xlabel('Segment - Ship Mode')
plt.ylabel('Value')
plt.legend(title='Region')
plt.show()
```
# DATA SET:
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/c481d429-fb24-47a9-b381-720cafa44237)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/10894345-6ee0-4a6d-b6ec-3d83e646ecc9)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/2102f5ed-ea6f-4b04-aa5f-a82a29f0ec5f)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/cce91570-ab75-4d60-bd90-9c0e464cfb9f)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/2bf4ddec-6bf8-4a74-bf7a-40746d5ca820)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/a8b05bbe-4232-45c0-b465-355ae7ddb424)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/a4097965-a432-44bb-aec6-61de0b826e5d)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/29f6a815-bfbe-4660-9d05-37fc809903ff)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/737bf00f-0ee9-458c-916f-4a01f91e6f18)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/d6d9d1d2-5d8c-4f9b-8ddc-93444e3ff7cd)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/db4da64f-a49b-4a90-9d8e-395f3a81d94a)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/33ba1b46-09c6-4158-81d9-131d95030dff)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/2e2a6b9f-739a-4cc8-9048-e1949fd2c239)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/a3f08772-39e7-42bc-b512-855ba4306684)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/9dbf2a8b-51d8-473d-a50f-aa4fc45be910)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/56e402a7-a569-4c35-b55f-a701cfec6254)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/3456a2d7-c350-4aa8-a434-e869f4ee9c5b)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/b2dca098-252b-4f78-8185-d4e8a69acd5a)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/af6bfb5e-e2e6-4656-9721-4b988d713a17)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/bb873050-c3f8-4f71-a6b4-d8903411bada)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/51c49a98-2e43-4478-b4fd-00921d1159da)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/de134f14-2860-4bee-b29a-fba8a708e054)
![image](https://github.com/Priyadharshini-Er/Ex-08-Data-Visualization-/assets/119558093/0d2dd6db-7db1-4cfd-a7b3-1221d1fed5ec)
