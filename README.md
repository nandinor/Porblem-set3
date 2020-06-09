# Problem-Set3

This repo contains the scripts to get the data and graph

Student: Norman Andino

## Problem 1

data = open("data/CO-OPS__8729108__wl.csv", "r")
next(data)
highestWaterLevel= float(- 1.0)
date = ""
for x in data :
    columns = x.split(",")
    if columns[1]=='':
        columns[1]='-1.0'
    waterLevel = float(columns[1])
    if waterLevel > highestWaterLevel:
        highestWaterLevel = waterLevel
        date = columns[0]
print("Date:{} highest water level:{}".format(date, highestWaterLevel))

Date:2018-10-10 18:06 highest water level:6.647

## Problem 2

import pandas as pd

data = pd.read_csv("data/CO-OPS__8729108__wl.csv", float_precision='round_trip', usecols= [0,1], names=['Date Time', 'Water Level'], header=1)
dataFrame = pd.DataFrame(data)
highestWaterLevel = dataFrame['Water Level'].max()
date = dataFrame.iloc[420][0]
print("Date:{} highest water level:{}".format(date, highestWaterLevel))

Date:2018-10-10 18:06 highest water level:6.647

## Problem 3

import pandas as pd

data = pd.read_csv("data/CO-OPS__8729108__wl.csv", float_precision='round_trip', usecols= [0,1], names=['Date Time', 'Water Level'], header=1)
dataFrame = pd.DataFrame(data)
differenceWaterLevel = float(-1)
for i in range(dataFrame.first_valid_index(),dataFrame.last_valid_index()):
    firstWaterLevel = float(dataFrame.iloc[i][1])
    secondWaterLevel = float(dataFrame.iloc[i+1][1])
    if secondWaterLevel > firstWaterLevel and differenceWaterLevel < secondWaterLevel - firstWaterLevel:
        date1 = dataFrame.iloc[i][0]
        date2 = dataFrame.iloc[i+1][0]
        #index = i
        differenceWaterLevel = secondWaterLevel - firstWaterLevel
print("Period:{} - {} highest change:{}".format(date1, date2, differenceWaterLevel))

Period:2018-10-10 17:36 - 2018-10-10 17:42 highest change:0.6400000000000006

## Problem 4

import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

data = pd.read_csv("data/CO-OPS__8729108__wl.csv", float_precision='round_trip', usecols= [0,1], names=['Date Time', 'Water Level'], header=0)
dataFrame = pd.DataFrame(data)
print(dataFrame)
for i in range(dataFrame.first_valid_index(),dataFrame.last_valid_index()+1):
    if i == 0:
        waterLevel = [(dataFrame.iloc[i][1])]
        date = [0]
    waterLevel.append(dataFrame.iloc[i][1])
    date.append(date[-1]+6)
fig= plt.figure(figsize=(20,10))
plt.plot(date, waterLevel)
plt.show()

## Extra Credit: I would like to point out that I used python 2.7 for this exercsie and I got the data from 06-08-20 to 06-09-20

import urllib2
import json

urllib2.urlopen('https://tidesandcurrents.noaa.gov//api/datagetter?product=water_level&application=NOS.COOPS.TAC.WL&begin_date=20200608&end_date=20200609&datum=MLLW&station=8729108&time_zone=GMT&units=english&format=json')

print json.load(urllib2.urlopen('https://tidesandcurrents.noaa.gov//api/datagetter?product=water_level&application=NOS.COOPS.TAC.WL&begin_date=20200608&end_date=20200609&datum=MLLW&station=8729108&time_zone=GMT&units=english&format=json'))

url = 'https://tidesandcurrents.noaa.gov//api/datagetter?product=water_level&application=NOS.COOPS.TAC.WL&begin_date=20200608&end_date=20200609&datum=MLLW&station=8729108&time_zone=GMT&units=english&format=json'
json_obj = urllib2.urlopen(url)
data = json.load(json_obj)
for item in data['data']:
    print item ['t']
    print item ['v']
    print item ['s']
    print item ['f']
    print item ['q']



## Web scraping (It seems does not work out) functions run with python 3.8 full

from selenium import webdriver
from BeautifulSoup import BeautifulSoup
import pandas as pd

driver = webdriver.Chrome("/usr/lib/chromium-browser/chromedriver")

Date=[]
Time=[]
Predicted=[]
Preliminary=[]
Verified=[]
driver.get("<a href="https://tidesandcurrents.noaa.gov/waterlevels.">https://tidesandcurrents.noaa.gov/waterlevels.html?id=8729108&units=standard&bdate=20200603&edate=20200603&timezone=GMT&datum=MLLW&interval=6&action=data")
content = driver.page_source
soup = BeautifulSoup(content)
for a in soup.findAll('div',href=True, attrs={'class':'span12'}):
data=a.find('div', attrs={'div':'data_listing'})
Date.append(Date.text)
Time.append(Time.text)
Predicted.append(Predicted.text)
Preliminary.append(Preliminary.text)
Verified.append(Verified.text)
    
python web-s.gyp
df = pd.DataFrame({'Data':datas,'Time':times,'Predicted':predictings, 'Preliminary':Preliminaries, 'Verified':Verifies}) 
df.to_csv('Data.csv', index=False, encoding='utf-8')
