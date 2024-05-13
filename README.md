# How-to-solve-Cloudflare-Turnstile-Captcha-with-Python
How to solve Cloudflare Turnstile Captcha with Python



## ⚙️ Prerequisites
- Python installed
- Capsolver API key

## 🤖 Step 1: Install Necessary Packages
Execute the following commands to install the required packages:

```python
pip install requests
```

## 👨‍💻 Step 2: Python Code for solve Cloudflare Turnstile Captcha

Here's a Python sample script to accomplish the task:

```python
import time
import requests

CAPSOLVER_API_KEY = "api key"
PAGE_URL = "url"
WEBSITE_KEY = "site key"

def solvecf(metadata_action=None, metadata_cdata=None):
    url = "https://api.capsolver.com/createTask"
    task = {
        "type": "AntiTurnstileTaskProxyLess",
        "websiteURL": PAGE_URL,
        "websiteKey": WEBSITE_KEY,
    }
    if metadata_action or metadata_cdata:
        task["metadata"] = {}
        if metadata_action:
            task["metadata"]["action"] = metadata_action
        if metadata_cdata:
            task["metadata"]["cdata"] = metadata_cdata
    data = {
        "clientKey": CAPSOLVER_API_KEY,
        "task": task
    }
    response_data = requests.post(url, json=data).json()
    print(response_data)
    return response_data['taskId']


def solutionGet(taskId):
    url = "https://api.capsolver.com/getTaskResult"
    status = ""
    while status != "ready":
        data = {"clientKey": CAPSOLVER_API_KEY, "taskId": taskId}
        response_data = requests.post(url, json=data).json()
        print(response_data)
        status = response_data.get('status', '')
        print(status)
        if status == "ready":
            return response_data['solution']

        time.sleep(2)


def main():
    
    taskId = solvecf()
    solution = solutionGet(taskId)
    if solution:
        user_agent = solution['userAgent']
        token = solution['token']

    print("User_Agent:", user_agent)
    print("Solved Turnstile Captcha, token:", token)

  
if __name__ == "__main__":
    main()


```

## ⚠️ Change these variables
- **CAPSOLVER_API_KEY**: Obtain your API key from the [Capsolver Dashboard](https://capsolver.com/dashboard).
- **PAGE_URL**: Replace with the URL of the website for which you wish to solve the CloudFlare Turnstile Captcha.
- **WEBSITE_KEY**: Replace with the site key of the website 

### What the CloudFlare Turnstile Captcha Looks Like
![Cloudflare Turnstile Captcha](https://assets.capsolver.com/prod/images/post/2024-05-13/aafc0187-0920-4e61-8000-5347209fb122.png)


Meanwhile, if you'd like to test your scripts for bot characteristics, BrowserScan's [Bot Detection](https://www.browserscan.net/bot-detection) tool can help you identify and refine bot-like behavior in your scripts.
