# How To Build A Chatbot
Learn to build a facebook chatbot using Python and Flask


### Create a Facebook Page and App

### Setup the Software

#### Prereq: Do you have python installed on your lapt

#### 1. Setup a Virtual Environment for this project

#### 2. Download Project Files

#### 3. Install Dependencies

###  Start Coding!

#### verify method
```python

def verify():

    if request.args.get("hub.mode") == "subscribe" and request.args.get("hub.challenge"):
        if not request.args.get("hub.verify_token") == "test_token":
            return "Verification token mismatch", 403
        return request.args["hub.challenge"], 200

    return "Hello world", 200

````

#### webhook method

#### send_message method












