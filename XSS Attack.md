## Table of Contents

- [I. What is Cross-site Scripting (XSS)](#i-what-is-cross-site-scripting-xss)
- [II. What are the types of XSS Attack](#ii-what-are-the-types-of-xss-attack)
  - [Reflected XSS](#reflected-xss)
  - [Stored XSS](#stored-xss)
  - [DOM-based XSS](#dom-based-xss)
- [III. How Does Cross-Site Scripting Work?](#iii-how-does-cross-site-scripting-work)
  - [Different cross site scripting approaches](#different-cross-site-scripting-approaches)
- [IV. What Are the Impacts of XSS?](#iv-what-are-the-impacts-of-xss)
- [V. How to prevent XSS](#v-how-to-prevent-xss)
  - [Input Validation](#input-validation)
  - [Output Encoding](#output-encoding)
  - [Content Security Policy (CSP)](#content-security-policy-csp)
  - [HTTP Headers](#http-headers)
  - [Libraries and Tools](#libraries-and-tools)
  - [Regular Security Audits](#regular-security-audits)
  - [Here are some examples](#here-are-some-examples)



### I. What is Cross-site Scripting (XSS)

**Cross-site Scripting (XSS)** is a **client-side code injection attack**, which is one of the most common attacks. The web vulnerability allows attackers to interfere with the interactions between users and an application. An XSS attack involves `injecting malicious code`; to exploit an XSS vulnerability, a hacker will insert `malicious scripts` to execute them on the client side. 

**When this malicious code spreads through the web, the hacker can gain control.** Cross-site scripting vulnerabilities often allow attackers to **impersonate the victim**, **perform any actions** that the user can do, and **access any of the user's data**.

For more details on the different types of XSS flaws, see: [What are the types of XSS Attack](#ii-what-are-the-types-of-xss-attack).
### II. What are the types of XSS Attack

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

For instance:
```html
<p>Hi, this is my comment!</p>
```
If the site doesn’t process the data submitted, an attacker can easily input    content that includes a malicious script that will infect other users. The victim retrieves the malicious script from the server when it requests the stored information.
```html
<p><script>/* Malicious code here... */</script></p>
```
**DOM-based XSS** (also known as **persistent XSS attacks**): where the **vulnerability** exists in **client-side code** rather than **server-side code**. This type of **XSS** happens when the web application's **client-side scripts manipulate the DOM** and introduce **unsafe data**. Since the **server** is not involved, traditional **server-side defenses** might not prevent this **attack**. **DOM XSS** is a relatively uncommon **cross site script** type of **attack**. An **attack** of this nature occurs when a **web application processes JavaScript data** from an **untrusted source** in an **unsafe way**. **DOM XSS attacks** always happen in **JavaScript** because **Java** is the only **language** that all **browsers understand**.

In the following example:

An application uses JavaScript to read the values from an input field. Then, it writes those values within an element of the HTML.

```javascript
var search = document.getElementById('search').value;
var results = document.getElementById('results');
results.innerHTML = 'You searched for: ' + search;
```
When attackers control the value of the input field, they can very easily insert the malicious code that triggers their script.
```javascript
You searched for: <img src=1 onerror='/* Bad stuff here... */'>

```

### III. How Does Cross-Site Scripting Work?

**Cross-site scripting** works by manipulating a **vulnerable web site** so that it returns **malicious JavaScript** to users. When the **malicious code executes** inside a **victim's browser**, the **attacker gains the ability to completely compromise the user's engagement with the application**.

It is important to note that **XSS attack’s mechanics** will vary based on the **type of attack being deployed**. So, **most attacks follow the same process**:
1. **The attacker identifies** a **place and method** for which to inject **malicious code into a web page**. For this to be possible, the website must allow users to add content to the page through **comments**, **posts**, or **contact fields**. If the attacker has a defined target, they will employ **social engineering tactics**, including **phishing** and **spoofing techniques** to encourage the user to visit the site in question. Otherwise, the code is left for any user to discover.

2. **The victim visits the website** with the injected code. Their device will accept and execute the **infected script** because it considers it part of the **source code** from a trusted site. Given that the code is not visible and most internet users do not understand common programming languages, such as **JavaScript**, it is difficult for the average user to detect **XSS attacks**.
Different **cross site scripting approaches**

An **XSS attack** can occur any place where input from an **HTTP request** could make its way into the **HTML output**. Below is a list of common tactics that attackers may leverage in an **XSS attack**:

- Utilize a ```<script>``` tag to reference, insert, or embed **malicious JavaScript code**.
- Exploit **JavaScript event attributes**, such as ```onload``` and ```onerror```, within different tags.
- Deliver an **XSS payload** within the ```<body>``` tag through **JavaScript event attributes**.
- Exploit unsecured ```<img>```, ```<link>```, ```<div>```, ```<table>```, ```<td>``` or ```<object>``` tags to reference **malicious script**.
- Leverage the ```<iframe>``` tag to embed a webpage within the existing page.
### IV. What Are the Impacts of XSS?

**XSS attacks** can result in significant issues for victims. In extreme cases, **XSS attackers** can leverage user cookies to masquerade as that person. The code can also steal files and data or install malware on the device.

On the **server-side**, **XSS attacks** can result in **reputational harm** to the host organization. For example, by changing the content on a **corporate site**, attackers can spread **misinformation** about the company’s business practices or activities. The adversary can also manipulate **website content** to provide **incorrect instructions** or **directions** to visitors. Becoming compromised in this way is especially dangerous if hackers can overtake **government websites** or resources during emergency events, ultimately misdirecting people on how and where to proceed in times of crisis.

Unfortunately, **XSS flaws** can be challenging to identify, especially if the user lacks computer programming knowledge. Even skilled developers rarely check code from trusted sites. Once injected, it is often very challenging to remove the **malicious code** from the application for **Stored XSS attacks**.

### V. How to prevent XSS

It’s crucial to ensure your organization is not vulnerable to XSS attacks. Script-based and other fileless attacks have increased in recent years because they can avoid detection by new and old security tools, including antivirus software and firewalls.

Preventing XSS involves multiple layers of defense, including input validation, output encoding, and secure coding practices. Here’s a comprehensive guide to preventing XSS:

**Input Validation**
- **Sanitize Inputs:** Remove any unwanted characters from user inputs. Use whitelisting to allow only safe characters.
- **Validate Inputs:** Ensure inputs conform to expected formats, lengths, and types.

**Output Encoding**
- **HTML Encoding:** Convert characters like `<, >`, `&,` and `"` to their HTML entity   equivalents(&lt;, &gt;, &amp;, &quot;). 
- **JavaScript Encoding:** Encode data before inserting it into JavaScript contexts.
- **CSS Encoding:** Encode data before using it in CSS contexts.
- **URL Encoding:** Encode URL parameters to prevent injection attacks.

**Content Security Policy (CSP)**
- **Implement CSP:** Use a Content Security Policy to restrict the sources from which content can be loaded. This helps mitigate the risk of XSS by blocking inline scripts and only  allowing resources from trusted domains.

**HTTP Headers**
- **Set Secure Headers:** Use HTTP headers like `X-Content-Type-Options: nosniff`,   `X-Frame-Options: DENY`, and `X-XSS-Protection: 1; mode=block` to enhance security.

**Libraries and Tools**
- **Use Security Libraries:** Leverage security libraries such as DOMPurify for sanitizing HTML  and preventing XSS.

**Regular Security Audits**
- **Conduct Code Reviews:** Regularly review your code for potential XSS vulnerabilities.
- **Penetration Testing:** Perform regular security testing to identify and mitigate vulnerabilities.

Here are some examples of how to implement these techniques in different contexts:
HTML encoding:
```javascript
function htmlEncode(value){
  return $('<div/>').text(value).html();
}
```
JavaScript encoding:
```javascript
function jsEncode(value) {
  return JSON.stringify(value).slice(1, -1);
}
```


