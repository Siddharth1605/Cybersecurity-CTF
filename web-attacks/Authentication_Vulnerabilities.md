## **What is authentication?**

Authentication is the process of verifying the identity of a user or client.

Authentication is the process of checking you are the user you said to be. Authorization is the process of validating you have access for the process.

Poor code or logic makes an attacker to bypass authentication mechanism → “Broken Authentication”

High-privileged user access can’t be exploited in public websites. But once you have low-user acc for the website, which normal people shouldn’t have, then you able to do privilege-escalation on your access.

TIP : Occasionally, responses contain email addresses of high-privileged users, such as administrators or IT support.

If a login page asks for username and password, first we can do username bruteforce by using burp-intruder. Check each request, and response for each username that is bruteforced. Response, and length may vary for proper username when compared to other response
