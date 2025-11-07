---
hide:
  - toc
---
Follow these steps to set up and test a $10 one-time payment flow. 
### 1. Get your Stripe keys 
 
- In the Stripe Dashboard → `Developers` → `API keys` →`Standard keys`.
	- Copy your **Publishable key** (pk_test_...) and **Secret key** (sk_test_...).
- Or, see your keys in the Recommendations box.
- Slot these keys into their homes in the `app.py` code.

![Stripe’s testing dashboard shares APIs on the main page or via menu navigation](images/get-api.png)
 
> Keep these keys private. Test keys are fine to paste directly into your code while learning and testing. When you publish real code online, use environment variables instead of hardcoding keys to keep it secret.
### 2. Install Dependencies (Stripe + Flask)
Install the required packages using your terminal.

- **Stripe’s SDK**: `python3 -m pip install stripe flask`. 
- **Python cryptography**: `python3 -m pip install cryptography`. This allows your script to generate https pages. 
 
### 3. Create a project folder
Create a new desktop folder called `stripe-test` to organize your files. Inside the test folder, create a second folder named `templates`.
### 4. Generate backend server code
This backend code runs your local server. It listens for requests from your browser (frontend), connects to Stripe’s API, and returns a checkout link.  
 
- Open [VS Code](https://vscode.dev/) and save the following code as `app.py` in your `stripe-test` folder.
   
**Python (Flask):**
```
import stripe
from flask import Flask, jsonify, request, render_template

app = Flask(__name__, template_folder="templates")

# Replace these with your own test keys from Stripe dashboard 
stripe.api_key = "sk_test_yourSecretKeyHere"
PUBLISHABLE_KEY = "pk_test_yourPublishableKeyHere"

@app.route("/")
def index():
    # Serve the HTML page with the Pay button
    return render_template("index.html", pk=PUBLISHABLE_KEY)

@app.post("/create-checkout-session")
def create_checkout_session():
    try:
        session = stripe.checkout.Session.create(
            mode="payment",
            line_items=[{
                "price_data": {
                    "currency": "usd",
                    "product_data": {"name": "Test Payment"},
                    "unit_amount": 1000,  # $10 in cents
                },
                "quantity": 1,
            }],
            payment_method_types=["card"],
            success_url="https://127.0.0.1:8443/success?session_id={CHECKOUT_SESSION_ID}",
            cancel_url="https://127.0.0.1:8443/cancel",
        )
        # Return the Checkout URL to redirect the user
        return jsonify({"url": session.url})
    except Exception as e:
        return jsonify({"error": str(e)}), 500

@app.get("/success")
def success():
    session_id = request.args.get("session_id")
    if session_id:
        try:
            session = stripe.checkout.Session.retrieve(session_id)
            print(f"Verified session: amount={session.amount_total} cents, status={session.payment_status}")
        except Exception as e:
            print(f"Error verifying session: {e}")
    return "Thanks for your payment! (Test mode)"

@app.get("/cancel")
def cancel():
    return "Payment canceled. Try again?"

if __name__ == "__main__":
    app.run(port=8443, debug=True, ssl_context='adhoc')
```
Remember to replace the in-line placeholder API keys with those you obtained in Step 1. If you have specific **Success** or **Cancel URLs** you’d like to redirect to, substitute them for the current `127.0.0.1:8443` placeholders.
### 5. Generate frontend HTML
Your frontend is the part the user interacts with, AKA the website itself. Because we’re running a test, we’re creating a simple, stand-in HTML site with a "Pay $10" button. 

- Open [VS Code](https://vscode.dev/) and create a new document titled `index.html` inside your `templates` folder. 
- Paste the following HTML into the VS Code interface:
 
```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Stripe Test</title>
  <script src="https://js.stripe.com/v3/"></script>
  <style>
    body { font-family: system-ui, sans-serif; padding: 2rem; }
    button { padding: 0.6rem 1rem; font-size: 1rem; cursor: pointer; }
  </style>
</head>
<body>
  <h1>Pay $10 (Test Mode)</h1>
  <button id="checkout-button">Pay $10</button>
  <script>
    document.getElementById("checkout-button").addEventListener("click", () => {
      fetch("/create-checkout-session", { method: "POST" })
        .then(res => res.json())
        .then(data => data.url ? (window.location = data.url) : alert("Error: " + (data.error || "Unknown")))
        .catch(err => alert("Network error: " + err.message));
    });
  </script>
</body>
</html>
```
### How It All Fits Together
The **frontend** is the website. It’s the page a customer would visit on your store, like a product page with a "Pay $10" button. The **backend** is the behind-the-scenes code — your **Flask** or **Express** server that runs silently in the background, handling what happens when someone clicks the "Pay $10" button. 
 
Here’s a flow chart of the secure payment system:

```
Customer Browser (index.html)
        |
        | 1) Click “Pay $10”
        v
Your Backend (app.py, secret key)
        |
        | 2) Create Checkout Session via Stripe API
        v
Stripe Checkout (hosted page)
        |
        | 3) Customer pays (test card)
        v
Stripe -> Your Site
		|
		| 4) Stripe redirects to success_url or cancel_url
		|
```
Those are the basics. Now, you’re ready to launch your local server, test your API keys, and make a transaction. 
