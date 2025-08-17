### **What is CSRF?**

Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform.

Client Side Request Forgery → When a client is accessing our website, we can do phishing kinda thing, by modifying html. That is known as csrf.

## Session Management :

Some websites have cookie have predictable values.

CSRF Attack  : An attack where the attacker causes the victim to do an action unintentionally while the victim is authenticated(logged-in).

For a CSRF Attack to take place: 
The request should 

1. Do an action(updating email)
2. It should be based on cookie-session handling
3. It shouldn’t have any unpredictable parameters

## Lab: CSRF vulnerability with no defenses

We login with our credentials

Then we will click update email from our account.

It’ll send request to website, we will intercept the request and regenerate csrf for that request, and put the url there as our exploit server and it’ll autosubmit. 

We will then paste the csrf body in our exploit server, which will be accessible by all users.

Once the victim clicks the url, their account will be taken over.
