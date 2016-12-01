# How To Build A Chatbot - Part 1
Learn to build a facebook chatbot using Python and Flask ( work in progress... )

### Table of Contents

* [Create Facebook Page](#create-a-facebook-page)
* [Installation](#stuff-to-install-and-setup)
    * [Download Project]()
    * [Setup Virtual Environment]()
    * [Install Dependencies]()
* [Server](#setting-up-your-first-flask-server)
    * [Flask]()
    * [ngrok]()
* [Code for the bot](#code-breakdown)
    * [Verify Method]()
    * [Webhook Method]()
    * [Send Message Method]()
    
  
### Create a Facebook Page

#### Now Create a Facebook App 
Click on this [Link](https://developers.facebook.com/quickstarts/?platform=web) to access FB's developer's console.


### Stuff to install and setup

#### Prereq: Make sure you have python installed on your laptop

#### 2. Download Project Files

#### 3. Install Dependencies

Navigate into the downloaded folder using ```python cd```

```python 
pip install -r requirements.txt
```

### Setting up your first flask server

```python 

from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()

```

Open localhost:5000

```python

./ngrok http 5000

```


###  Code Breakdown

#### verify() method

```python

@app.route('/', methods=['GET'])
def verify():

    if request.args.get("hub.mode") == "subscribe" and request.args.get("hub.challenge"):
        if not request.args.get("hub.verify_token") == "test_token":
            return "Verification token mismatch", 403
        return request.args["hub.challenge"], 200

    return "Hello world", 200

````



#### webhook() method

```python

@app.route('/', methods=['POST'])
def webhook():

    # Recieve your package from facebook

    data = request.get_json()
    
    # print(data)
    
    if data["object"] == "page":

        for entry in data["entry"]:
            for messaging_event in entry["messaging"]:

                if messaging_event.get("message"):  # someone sent us a message

                    sender_id = messaging_event["sender"]["id"]        # the facebook ID of the person sending you the message
                    recipient_id = messaging_event["recipient"]["id"]  # the recipient's ID, which should be your page's facebook ID
                    message_text = messaging_event["message"]["text"]  # the message's text

                    print(sender_id)
                    print(message_text)

                    # responding to your user
                    send_message(sender_id, message_text)

                if messaging_event.get("delivery"):  # delivery confirmation
                    pass

                if messaging_event.get("optin"):  # optin confirmation
                    pass

                if messaging_event.get("postback"):  # user clicked/tapped "postback" button in earlier message
                    pass

    return "ok", 200


```

#### send_message() method


```python

def send_message(recipient_id, message_text):

    # Prepare your package

    params = {
        "access_token": "EAAYwt0UP4psBAGANjPbNrkCg3WZCrz15ZAkBZB1fchNPzj3d8F4tZBs6du6kB0b5TwIZCNVUJZAuy3BnAi9fHSuJuuR2ZA0hNrx2IRmW4hH62C1ZBarQaOSMMtad8JneBf0uV3GS3rITztTk0l4NCBRr5xkeyyaEBlE6pBB3RQe1HAZDZD"
    }
    headers = {
        "Content-Type": "application/json"
    }
    data = json.dumps({
        "recipient": {
            "id": recipient_id
        },
        "message": {
        	# add the text you want to send here
            "text": message_text
        }
    })

    # Send the package to facebook with the help of a POST request
    r = requests.post("https://graph.facebook.com/v2.6/me/messages", params=params, headers=headers, data=data)

    # do this to check for errors
    if r.status_code != 200:
    	print("something went wrong")


```


### Other







