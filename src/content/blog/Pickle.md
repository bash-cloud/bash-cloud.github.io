---
title: Intro to Serialization and Deserialization using Pickle
author: Bash
pubDatetime: 2022-11-10T01:00:00Z
slug: Pickle
featured: true
draft: false
tags:
  - Python
  - Insecure Deserialization Vulnerability
  - Exploits
  - DevSecOps
ogImage: ""
description: Serialization and Deserialization

---




Hello there, let’s talk about the Serialization and Deserialization of bytes streams and the security implications of data serialization. If you’re new to this you might be wondering what serialization is and why you would need it at all. To explain this let me put you in a situation where you have some data that you would like to send or store, say in a database. The data could be anything from entries to functions or classes that you want to store or transmit. The most common way of data serialization is using JSON( Javascript Object Notation) which stores the data in a human-readable format mostly encoded in UTF-8 format. JSON is the most common way of transmitting data and is applicable in many aspects including web-server communication but that’s not what we are going to discuss here today.

JSON serializes texts/data mostly in Unicode format but let’s say you want to serialize your data in binary format. One way you could do this is by using the Pickle library in python. So what is this Pickle? Simply put pickling can also be called serialization. We can therefore define it as the process where data is converted into byte streams(binary files) and unpickling is the reverse process.

You might be wondering why you’d want to use Pickle instead of the commonly used JSON serialization so let’s dive into a case scenario where you’d use Pickle and how you can practically implement it in your code.

Let’s begin. We have a website that we need to access and perform some actions on ( we will use Github.com as our lab rat). We need to access GitHub and simply print a the Github explore feature based on your interests directly from the code without having to access GitHub. This is a process we do frequently and so we want to do away with having to call the login function every time we run the script. One of the alternatives we have is to store the login object on a database and call it every time we list the explore. This way we can access our Github account for a period of time depending on how long the Session object can last and query different functions without having to log in every time. We will be making use of the Request module in python to do this. Here is a simple script that will allow us to log in to our GitHub account for every request. I am assuming that you’re device is already registered with GitHub so there won’t be a need for sending an OTP request.

