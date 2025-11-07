---
hide:
  - toc
---
This guide helps business owners, new developers, designers, and writers get comfortable integrating **Stripe Checkout** into their applications.  

You’ll build a simple, one-time $10 payment flow that works end-to-end using Stripe’s prebuilt checkout page.  

**The goal:** Understand how a secure payment moves from button-click to confirmation, without getting lost in setup, full-stack complexity, or Stripe’s 100-plus-page documentation.  
### Stripe Checkout Overview
Stripe Checkout is a hosted payment page that handles card details, validation, and security so you don’t have to manage sensitive data on your own servers. We’ll use Stripe libraries/SDKs for this. The **Software Development Kit (SDK)** gives us pre-written code that makes it easier to connect to Stripe’s API without starting from scratch. That way, we can:

1. Create a checkout session
2. Redirect users to pay
3. Handle the response.

**Total on-hands time**: 5-15 minutes for the basic flow, depending on your comfort with python and terminals.

![Image of someone making a purchase through Stripe’s phone app](images/stripe-transaction.jpg)

### Additional Details
This guide uses **Python** (Flask) for backend examples. If you prefer working in **JavaScript** (Node/Express), you can follow the companion guide here (coming soon).

This is **not** a guide for setting up or building a business profile on Stripe. By the end of this guide, you'll have an understanding of how to start accepting payments. 

All examples use Stripe’s Sandbox, or testing, mode. No real money changes hands. This keeps things safe while you learn and test. Check the Prerequisites to set up.
