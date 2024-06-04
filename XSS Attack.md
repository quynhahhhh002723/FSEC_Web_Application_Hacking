# I. What is Cross-site Scripting (XSS)

**Cross-site Scripting (XSS)** is a **client-side code injection attack**, which is one of the most common attacks. The web vulnerability allows attackers to interfere with the interactions between users and an application. An XSS attack involves `injecting malicious code`; to exploit an XSS vulnerability, a hacker will insert `malicious scripts` to execute them on the client side. 

**When this malicious code spreads through the web, the hacker can gain control.** Cross-site scripting vulnerabilities often allow attackers to **impersonate the victim**, **perform any actions** that the user can do, and **access any of the user's data**.

For more details on the different types of XSS flaws, see: [What are the types of XSS Attack](#).
# II. What are the types of XSS Attack

There are three main types of Cross Site Scripting attacks:

**Reflected XSS:** where the malicious script comes from the current HTTP request. This type of XSS is often found in search engines or error messages where user input is reflected back in the web page. Attackers typically craft a special URL and trick users into clicking it, causing the malicious script to be executed in their browsers.

Example:

```html
https://insecure-website.com/status?message=All+is+great.

<p>Status: All is great.</p>
  ```
Because the application does not process the data in any way, the attacker can easily launch a script-based attack like the one below on other users.
```html
https://insecure-website.com/status?message=<script>/*+Malicious+code +here...+*/</script>

<p>Status: <script>/* Malicious code here... */</script></p>
```

When the user accesses the URL constructed by the attacker, the attacker's script runs within the user's browser, in the context of that user's session with the application. During this process, the script gains the ability to perform any action and access any data that the user is authorized to interact with.

**Stored XSS** (also known as **persistent** or **second-order XSS**): where the **malicious script** comes from the website's **database**. This type of **XSS** occurs when **user input** is stored on the **server** (in a **database**, for instance) and then displayed to other users without proper **sanitation**. It is especially dangerous because it can affect many users at once if they view the infected page or content. In a **Stored Cross Site Script attack**, the **injected script** is saved permanently on the **target servers**, such as in a **database**, displaying **social media posts**, **comment**, a **network monitoring application** displaying **packet data** from **network traffic** or other location.



