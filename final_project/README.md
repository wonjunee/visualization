# Visualizing Fligh Paths of US Airports
## Udacity Data Analyst Nanodegree
## Data Visualization Course Final Project

### Summary

This final project visualizes the data of US Airports flight information in 2008. The data is visualized by nodes and edges where nodes represent the airports and the edges represent the possible flight paths. 

### Design

I used the US map as a background to show readers where exactly the airports are. The map is scaled so that the readers can see the nodes and edges clearly.

When the project starts, it shows the nodes and their edges one at a time. The order of the appearance is based on the activity of the airports (The most active airpots is shown first). Readers can see that the activity of the airports is not proportional to the number of edges in the visualization because not all of the US airports are shown on the map.

The nodes have different sizes of circles and the radius of circles are based on the number of edges of the nodes. I used only 30 airports because too many airports will only confuse readers without providing clear insights of the data.

### *Changes after feedback*

### Feedback


### Resources

I used the 2008 flight data from http://stat-computing.org/dataexpo/2009/the-data.html. The data don't contain the coordinate information of each airport so I used the second data from https://gist.githubusercontent.com/tdreyno/4278655/raw/7b0762c09b519f40397e4c3e100b097d861f5588/airports.json. To create a US map, I used world_countries.json from Visualization Course.

Summary - in no more than 4 sentences, briefly introduce your data visualization and add any context that can help readers understand it
Design - explain any design choices you made including changes to the visualization after collecting feedback
Feedback - include all feedback you received from others on your visualization from the first sketch to the final visualization
Resources - list any sources you consulted to create your visualization