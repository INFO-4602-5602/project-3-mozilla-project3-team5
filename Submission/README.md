<h2>**Information about our visualization**<h2>
  * What they show
    * Graph 1
    this graph shows each device has how many peoples used. X axis is different devices and Y axis is numbers. The reason we create this visualization is that there are eleven types of device, so we want to find out the number of each type, and create the bar chart to show which one has the most number of usage.
    * Scatter plot for 'India - price'
    In this graph, we put price_level on the x-axis and total quantity on the y-axis. So, we can conclude that most people choose price_level 1, which means that they don't really concern about price when they buy internet stuff

    * Bar chart for 'India - price'
    This graph shows the same thing, but in form of bar chart

    * Link Name count
    In this graph, we place all the name for internet on the x-axis, and total quantity on the y-axis. From the line, we can conclude that more than 150 thousand people choose Snippet.

  * Information about interaction
    * Graph 1
    You can interact with each bar on the visualization. You can use mouse to put on the bars and it will shows the x and y values. At the same time the color will change to green from red.

    * Scatter plot for 'India - price'
    When your mouse on each point on the graph, you will be able to see more Information about this point you on, including: price level, quantity and country
    * Bar chart for 'India - price'
    For this graph, you can see a tool-box on the right hand side. If you choose the second tool from top, you are be able to zoom in particular bar chart that you square. Additionally, if you choose the third tool from top, you are be able to zoom in and out the entire bar chart
    * Link Name count
    When your mouse on each point on the graph, you will be able to see more Information about this point you on, including: Link name and total quantity of how many people choose this link.


##**Your design process**

**Yan's Code**

The first visualization, we use python3 do data clean(Jupyter) and use D3 show the graph. The file name is py.ipynb in the data process folder. First we read the file and extract the column: 'WiFi Router:Check all the internet connected devices you currently own:', 'Laptop computer:Check all the internet connected devices you currently own:', 'Smart phone:Check all the internet connected devices you currently own:', 'Smart TV:Check all the internet connected devices you currently own:', 'Activity Tracker (ex: Fitbit or Apple Watch):Check all the internet connected devices you currently own:', 'Smarthome Hub (ex. Amazon Echo, Google Alexa):Check all the internet connected devices you currently own:', 'Car that connects to the internet:Check all the internet connected devices you currently own:', 'Smart Thermostat (ex: Nest):Check all the internet connected devices you currently own:', 'Smart Appliance (ex. Coffeemaker, Refrigerator, Oven, Fridge):Check all the internet connected devices you currently own:', 'Smart Door Locks (ex. Door locks for your home you can open via bluetooth):Check all the internet connected devices you currently own:', 'Smart Lighting (ex. Connected lighting switches, dimmers, or bulbs):Check all the internet connected devices you currently own:'. Second we count each column numbers, then transfer to 1.csv file. the code is：

```
df= pd.read_csv('se.csv', encoding = "ISO-8859-1")
df1 = df[['WiFi Router:Check all the internet connected devices you currently own:', 'Laptop computer:Check all the internet connected devices you currently own:', 'Smart phone:Check all the internet connected devices you currently own:', 'Smart TV:Check all the internet connected devices you currently own:', 'Activity Tracker (ex: Fitbit or Apple Watch):Check all the internet connected devices you currently own:', 'Smarthome Hub (ex. Amazon Echo, Google Alexa):Check all the internet connected devices you currently own:', 'Car that connects to the internet:Check all the internet connected devices you currently own:', 'Smart Thermostat (ex: Nest):Check all the internet connected devices you currently own:', 'Smart Appliance (ex. Coffeemaker, Refrigerator, Oven, Fridge):Check all the internet connected devices you currently own:', 'Smart Door Locks (ex. Door locks for your home you can open via bluetooth):Check all the internet connected devices you currently own:', 'Smart Lighting (ex. Connected lighting switches, dimmers, or bulbs):Check all the internet connected devices you currently own:']]
df2= df1.count()
#print(df2)
df2.to_csv("1.csv")

```

**Yifan's Code**

We use pandas to do the data cleaning part

project3_raw is dataframe name before dropping all missing value
```
import pandas as pd
data = pd.read_csv('SurveyExport.csv',encoding = "ISO-8859-1")
project3_raw = data[['Country','Price','Features','Safety','Security','Privacy','Reliability','User Reviews','Expert Recommendation','Friend or Family Recommendation','Convenience']]
```
project3 is dataframe name with clean data

```
project3 = project3_raw.dropna()
# print(len(project3_raw))
project3.head()
```

