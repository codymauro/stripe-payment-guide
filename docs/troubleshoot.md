---
hide:
  - toc
---
| **Error or Symptom** | **What It Means** | **Quick Fix** |
|-----------------------|------------------|----------------|
| `zsh: command not found: python` | macOS no longer ships Python 2; your system doesn't recognize the `python` command. | Run `python3 app.py` instead. |
| `ModuleNotFoundError: No module named 'flask'` | Flask isn't installed in your current environment. | Run `python3 -m pip install flask stripe`. |
| `Invalid API Key provided` | You're using the wrong or empty API key. | Copy your **test** keys again from Stripe Dashboard (`sk_test_...` and `pk_test_...`). |
| `Address already in use` | The port (5000 or 3000) is still occupied by a previous server run. | Press `Ctrl + C` in that terminal to stop the old server, then restart. |
|Raw HTML code displays in browser instead of rendered page (e.g., seeing  as text) | The index.html file was likely saved in rich text format (RTF) by a default TextEdit, which adds extra markup and escapes your actual HTML, so the browser treats it as plain text paragraphs rather than a web page structure. | Delete the file and recreate it in a plain text editor like VS Code. | 403 Access Denied error on redirect after payment (e.g., “You don’t have authorization to view this page”) | Browser security blocks the redirect because Stripe’s page uses secure HTTPS (encrypted connection), but your local server defaults to HTTP (unencrypted), creating a protocol mismatch that prevents the jump back to localhost. | Make sure your app.py code and server URLs start HTTPS |
| Browser shows 'Network Error' or 'Failed to fetch' | The frontend can't reach the backend (server not running or wrong URL). | Make sure the terminal shows 'Running on http://localhost:PORT.' Visit that exact URL — not the HTML file directly. |
| Nothing happens when clicking 'Pay $10' | JavaScript failed or server not running. | Check that the backend is running and that your HTML uses **straight quotes** (not curly) around fetch URLs. |
| `SyntaxError: invalid character '''` | Smart quotes from copy-paste are breaking code. | Replace all curly quotes with straight quotes `' '` or `' '`. |
| `400 Bad Request` or `403 Forbidden` | The server made a request Stripe didn't accept (usually a bad key or malformed JSON). | Verify API keys and copy code exactly as shown. |
| 'Payment canceled. Try again?' appears unexpectedly | You used a declined test card or canceled mid-checkout. | Retry with `4242 4242 4242 4242`. |
| No verification message prints in terminal | Your `success_url` is missing `?session_id={CHECKOUT_SESSION_ID}` or your retrieve call is commented out. | Double-check your `success_url` line and success route code. |

### Pro Tip
If you ever get stuck, restart your local server:
```bash
Ctrl + C
python3 app.py
```
Then refresh your browser at `http://localhost:PORT`.

For additional error codes and how to resolve them, visit [Stripe's Developer Resources page](https://docs.stripe.com/error-codes)