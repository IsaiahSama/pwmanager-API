# Documented below is the usage for the API LAPasswordManager.

## Start Note:
LAPasswordManager (Look Another Password Manager) is an API and webapp, which functions as a place where you can store and get your passwords as you desire. 
This webapp was created to be an extension of https://github.com/IsaiahSama/Password_Manager (Another Password Manager [APM] of mine), and therefore, APM has been rewritten to provide a direct way to access and use this API. However, the documentation below is for those that wish to access the API using their own way.

## About the API:
This API allows you to create, get, update, delete, store and generate passwords. When making a request through the API, you will be sent an email containing a key, that will allow you to interact with the API for 5 hours. This is to ensure your safety and privacy when trusting us to keep your passwords secure.

## Before Using the API:

In order to use this API, you will need to have an account already created. To do this, you may visit https://lapasswordmanager.herokuapp.com. After creating an account, that's all you need before using the API. 

When using the API for the first time in a while (About 5 hours), an email will be sent to you to verify that it is you who is making the request. After, all subsequent requests for 5 hours will no longer request that verification key.

The base URL for all requests is ```https://www.lapasswordmanager.herokuapp.com/api/v1/```. For the following documentation of this API, this base URL will be referred to as the constant `BASE`.

## Note:
For all API requests, valid JSON is expected. And in return, valid JSON Will be returned.

All responses will ALWAYS contain at least the following:
A successful response will be in the form of:
```json
{
    "LAPW": {
        "SUCCESS_OR_FAILURE": {
            "SUCCESS": true,
            "MESSAGE": "A success message"
        }
    }
}
```
Where MESSAGE is a success message containing information

Whereas, if an error has occurred, the following JSON will be returned
```json
{
    "LAPW":{
        "SUCCESS_OR_FAILURE":{
            "SUCCESS": false,
            "MESSAGE": "An error message"
        }
    }
}
```
Where MESSAGE is the error that occurred.

In case of a success, the other data that was requested, will be also be provided in a "DATA" entry.


## Activating your API
As mentioned twice before now, you have to activate your session before any of your requests can be made. An activated session lasts for 5 hours.
The URL for this is:
```py
BASE + "activate"
```

To activate a session, the following are required. These are: The registered email and the password for that email. 
Example:
```py
import requests
import json

BASE = "https://www.lapasswordmanager.herokuapp.com/api/v1/"

# LAPW (Look Another Password Manager), Should be the outermost of the JSON, and everything else comes as a value to this key
to_post = {
    "LAPW": {
        "AUTH": {
            "EMAIL": "someone@example.com",
            "PASSWORD": "securepassword
        }
    }
}


url = BASE + "activate"
# Therefore: The URL is https://www.lapasswordmanager.herokuap.com/api/v1/activate

x = requests.post(url, json=to_post)
# Checks to see that the connection was made successfully
x.raise_for_status()

# To get the response data, you can do
# response = x.json()
# Or if you want it directly as a python dictionary for you to use, you can do
response = loads(x.json())
# You can then see if the operation was a success by doing
# Checks if the operation was a success
if response["LAPW"]["SUCCESS_OR_FAILURE"]["SUCCESS"] == True:
    # Prints out the success statement
    print("All is good")
else:
    print("Something went wrong")

# Prints out the message
print(response["LAPW"]["SUCCESS_OR_FAILURE"]["MESSAGE"])
```

Returns only the basic `SUCCESS_OR_FAILURE` as a response as shown above


After a successful response, a verification email will be sent to the given email address, containing the verification code to be sent in the following request.

In order to verify now, and have your account available for you to use. The procedure is almost the exact same.
Example:

```py
import requests
import json

BASE = "https://www.lapasswordmanager.herokuapp.com/api/v1/"

# LAPW (Look Another Password Manager), Should be the outermost of the JSON, and everything else comes as a value to this key
to_post = {
    "LAPW": {
        "AUTH": {
            "EMAIL": "someone@example.com",
            "PASSWORD": "securepassword",
            "VERICODE": "insert verification code here"
        }
    }
}

# Therefore: The URL is https://www.lapasswordmanager.herokuap.com/api/v1/activate
url = BASE + "activate"

x = requests.post(url, json=to_post)
# Checks to see that the connection was made successfully
x.raise_for_status()

# To get the response data, you can do
response = x.json()
# Or if you want it directly as a python dictionary for you to use, you can do
response = loads(x.json())
```
Where VERICODE is the code received from the email.

Returns only the basic `SUCCESS_OR_FAILURE` as a response as shown above.
