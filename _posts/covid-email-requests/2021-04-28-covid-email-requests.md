---
layout: post
title:  "Classify COVID related email requests using Python "
date:   2021-04-28 09:29:20 +0700
tags: [Python, Pandas, Plotly]
---

2nd wave of COVID impacted India hard and over month of April & May a lot of my collegues were requesting help (bed, oxygen etc). With all these emails coming in, I wanted to know which this is required more. 

For this analysis I decided to import last 3 months emails from my inbox and using subject filter only requests.

```python
import win32com.client
import pandas as pd
import plotly.express as px

outlook = win32com.client.Dispatch("Outlook.Application").GetNamespace("MAPI")

inbox=outlook.GetDefaultFolder(6) #Inbox default index value is 6
messages=inbox.Items

raw_subject_list = []
raw_body_list = []
raw_SentOnDate_List = []
raw_Sender_List = []
raw_To_List = []

for m in messages:
    if m.Class == 43: #MailItems class code is 43
        raw_subject_list.append(m.Subject)
        raw_body_list.append(m.body)
        raw_SentOnDate_List.append(m.senton.date())
        raw_Sender_List.append(m.Sender)
        raw_To_List.append(m.To)


dict_test = {
            'Subject':raw_subject_list, 
            'body':raw_body_list,
            'SentOnDate':raw_SentOnDate_List,
            'Sender':raw_Sender_List,
            'To':raw_To_List
            }


Email_Category_List = ['plasma','injection','bed','oxygen']

def CategoriseSubject(sub):
    for Mword in Email_Category_List:
        if sub.find(Mword) != -1:
            return Mword
    return 'N/A'

df_test_og = pd.DataFrame.from_dict(dict_test, dtype=str)


for label,row in df_test.iterrows():
    df_test.loc[label,"Category"] = CategoriseSubject(df_test.loc[label,"Subject"].lower())
    
    
df_test = df_test[df_test['Category']!='N/A']
df_test['is_Reply'] = df_test['Subject'].apply(lambda x : x[:2]=='RE')
df_test = df_test[df_test['is_Reply']==False]
```

Next, I wanted to see daily trend of requests coming in so I created a plotly chart:

<iframe src='../assets/img/post_img/covid-email-requests/linegraph.html' width="100%" height="400px"></iframe>


Now I will categories all requests into 4 categories :

<iframe src='../assets/img/post_img/covid-email-requests/bargraph.html' width="100%" height="400px"></iframe>


Conclusion : 
- 25th Apr - 4th May week had highest impact in daily trend
- Requests for Bed & Plasma were highest