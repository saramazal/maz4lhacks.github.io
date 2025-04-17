---
title: "A quick cheat-sheet for validating & sanitizing user inputs to prevent XSS"
author: saramazal
layout: post
date: 2025-04-17
categories: [XSS]
tags: [xss, web, pentesting, tryhackme, client-side, appsec]
img_path: /images/covers
image:
  path: /images/covers/lighthouse.png
---

### ✅ **Best Practices: Validate + Sanitize User Input**

---

### 🧼 1. **Sanitize Output (Escape Dangerous Characters)**

**In JavaScript / HTML context:**
```js
function escapeHTML(str) {
  return str.replace(/[&<>"']/g, function(match) {
    return ({
      '&': '&amp;',
      '<': '&lt;',
      '>': '&gt;',
      '"': '&quot;',
      "'": '&#x27;'
    })[match];
  });
}

// Usage
const safeInput = escapeHTML(userInput);
output.innerHTML = safeInput;
```

---

### 🧱 2. **Validate Input (Server or Client-side)**

#### ✅ Example: Only allow alphanumeric search query
```js
const query = location.search;
if (/^[a-zA-Z0-9\s]+$/.test(query)) {
  // Accept input
} else {
  alert("Invalid characters detected!");
}
```

---

### 🔒 3. **Use `textContent` Instead of `innerHTML`**
```js
output.textContent = userInput; // Safe
// ❌ output.innerHTML = userInput; // Risky
```

---

### 🔧 4. **Server-Side Input Filtering (PHP Example)**
```php
// Validate expected input
if (preg_match('/^[a-zA-Z0-9_ ]+$/', $_GET['q'])) {
    echo htmlspecialchars($_GET['q']);
} else {
    echo "Invalid input";
}
```

---

### 🛡️ 5. **Framework Helpers (if you're using them)**

- **React**: Avoid using `dangerouslySetInnerHTML`
- **Express (Node.js)**: Use `helmet` middleware
- **PHP**: Use `htmlspecialchars()`

```php
echo htmlspecialchars($input, ENT_QUOTES, 'UTF-8');
```

---

### 🧠 TL;DR

| Task | Use |
|------|-----|
| Escape HTML | `& < > " ' → &amp; &lt; ...` |
| Sanitize JS | Never inject raw user input into DOM |
| Validate Input | RegEx or whitelist expected formats |
| Encode Output | `textContent`, `htmlspecialchars()` |

---
