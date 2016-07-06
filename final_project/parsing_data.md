
#### Note:
**2008.csv** file is too large so it is not in my git repository. If you want to run the scripts then you may have to download the file from http://stat-computing.org/dataexpo/2009/the-data.html.

# Parsing the data
### Final Project - Data Visualization Course
### Udacity Data Analyst Nanodegree

For this final project I will use 2008 flights data that I downloaded from http://stat-computing.org/dataexpo/2009/the-data.html. The file doesn't have the geographic coordination of each airport so I downloaded the file that contains the information of coordinates of each airport from https://gist.githubusercontent.com/tdreyno/4278655/raw/7b0762c09b519f40397e4c3e100b097d861f5588/airports.json.


First I will load the json file and save it as a dictionary.


```python
### import necessary libraries
import json

### Create an empty dictionary
airport_dict = {}

### load the json file
count = 0
with open("airport.json") as F:
    for i in json.load(F):
        try:
            airport_dict[i['code']] = {'lon': i['lon'], 'lat': i['lat']}
        except:
            print i['code'], ":", i['name']
            count += 1
```


```python
print "Saved airports:",len(airport_dict)
print "Excluded airports:", count
```

    Saved airports: 3885
    Excluded airports: 0


Now we will load the flights file and parse the data.

Since there are too many records in this file I will filter the january data only.

When you look at the data in **2008.csv** the same Destination and Origin are repeated at the same day with different hours. I will include only one of them and exclude redundant data.


```python
### load csv library
import csv

### List for avoiding redundancy
redun = []

### load csv data
id_count = 0
with open("2008.csv") as csvfile:
    with open("airport_coord.csv", "wb") as wfile:
        fieldnames = ['id', 'day', 
                      'Dest', 'Dest_lon', 'Dest_lat', 
                      'Origin', 'Origin_lon', 'Origin_lat']
        writer = csv.DictWriter(wfile, fieldnames=fieldnames)
        writer.writeheader()
        
        ### write rows
        for i in csv.DictReader(csvfile):
            if int(i["Month"]) == 1:
                if i['Dest'] != 'OGD':
                    tmp_dict = {}
                    tmp_dict['day'] = i["DayofMonth"]
                    tmp_dict['Dest'] = i['Dest']
                    tmp_dict['Dest_lon'] = airport_dict[i['Dest']]['lon']
                    tmp_dict['Dest_lat'] = airport_dict[i['Dest']]['lat']
                    tmp_dict['Origin'] = i['Origin']
                    tmp_dict['Origin_lon'] = airport_dict[i['Origin']]['lon']
                    tmp_dict['Origin_lat'] = airport_dict[i['Origin']]['lat']
                    
                    ### Check if the item is redundant
                    if tmp_dict['day']+tmp_dict['Dest']+tmp_dict['Origin'] not in redun:
                        redun.append(tmp_dict['day']+tmp_dict['Dest']+tmp_dict['Origin'])
                        tmp_dict['id'] = id_count

                        ### Write rows
                        writer.writerow(tmp_dict)
                        id_count += 1
```

Only the records containing **OGD** are excluded. Everything else is saved as a csvfile.


```python
id_count-1
```




    134653



There are total **134653** records in the saved csv file. Stil there are too many so I will filter the data further. First I will check the most frequently appeared **Destination** and **Origin**.


```python
### Create a dictionary for Destinations and Origins
Dest_dict = {}

### Load airport_coord.csv file
with open("airport_coord.csv") as F:
    for i in csv.DictReader(F):
        ### Count Destinations
        if i["Dest"] in Dest_dict.keys():
            Dest_dict[i["Dest"]]+=1
        else:
            Dest_dict[i["Dest"]]=1

### Sort by value
import pprint
import operator
Dest_sorted = sorted(Dest_dict.items(), key=operator.itemgetter(1))

### Print out the result
print "Destinations"
pprint.pprint(Dest_sorted[-15:])
```

    Destinations
    [('MEM', 2107),
     ('MCO', 2147),
     ('LAX', 2332),
     ('LAS', 2553),
     ('EWR', 2594),
     ('CVG', 2669),
     ('PHX', 2720),
     ('SLC', 2956),
     ('MSP', 3257),
     ('IAH', 3281),
     ('DTW', 3344),
     ('DEN', 3377),
     ('DFW', 3958),
     ('ORD', 4218),
     ('ATL', 4945)]


Above is the list of top **15** destinations that will be used in the data visualization project.


```python
### Save the list of top 15 airports from the array
top_Dest = []

for i in Dest_sorted[-15:]:
    top_Dest.append(i[0])
```


```python
### open and filter the data
id_num = 0
with open("airport_coord.csv") as F:
    with open("airport_filtered.csv", "wb") as WF:
        fieldnames = ['id', 'day', 
                      'Dest', 'Dest_lon', 'Dest_lat', 
                      'Origin', 'Origin_lon', 'Origin_lat']
        writer = csv.DictWriter(WF, fieldnames=fieldnames)
        writer.writeheader()
        
        ### write rows
        tmp_line = ""
        for i in csv.DictReader(F):
            if i["Dest"] in top_Dest:
                i['id'] = id_num
                writer.writerow(i)
                id_num+=1
```


```python
id_num-1
```




    46457



There are now total **46457** records in the file. This file will be used in **html** for data visualization using **D3.js**. You can see the result in **final_project.html**.
