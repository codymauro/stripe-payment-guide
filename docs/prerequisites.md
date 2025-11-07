---
hide:
  - toc
---
Before running the code, make sure you have these basics ready:  
  
**Testing tools**  
  - A web browser to test your endpoints.
  
**Stripe account:**  
  - Sign up at [dashboard.stripe.com/register](https://dashboard.stripe.com/register).  
  - Place your account in Sandbox mode for testing so that no real money moves.
 
**Code editor and terminal:**  
  - Use a code editor like [VS Code](https://vscode.dev/). Rich-text editors (e.g., TextEdit) can add hidden     formatting that breaks HTML and Python.  
  - You’ll run commands in your **system terminal**.
 
**Python installed:**   
  - Python 3.6+ (check in terminal with `python3 --version`)  
  - Or [install Python](https://www.python.org/).
     
**Test card numbers:**   
  - For testing successful payments → 4242 4242 4242 4242  
  - For testing declined payments → 4000 0000 0000 0002  
  - Use any future expiry date and any 3-digit CVC.  
### Redirect Pages 
After checkout, Stripe automatically redirects users to one of two pages:  
 
  - **Success URL** — Payment approved. This page shows something like, “Thanks for your payment!” and optionally retrieves session info to confirm amount or status.   
  - **Cancel URL** — Payment canceled or abandoned. This page might say, “Payment canceled. Try again?”
  
For local testing, default to the following URLs:   
```
https://127.0.0.1:8443/success
https://127.0.0.1:8443/cancel
```
These URLs are already built into the example backend code in the next section. In the future, when you want to deploy your code to the web, you’ll switch those local host URLs to your fully developed pages, like:
```
https://your-site.com/success
https://your-site.com/cancel
```
Once the above tools are installed, you’re ready to build your local server and connect to Stripe Checkout in the next step.
  
  > Note: **When Going Live**,, don’t confirm payments from the `/success` URL page. Use Stripe webhooks, which let Stripe automatically notify your backend when a payment succeeds. That way, your app can safely fulfill orders even if the customer never returns to your site. Though not mandatory, Stripe highly recommends them. [Learn more here](https://docs.stripe.com/webhooks).
