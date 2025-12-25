# XSS - Merry XSSMas
Learn about types of XSS vulnerabilities and how to prevent them.

## Learning Objectives
- Understand how XSS works
- Learn to prevent XSS attacks

**XSS:** A type of security vulnerability typically found in web applications. It allows attackers to inject malicious scripts into web pages viewed by other users. These scripts can then steal sensitive information, like user's cookies, session tokens, or other sensitive data.

---

**HTTP:** Hypertext Transfer Protocol (HTTP) is the protocol that specifies how a web browser and a web server communicate. Your web browser requests content from the TryHackMe web server using the HTTP protocol as you go through this room.

## 🔍 What is XSS?

Cross-Site Scripting (XSS) is a web application vulnerability that allows attackers to inject malicious code—usually JavaScript—into a trusted website.

This happens when:
- User input is not properly validated or sanitized
- Input is reflected or stored and later rendered as executable code

Once triggered, XSS can allow attackers to:
- Steal session cookies
- Impersonate users
- Deface websites
- Perform actions as the victim

---

## 🧪 Types of XSS

### 1️⃣ Reflected XSS

Reflected XSS occurs when injected input is immediately reflected back in the server’s response.

#### Example
Legitimate search request:
```https://trygiftme.thm/search?term=gift```


Malicious request:
```html
https://trygiftme.thm/search?term=<script>alert(atob("VEhNe0V2aWxfQnVubnl9"))</script>
```

If a victim clicks this link:
- The browser executes the injected JavaScript
- The attack runs instantly

**Characteristics**
- Not stored on the server
- Requires victim interaction (usually phishing)
- Executes once per click

### 2️⃣ Stored XSS

Stored XSS occurs when malicious code is saved on the server and executed every time a user loads the affected page.

#### Example
Normal Comment Submission:
```http
POST /post/comment HTTP/1.1
Host: tgm.review-your-gifts.thm

postId=3
name=Tony
comment=Great gift!
```

Malicious Comment Submission:
```html
<script>alert(atob("VEhNe0V2aWxfU3RvcmVkX0VnZ30="))</script>
```

Because the payload is stored in the database:
- Every visitor triggers the script
- No user interaction is required

**Characteristics**
- Persistent
- Affects multiple users
- Higher impact than reflected XSS

---

## Impact of XSS Attacks

When an XSS vulnerability is successfully exploited, the attacker can execute malicious code **as if they were the victim user**. This allows them to perform a wide range of harmful actions, including:

- **Stealing session cookies**, which can lead to account takeover  
- **Triggering fake login pop-ups** to harvest credentials  
- **Defacing web pages** by injecting unwanted or malicious content  

Because the code runs in the victim’s browser, the attacker gains the same permissions as that user within the application.

---

## Protecting Against XSS

Preventing XSS requires careful design and secure handling of user input. While each application may differ, the following best practices significantly reduce XSS risk:

### 1. Avoid Dangerous Rendering Methods
- Do **not** use `innerHTML`, as it directly injects content into the DOM and allows script execution.
- Use safer alternatives like `textContent`, which treats input strictly as text instead of executable HTML.

### 2. Protect Cookies from JavaScript Access
- Set session cookies with the following attributes:
  - `HttpOnly`
  - `Secure`
  - `SameSite`
- These flags prevent JavaScript from accessing cookies and limit damage if XSS occurs.

### 3. Sanitize and Encode User Input & Output
- Always sanitize and encode **all user-supplied input** before storing or displaying it.
- Remove or escape:
  - `<script>` tags  
  - Event handlers (e.g., `onload`, `onclick`)  
  - JavaScript URLs  
- If limited HTML is allowed (e.g., links or formatting), ensure only safe tags and attributes are permitted.

---

## Exploiting XSS (Why Input Fields Matter)

To exploit an XSS vulnerability, attackers need a place to inject malicious code. Common targets include:

- Search bars  
- Comment sections  
- Message forms  

In this case, the **search section** reflects user input directly, making it a good starting point for testing reflected XSS vulnerabilities.

---

# Exploiting Reflected XSS

Reflected XSS happens when a website immediately shows back what you typed without checking or cleaning it.

## How it works
1. You enter some input (for example, in a search box).
2. The website displays that input directly in the response.
3. If the input contains JavaScript, the browser runs it as code.

## Example
If you type this into a search field:
```html
<script>alert('Reflected Meow Meow')</script>
```

And an alert pops up, it means:
- The website did **not sanitize** your input
- Your script was **reflected back** and executed by the browser

## Why it’s dangerous
- Attackers can send malicious links to users
- When the victim clicks the link, the attacker’s script runs
- This can steal cookies, tokens, or perform actions as the victim

> Reflected XSS usually affects **one user at a time** and requires them to click a crafted link.

---

# Exploiting Stored XSS

Stored XSS is more dangerous because the malicious code is **saved on the server**.

## How it works
1. You submit malicious input (for example, in a comment or message).
2. The website stores this input in its database.
3. Every user who views that page runs the malicious script automatically.

## Example
If you submit this as a message:
```html
<script>alert('Stored Meow Meow')</script>
```

And the alert shows up **every time the page loads**, it means:
- The input was saved without validation
- The script is executed for **all users**, not just you

## Why it’s dangerous
- It’s a set-and-forget attack
- Every visitor becomes a victim
- Attackers can:
 - Steal session cookies
 - Show fake login prompts
 - Perform actions as logged-in users

> Stored XSS affects **many users** and does **not require clicking a link** after the payload is stored.

---

# Quick Comparison

| Feature         | Reflected XSS             | Stored XSS             |
| --------------- | ------------------------- | ---------------------- |
| Payload storage | Not stored                | Stored in database     |
| Victims         | One user at a time        | All users viewing page |
| Trigger         | Clicking a malicious link | Loading the page       |
| Severity        | Medium                    | High                   |

---

## Key Takeaway

- Reflected XSS = input comes back immediately and executes
- Stored XSS = input is saved and executes for everyone

Both happen because the application trusts user input too much and fails to sanitize or encode it properly.

---

# Answer the questions below

**Which type of XSS attack requires payloads to be persisted on the backend?**
`Stored`

