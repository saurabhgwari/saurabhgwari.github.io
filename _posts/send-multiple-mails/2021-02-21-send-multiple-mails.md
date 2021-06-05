---
layout: post
title:  "Sending Automated Custom mails to multiple locations using Python"
date:   2021-02-21 09:29:20 +0700
tags: [Python,Automation, HTML]
---

In this project, I automate the task of sending custom emails to 20 different locations. Let's start by importing data using pandas and have a quick look,  list of locations are stored in "input.xlsx" file.

```python
import pandas as pd
import os
import win32com.client as win32

fileDir = os.getcwd()
fileName = "input.xlsx"

df = pd.read_excel(io=fileDir+"\\"+fileName)
df.head()
```

Emailer function below takes input each row from above table and generates draft email. Here the name of person changes in each email, but other values such as "Status" can be used in email body as well with attachments etc.


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Location</th>
      <th>Status</th>
      <th>Email</th>
      <th>To</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Canada</td>
      <td>99.898515</td>
      <td>Mila@Canadaoffice.com</td>
      <td>Mila</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Japan</td>
      <td>51.443222</td>
      <td>Nova@Japanoffice.com</td>
      <td>Nova</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Germany</td>
      <td>65.714893</td>
      <td>Kai@Germanyoffice.com</td>
      <td>Kai</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Switzerland</td>
      <td>15.359779</td>
      <td>Aaliyah@Switzerlandoffice.com</td>
      <td>Aaliyah</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Australia</td>
      <td>80.806992</td>
      <td>Braxton@Australiaoffice.com</td>
      <td>Braxton</td>
    </tr>
  </tbody>
</table>
</div>




```python
def Emailer(Bodytext, subject, recipient):
    outlook = win32.Dispatch('outlook.application')  
    mail = outlook.CreateItem(0)
    mail.To = recipient
    mail.Subject = subject
    mail.HtmlBody = Bodytext
    #mail.Attachments.Add(AttachFile)
    mail.Save()

    
for label,row in df.iterrows():
    officeName = df.loc[label,"Location"]
    officeSPOC = df.loc[label,"Email"]
    SPOCname = df.loc[label,"To"]
    StatusValue = df.loc[label,"Status"]
     
    Bodytext =  f"""\
    <html>
    <head></head>
    <body style="font-family:verdana; font-size:13px">

    Hi {SPOCname},<br><br>
    
    This is a sample email<br><br>
    
    Please ignore.<br><br>

    Regards,<br><br>
    
    <b style="color:rgb(169,169,169); font-size:11px">
    <b style="color:rgb(0,0,0)">Your Name & Designation</b><br>
    Company Name<br>
    <a href="https://www.bain.com/">Company.com</a>
    </b>


    </body>
    </html>"""
    
    sub = r"Testing Email - "+officeName
    
    Emailer(Bodytext, sub, officeSPOC)
```

I have used HTML to define body of email, which allows formatting email as per our need. Finally the output of this scipt is generated in outlook's draft folder :

<figure>
    <img src="../assets/img/post_img/send-multiple-mails/drafts.png" alt="Draft folder screenshot">
    <figcaption>Draft folder with newly generated emails</figcaption>
</figure>

Also, notice the custom subject & greetings in every email.

<figure>
    <img src="../assets/img/post_img/send-multiple-mails/mail.PNG" alt="Generated email sample">
    <figcaption>Sample Email generated</figcaption>
</figure>

Final note : Python scripts can really help in automating tedious tasks, giving us time to really think about the bigger picture.  Feel free to use above script in your workflow, however keep in mind this will require some modifications based on your use case.