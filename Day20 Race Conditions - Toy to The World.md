# Race Conditions - Toy to The World
Learn how to exploit a race condition attack to oversell the limited-edition SleighToy.

## Learning Objectives
- Understand what race conditions are and how they can affect web applications.
- Learn how to identify and exploit race conditions in web requests.
- How concurrent requests can manipulate stock or transaction values.
- Explore simple mitigation techniques to prevent race condition vulnerabilities.

---

# Race Condition

A **race condition** occurs when:
- Two or more actions happen **at the same time**
- The system’s result depends on **which action finishes first**
- Proper synchronization is **missing**

In web apps, this often affects:
- Inventory systems
- Checkout/payment flows
- Account balances

## Types of Race Conditions

Generally, race condition attacks can be divided into **three** categories:

**1. Time-of-Check to Time-of-Use (TOCTOU)**
- The system checks a condition first (e.g., stock available)
- The data changes before it is used
- Result: multiple users buy the “last item”
**Example:**
Two users check stock at the same time → both see “1 item left” → both succeed.

**2. Shared Resource Race**
- Multiple requests update the **same data simultaneously**
- No locking or synchronization
- Final state depends on which request finishes last
**Example:**
Two checkout requests update the same inventory record at once.

**3. Atomicity Violation**
- A process that should run **all at once** is split into steps
- Another request interrupts mid-process
- Causes inconsistent or invalid states
**Example:**
Payment succeeds but order confirmation fails due to interference.

---

# 🛠️ Lab Setup

**Tools Used**
- Firefox (AttackBox)
- Burp Suite
-- Proxy
-- Repeater
- FoxyProxy

**Environment Configuration**
1. Enable **Burp proxy** via FoxyProxy
2. Launch Burp Suite → Temporary Project → Start Burp
3. Ensure **Intercept is OFF** in Burp Proxy

## 🔐 Legitimate Purchase Flow

1. Login
-- Username: `attacker`
-- Password: `attacker@123`
2. Add **SleighToy Limited Edition** to cart
3. Click **Checkout**
4. Click **Confirm & Pay**
5. Observe successful order confirmation

## ⚔️ Exploiting the Race Condition

**Step 1: Capture the Request**
- In Burp → **Proxy** → **HTTP history**
- Find `POST /process_checkout`
- Right-click → **Send to Repeater**

**Step 2: Prepare Parallel Requests**
1. In Repeater:
-- Create a **Tab Group** (e.g., `cart`)
-- Duplicate the request (e.g., **15 copies**)
2. Select:
-- **Send group in parallel (last-byte sync)**

**Step 3: Trigger the Race**
- Send all requests **simultaneously**
- Backend processes multiple checkouts before stock updates

### 📉 Results

- Multiple successful orders
- Inventory count goes **negative**
- Race condition successfully exploited

## 🧯 Mitigation Techniques

To prevent race condition vulnerabilities:

- ✅ Use **atomic database transactions**
- ✅ Validate stock **at commit time**
- ✅ Implement **idempotency keys** for checkout
- ✅ Apply **rate limiting** and concurrency controls
- ✅ Use proper **locking mechanisms**

## 🔐 Security Takeaway

> A few milliseconds can break business logic.

Race conditions are **logic flaws**, not input validation issues.
They often remain hidden until exploited under **high concurrency**.
