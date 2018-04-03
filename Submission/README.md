```
import pandas as pd
data = pd.read_csv('SurveyExport.csv',encoding = "ISO-8859-1")
project3_raw = data[['Country','Price','Features','Safety','Security','Privacy','Reliability','User Reviews','Expert Recommendation','Friend or Family Recommendation','Convenience']]
project3 = project3_raw.dropna()
# print(len(project3_raw))
project3.head()
```
We use pandas to do the data cleaning part by dropping all row contained missing value and selecting Country and results column