Then, we change all number into integer type
```
project3['Price'] = project3['Price'].astype(int) # convert Price arrtibute from object to int
project3['Features'] = project3['Features'].astype(int)
project3['Safety'] = project3['Safety'].astype(int)
project3['Security'] = project3['Security'].astype(int)
project3['Privacy'] = project3['Privacy'].astype(int)
project3['Reliability'] = project3['Reliability'].astype(int)
project3['User Reviews'] = project3['User Reviews'].astype(int)
project3['Expert Recommendation'] = project3['Expert Recommendation'].astype(int)
project3['Friend or Family Recommendation'] = project3['Friend or Family Recommendation'].astype(int)
project3['Convenience'] = project3['Convenience'].astype(int)
project3.info()
```

This time, we are using bokeh for plotting our graph
We import some libraries from bokeh
```
from bokeh.io import output_file,show,output_notebook,push_notebook
from bokeh.plotting import figure
from bokeh.models import ColumnDataSource,HoverTool,CategoricalColorMapper
from bokeh.layouts import row,column,gridplot
from bokeh.models.widgets import Tabs,Panel
output_notebook()
```

Now, we are selecting all rows where country attribute is 'India' and store then in India
```
India = project3[(project3.Country == 'India')]
```

We get the price attribute from dataframe India and change the type to integer and storeed in India_Price
```
India_Price = India['Price'].astype(int)
```

Use '.value_counts()' to count total number for every unique price and store them in India_count
```
India_count = India_Price.value_counts()
India_count
```
From India_count, we get the only price value and store them in list1
```
list1 = list(India_count.keys().values)
list1
```

Get total number for each correspondence price and store them in list2
```
list2 = list(India_count.values)
list2
```

Now, we need to import HoverTool for tooltip
```
from bokeh.models import HoverTool

```

Code below is for plotting scatter graph for India_Price
```
#Graph a is scatter plot for 'India-price'
hover = HoverTool(tooltips=[
    ("Price Level ", "@x"),
    ("Quantity ", "@y"),
        ("Country","India")

])
a = figure(plot_width=400, plot_height=400, tools=[hover],
           title="India - Price")
source = ColumnDataSource(data=dict(
    x=list1,
    y=list2,
))

a.circle('x', 'y', size=7, source=source)
a.yaxis.axis_label = "Quantity"
a.xaxis.axis_label = "Price Level"

```

Code below is for plotting bar graph for India_Price

```
# Graph b is bar plot for 'India-price'

b = figure(plot_width=400, plot_height=400,title="India - Price")

b.yaxis.axis_label = "Quantity"
b.xaxis.axis_label = "Price Level"
b.vbar(x=list1, width=0.5, bottom=0,
       top=list2, color="blue")
```
Code below is for plotting line graph for link_name
```
# Graph p is line graph for "Link name"

Link_raw = data[['Link Name']]
Link = Link_raw['Link Name'].value_counts()
link_name = list(Link.keys().values)
link_value = list(Link.values)
dictionary = dict(zip(link_name,link_value))
dictionary
number  = [1,2,3,4,5,6,7,8,9,10]


source = ColumnDataSource(data=dict(
    x=link_name,
    y=link_value,
    z= number,
))

hover = HoverTool(tooltips=[
    ("Link Name ", "@x"),
    ("Link Value ", "@y"),
])
p = figure(plot_width=400, plot_height=400,tools=[hover],title="Link Name count")

# p = figure(x_range = link_name, y_range = link_value)
p.line(number, link_value, line_width=2)
p.circle('z', 'y', size=10,source = source)
p.yaxis.axis_label = "Quantity"
p.xaxis.axis_label = "Name Index"
```

For showing part, we just need to use 'show'.
For these graph, we choose to format them as column
```
show(column(a,b,p))
```

**Zening's explanation for Link Name graph**
The visualization is a line chart. We use the Link Name attribute, and we count the total number of the Link Name as the second attribute. The second attribute called Total number. The Link Name is for the x axis, and the Total number is for the y axis. The reason we create this visualization is that there are ten types of Link, so we want to find out the number of each type, and create the line chart to show which one has the most number of usage and the slope of each point on the visualization. We use Python to process the data. First of all, we shrink the whole data, which only has the Link Name. Then, we use count the total number of each type of link. Finally, we store the correct data into a new csv file. Everything we do are in Jupyter Notebook by pandas. After we construct the data, we used Python to creates the graph. You can interact with each point on the visualization. You can use mouse to put on the points and it will shows the x and y values. The way to see this visualization is that use Python to create a server and run the html file.


##**Team Role**

* Peng Yan do:
  * Write some part of the readme.
  * Design the bar chart.
  * Create the data for the bar chart.
  * Create and solve some problems of the bar chart.

* Zening do:
  * Write some part of the readme.
  * Design the line chart.
  * Create the data for the line chart.
  * Work with Yifan Li and Peng Yan to create and solve some problems of the line chart.


##**Above and Beyond**

##**How to run our project**

here is for how to run our project
