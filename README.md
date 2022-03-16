# Gmail_API_GUIDE
># Here's an article on how to use the Gmail API.
># This is an extension from the Youtube API GUIDE article. So please read it first.  

---
# quick start  
1) basic code structure  
2) You can find and write parameter meanings.  
3) You can find and write what you want from the code samples provided on the official site.  
---


# basic code structure  
```python
import os
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from googleapiclient.discovery import build
     
#Get Oauth Certificate Access File
flow = InstalledAppFlow.from_client_secrets_file("client_secrets.json",
                                                 scopes=["(the scope you want)"])

flow.run_local_server(port=8080, prompt="consent")

credentials = flow.credentials


gmail = build('gmail', 'v1', credentials=credentials)
```
># Get an object to connect to the server and use the gmail API as shown in the following code.  
># In the gmail API, we use function methods, not functions.  
># ![image](https://user-images.githubusercontent.com/87273590/158611355-52cbd3b5-1fa9-4355-89a2-905b4b22ccf6.png)  
># It is a structure with several methods for the users function.  
```python
gmail.users()."""method name to write(
            'desired parameters'
            ).execute()"""
```
># You may not understand. Don't worry, just follow me  

# You can find and write parameter meanings.  
># ![image](https://user-images.githubusercontent.com/87273590/158612909-544ed48a-afbd-44a1-a254-cc73e7a41861.png)  
># ![image](https://user-images.githubusercontent.com/87273590/158612963-9b8b124d-7e82-4e42-9c3f-056d3a897020.png)  
># Choose the method you want to use  
># ![image](https://user-images.githubusercontent.com/87273590/158613398-e1f0a94f-9397-4e76-b291-500572462a29.png)  
># And I figure out what the method I choose wants.  
># I want the current User Id value, and how to give it is also shown.  
># 'me' or email address   
```python
import os
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from googleapiclient.discovery import build
     
#Get Oauth Certificate Access File
flow = InstalledAppFlow.from_client_secrets_file("client_secrets.json",
                                                 scopes=["https://www.googleapis.com/auth/gmail.modify"])

flow.run_local_server(port=8080, prompt="consent")

credentials = flow.credentials


gmail = build('gmail', 'v1', credentials=credentials)

requests = gmail.users().getProfile(
        userId='me'
    ).execute()

print(requests)
```
```json
{'emailAddress': 'anonymous@gmail.com', 'messagesTotal': 123, 'threadsTotal': 123, 'historyId': '#####'}
```
># You can get it in the form of a dictionary like this:  
># ![image](https://user-images.githubusercontent.com/87273590/158614694-a17eed7d-5d9f-4ec1-a79e-86ceb023a02d.png)  
># If you want to know what the response value is, you can lower it a bit.  

# You can find and write what you want from the code samples provided on the official site.  
># ![image](https://user-images.githubusercontent.com/87273590/158627410-8f2bebb5-8610-47ce-a6ee-7f0add18685f.png)  
># You can't write code just by looking at this content.  
># So you should look at the example code.  
># ![image](https://user-images.githubusercontent.com/87273590/158627566-a84513a4-8d8a-483c-adac-2ae97a2bcab8.png)  
># ![image](https://user-images.githubusercontent.com/87273590/158627620-f5b6129c-c640-4534-b0d5-acbe074abcd9.png)  
># After seeing the example, you should be able to extract it like the code below.(If you have the skills, you can do it)  

```python
import os
import base64
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
from googleapiclient.discovery import build
from googleapiclient.errors import HttpError
from google.oauth2.credentials import Credentials
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

#Get Oauth Certificate Access File
flow = InstalledAppFlow.from_client_secrets_file("client_secrets.json",
                                                 scopes=["https://mail.google.com/"])

flow.run_local_server(port=8080, prompt="consent")

credentials = flow.credentials


gmail = build('gmail', 'v1', credentials=credentials)

def send_message(service, sender, to, subject, message_text):
    message = MIMEText(message_text)
    message['to'] = to
    message['from'] = sender
    message['subject'] = subject
    raw = base64.urlsafe_b64encode(message.as_bytes())
    raw = raw.decode()
    body = {'raw' : raw}
    message = (service.users().messages().send(userId='me', body=body).execute())
    print('Message Id: {}'.format(message['id']))
    return message

print(send_message(gmail,
                    "(your email)",
                    "(your email)",
                    "test",
                    "Test message, have you been well?"))

```
># ![image](https://user-images.githubusercontent.com/87273590/158627909-4d6b767e-9c4b-4f1e-b104-6d1e40839e0f.png)  
># good  