```
import requestsimport jsonfrom bs4 import BeautifulSoup#write your UsernameUsername = ""#write your passwordPassword = ""session = requests.Session()def authtoken(res):soup = BeautifulSoup(str(res.content))token = soup.find('input', {'name':'authenticity_token'})['value']return tokenurl = "https://github.com/login"payload={}headers = {'Host': 'github.com','Cache-Control': 'max-age=0','Sec-Ch-Ua': '"Chromium";v="105", "Not)A;Brand";v="8"','Sec-Ch-Ua-Mobile': '?0','Sec-Ch-Ua-Platform': '"Linux"','Upgrade-Insecure-Requests': '1','User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.127 Safari/537.36','Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9','Sec-Fetch-Site': 'same-origin','Sec-Fetch-Mode': 'navigate','Sec-Fetch-User': '?1','Sec-Fetch-Dest': 'document','Referer': 'https://github.com/','Accept-Encoding': 'gzip, deflate','Accept-Language': 'en-US,en;q=0.9','Cookie': '_device_id=97dc41fc773c5fa3c337437f20c4ca7b; _gh_sess=a3GvjqjLR%2B88E610kZ4bacETE9j62iWVOE%2FRSEsS%2Bo0ozc06u8vQWwkEFfHXReZQumAnFTMexcHgS7r5tAQq38LX2Od8S96Ulsame62ko%2BnynEPS9EBjvv9Datgmksft1RnPc8TnWAIgJzNjPBioxXwlsh1AEQWMVuopjqDt9r5SemW166v%2FQWDjKWFhk6veoUnQgqh4GDWqkTbaZT4yyaajNf2KaiW2YvigP8Jpi0obCbmFR9vSZ6kVaC4HmH48C782v9pDkjostV4MB4yMNycfLfI540AeAWO%2FGV7f6uRhZx8W--oZ7guuOTFQrrJ6sg--gUiyInonv0WzpXwEgtYwNg%3D%3D; _octo=GH1.1.1281064647.1667984994; logged_in=no'}res = requests.get(url, headers=headers, data=payload)authenticity_token = authtoken(res)print(authenticity_token)def login():url = "https://github.com/session"payload = {"commit": "Sign+in","authenticity_token": authenticity_token,"login": Username,"password": Password,"webauthn-support":"supported","webauthn-iuvpaa-support":"unsupported","return_to":"https%3A%2F%2Fgithub.com%2Flogin","allow_signup": "","client_id": "","integration": "","required_field_00ab": "","timestamp": "1667990906670","timestamp_secret":"8a3c6ef0612a64a6a22d8d985d4d55bab6e779044755630b6d14f089008341fc"}headers = {'Host': 'github.com','Content-Length': '427','Cache-Control': 'max-age=0','Sec-Ch-Ua': '"Chromium";v="105", "Not)A;Brand";v="8"','Sec-Ch-Ua-Mobile': '?0','Sec-Ch-Ua-Platform': '"Linux"','Upgrade-Insecure-Requests': '1','Origin': 'https://github.com','Content-Type': 'application/x-www-form-urlencoded','User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.127 Safari/537.36','Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9','Sec-Fetch-Site': 'same-origin','Sec-Fetch-Mode': 'navigate','Sec-Fetch-User': '?1','Sec-Fetch-Dest': 'document','Referer': 'https://github.com/login','Accept-Encoding': 'gzip, deflate','Accept-Language': 'en-US,en;q=0.9','Cookie': '_device_id=97dc41fc773c5fa3c337437f20c4ca7b; _gh_sess=yHrpb0Y2N24ZvHQnMebRhT9W0Eh%2BSNjxwK5V6n5nTdJFHwyVr5hZjQRI%2B%2BuVJHf%2FwTQaqJUFPKhPKJsovCWtiUy%2B5MoKnoSu9Acsjbgt4x8%2Fc9IAewDsYMn8XqJvz1V8UrfdZVoemE9GWYZv1Rnt8h2QfKQZ302GUGUffJZ4lNzQF6rB0bwYYNj%2F5TsGCM2HbYWdOVIFan1X8ZNmDsaq6QKJE9YoAn7n5KYzZdK8kAkkikIDQusGUKca4vk1Z7nhrAN1SCHdBXd4HSCXmlLtwRYJUu5xoeEtgJUwBy6MUJMfzRQw--X1fnFCu%2BB%2Bf%2Boux%2B--Yc115xEhWODRDYN5u3tLIA%3D%3D; _octo=GH1.1.1281064647.1667984994; logged_in=no'}response = session.post(url, headers=headers, data=payload)return response# with open("github003.html", "wb") as f:#     f.write(response.content)login()def otp_request():url = "https://github.com/sessions/verified-device"payload={}headers = {'Host': 'github.com','Cache-Control': 'max-age=0','Upgrade-Insecure-Requests': '1','User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.127 Safari/537.36','Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9','Sec-Fetch-Site': 'same-origin','Sec-Fetch-Mode': 'navigate','Sec-Fetch-User': '?1','Sec-Fetch-Dest': 'document','Sec-Ch-Ua': '"Chromium";v="105", "Not)A;Brand";v="8"','Sec-Ch-Ua-Mobile': '?0','Sec-Ch-Ua-Platform': '"Linux"','Referer': 'https://github.com/sessions/verified-device','Accept-Encoding': 'gzip, deflate','Accept-Language': 'en-US,en;q=0.9','Cookie': '_device_id=97dc41fc773c5fa3c337437f20c4ca7b; _gh_sess=eLH9uF47EHuAjk7lU8cZHHzhdgS2aQ8ABABkN94lM%2Bt66ujITYwhA%2BRlloqhBi%2BqNPySM29y6suPS0%2FTi3ZSvjqQG4WI9XHN3kLmqjfA9YfyG8eZ%2BMCNLF3GfAUvE1PUIwN1QJV%2FHbHVXnk1J5PG5XUIH05XSruAUGTP3P0vJ6bo5iQKA06x9XjxnSk7QWxt6pdCdWN1j4aJxLK2ESxI6VkufpjoMPEhnXOQnxrzqKnw8%2BbNdzvDXEFCSFWHc9GElN53EiLxhQ%2BWZeEntjF%2FQXnuqkdtlDAik6aHrN%2FvsVvTBYHp--PaqNXaKt3%2Brv0ovB--%2FKBF0KdWFwC%2Fmy9QAQfshQ%3D%3D; _octo=GH1.1.1281064647.1667984994; logged_in=no'}response = session.get(url, headers=headers, data=payload)return responsedef explore():url = "https://github.com/explore"payload={}response = session.get( url, data=payload)
print(response.jsonexplore())
```
With the script above we have called the login API endpoint and accessed our account. I will write a more detailed explanation of how we can reverse-engineer different endpoints easily using python in my next writeup. Now that we have our test subject ready let us start pickling. Our goal here is to have the login function on one file that we will name github_login.pkl and have our explore function on another file first. We will then load our login module to our second script and execute it. Pickle makes this easy using the pickle.dump and pickle.load module interfaces. Let’s save our login function on a different file and open a new file called github_login.pkl then dump the function to the file.

#open a file in write binary format
with open("github_login", "wb"):
  pickle.dump(login, github_login)
  github_login.close()

You will see that a new binary file will have been created. Now let’s go back to our second function script and load our pkl file to it.

with open("github_login, "rb"):
 pickle.load(github_login)

The pickle load module reads our pickled representation of the login object and reconstructs the object hierarchy or in simple terms, it deserializes our login file.

In a practical implementation of this, you might want to use the Shelves library in python to save the files in a database and query the modules individually.

From a security perspective, the pickle module is not secure as it is possible to execute arbitrary code when constructing malicious pickle data. Arbitrary Code Execution(ACE) is a symlink injection vulnerability where malicious programs can be created without an executable file(.exe). Using pickle attackers can serialize their malicious code into byte streams and reconstruct them during unpickling. It is recommended to only pickle data that you trust or from trusted sources. I am happy to collaborate and research more on ACE vulnerability and different ways you can use the Pickle library. That’s all for today folks! Until next time

Reference:

https://docs.python.org/3/library/pickle.html#