# Phishing - Phishmas Greetings
Learn how to spot phishing emails from Malhare's Eggsploit Bunnies sent to TBFC users.

## Learning Objectives
- Spotting phishing emails
- Learn trending phishing techniques
- Understand the differences between spam and phishing

**Phishing:** When emails are sent to a target(s) purporting to be from a trusted entity to lure individuals into providing sensitive information.

---

# Phishing Overview

Phishing may be one of the oldest cyber tricks, but it’s still one of the most effective.

Modern phishing focuses on precision and persuasion. Messages are carefully crafted, often mimicking real people, portals, or internal processes to trick even the most cautious users.

Common intentions behind phishing messages:
- **Credential theft:** Tricking users into revealing passwords or login details.
- **Malware delivery:** Disguising malicious attachments or links as safe content.
- **Data exfiltration:** Gathering sensitive company or personal information.
- **Financial fraud:** Persuading victims to transfer money or approve fake invoices.

> free domain (gmail.com)

Even though systems are well protected, phishing still works because it tricks people. Technology can be secured — humans can still make mistakes.

---

# Why Spam Is Not Phishing?

**Spam** is mostly just digital noise — annoying messages that clutter your inbox but usually don’t cause real harm.

Phishing, on the other hand, is a targeted attack. It is carefully crafted to trick specific people, like TBFC employees, into clicking links, sharing credentials, or opening malicious files. This makes phishing one of the easiest ways for attackers to break into an organisation.

Spam focuses on **quantity over accuracy**. Messages are sent in bulk to as many users as possible, usually for advertising or promotion, not data theft.

Phishing focuses on **precision**. It aims to deceive the right person at the right time in order to gain access to systems, data, or accounts.

Common intentions behind spam messages:
- **Promotion:** Advertising products, services, or events. Often unsolicited or low-quality.
- **Scams:** Spreading fake offers or “get rich quick” schemes to attract clicks.
- **Traffic generation (clickbait):** Driving users to external sites or boosting ad metrics.
- **Data harvesting:** Collecting active email addresses for future campaigns.

<img width="1475" height="805" alt="image" src="https://github.com/user-attachments/assets/8ea738bd-1fe4-4630-88f0-ce2d7e4b27a8" />

In the above screenshot, it's a sample marketing email offering logistics solutions for the SOC-mas event. We can observe that the intention behind the message is pure marketing.

You need to be careful when analysing each message. Look for the intention behind it. Not every unwanted email is a threat; sometimes it’s just spam that poses no real risk.

Attackers use some common techniques in almost every phishing attempt. In a phishing attack, at least one of the following techniques is present:

## **Impersonation**

Some attackers may act as a person, department, or even a service to lure users. This is a way to gain credibility by impersonating the recipient manager or an important person within the company.

<img width="1483" height="641" alt="image" src="https://github.com/user-attachments/assets/e70f92cf-c5e2-4624-8e79-71a0cdc33e59" />

We can easily spot impersonation attempts by looking to see if the sender's email matches the internal domain or the standard email structure of the company. In this case, the sender's domain is a free domain from Gmail, which is not aligned with TBFC's domain.

## **Social Engineering**

Social engineering in phishing is the art of manipulating people rather than breaking technology. Attackers craft believable stories, emails, calls, or chat messages that exploit emotions (fear, helpfulness, curiosity, urgency) and real-world context to lure the recipients of a message.

We can spot multiple social engineering techniques from the previous mail:
- **Impersonation:** Is a type of Social Engineering. The attacker is pretending to be McSkidy!
- **Sense of urgency:** We can observe words such as "urgent" and "immediately" to pressure the recipient.
- **Side channel:** The attacker tries to discourage the recipients from reaching McSkidy using his standard communication channels (phone and email address).
- **Malicious intention:** The attacker is trying to trick the user into giving VPN credentials. They can also try to ask for approval of payments, opening malware, or sharing sensitive data.

<img width="1482" height="641" alt="image" src="https://github.com/user-attachments/assets/7ef5bcd1-5972-4d0a-80cd-de98b594335b" />

## **Typosquatting and Punycode**

Checking the sender’s domain is a good way to spot fake emails.
But attackers know people don’t always look closely — so they use tricks like typosquatting and punycode.

**Typosquatting**

Typosquatting happens when attackers register domains that look almost like real ones, hoping users won’t notice small mistakes.

**Example:**
- `github.com` → `glthub.com` (letter **l** instead of **i**)

If a user doesn’t look carefully, they may trust the fake domain and fall for the attack.

**Punycode**

Punycode allows Unicode characters (used in languages like Cyrillic or Greek) to appear as normal Latin letters.

Attackers exploit this by replacing letters with **look-alike characters**.

**Example:**
- Normal `tryhackme.com`
- Fake `тrуhackme.com` (Cyrillic letters instead of Latin)

To users, both look identical, but the second one is malicious.

### Why This Is Dangerous
Both typosquatting and punycode:
- Create fake domains that look legitimate
- Trick users into clicking malicious links
- Make phishing emails appear to come from trusted companies

### How to Spot Punycode
- Check the **Return-Path** in email headers
- Look for `xn--` prefixes or encoded characters
- Mismatch between display name and real domain is a red flag

Attackers don’t always need malware or exploits — sometimes they just rely on:
- Tiny spelling mistakes
- Look-alike characters
- User inattention

## **Spoofing**

Email spoofing is another way attackers can trick users into thinking they are receiving emails from a legitimate domain.
The message looks like it came from a trusted sender (the display name and “From:” you see in the preview), but the underlying headers tell a different story.

Although the email appears to come from a legitimate domain, the real truth is revealed by inspecting the email headers.

The most important fields to check are:
- **Authentication-Results**  
- **Return-Path**

These headers show whether the email actually passed authentication checks and where it truly originated from, helping identify spoofed or malicious emails.

On _Authentication-Results_, SPF, DKIM, and DMARC are security checks that help confirm if an email really comes from who it says it does:
- **SPF (Sender Policy Framework):** Specifies which mail servers are authorised to send emails on behalf of a domain. If SPF fails, the sending server is not permitted by the domain owner.
- **DKIM (DomainKeys Identified Mail):** Uses a cryptographic signature to ensure the email was not altered and genuinely originated from the stated domain.
- **DMARC (Domain-based Message Authentication, Reporting & Conformance):** Combines SPF and DKIM results to decide how unauthenticated emails should be handled (deliver, quarantine, or reject).

If **SPF and DMARC fail**, it is a strong indicator that the email is **spoofed**—the sender is impersonating a trusted domain.  
In this case, **all authentication checks failed**, confirming the message is not legitimate.

The **Return-Path** header reveals the actual email address used to send the message.
**Example:** _Return-Path:_ zxwsedr@easterbb.com

This address does **not** belong to the TBFC domain.  
It confirms that the email was **spoofed** and originated from an external attacker-controlled domain.

<img width="1433" height="418" alt="image" src="https://github.com/user-attachments/assets/afa36f9b-b717-4294-85e7-fb63ab26cc4e" />

## **Malicious Attachments**

The most classic way of phishing is attaching malicious files to an email. Usually, these attachments are sent using some sort of social engineering technique in the body.

In the previous email that we analysed, the body tried to trick users into thinking that the attachment was a voice message from McSkidy.
Instead, the file is an .html file:

<img width="1465" height="194" alt="image" src="https://github.com/user-attachments/assets/b955bd7f-5079-4ba0-8203-e1d527f20e6e" />

Malicious attachments are files that cause harm when opened. They can install malware, steal login details, or let attackers access a computer.

HTA and HTML files are often used in phishing because they run outside the browser’s safety limits. This allows the scripts inside them to fully interact with the system, making these attachments very risky.

---

# Trending Phishing

Since email platforms and security tools got much better at blocking suspicious messages and attachments, the Malhare's Eggsploit Bunnies and other attackers had to change their game:

Instead of sending malware files (which are easily caught), they now focus on tricking users into leaving the company’s secure environment.
They often use legitimate tools or websites to make their lures look trustworthy and get users to hand over their credentials or download malicious files themselves.
In short, most phishing attacks aren’t about dropping malware directly; they’re focusing on stealing access.

## Legitimate Applications

Attackers often hide behind trusted services like Dropbox, Google Drive, Google Docs, and OneDrive.
These links look safe and usually pass email security filters, which makes people trust them.

A common trick works like this:
- The attacker sends a shared file link.
- The file looks important or attractive (for example, salary raise, HR document, or laptop upgrade).
- When the user clicks the link, they are taken to a fake document.
- From there, the user is tricked into:
 - Entering login credentials on a fake login page, or
 - Downloading a malicious file.

Because the service itself is legitimate, users often don’t suspect anything — which is exactly what attackers rely on.

>Just because a link comes from a trusted platform does **not** mean the content behind it is safe. Always verify the sender and the purpose of shared files.

## Fake Login Pages

Attackers mainly want usernames and passwords, and fake login pages are one of their easiest ways to steal them.

They create pages that look exactly like real login screens people use every day—such as:
- Microsoft Office 365
- Google accounts
- Email portals
- Company internal login pages

When a user enters their credentials on these fake pages, the details are **sent directly to the attacker** instead of logging the user in.

Because the pages look familiar and trustworthy, many users don’t realize anything is wrong until it’s too late.

Fake login pages are effective because:
- They copy real designs and logos
- Users are already used to logging in daily
- One mistake can give attackers full account access

## Side Channel Communications

Side-channel communication happens when attackers move the conversation **outside email** to avoid company security controls.

Common side channels include:
- SMS
- WhatsApp / Telegram
- Phone calls
- Video calls
- Shared document platforms
- Texted links

By switching platforms, attackers:
- Reduce monitoring by security teams
- Increase trust and urgency
- Continue social engineering without restrictions

This technique is often used after the first phishing message succeeds.

---

Attackers don’t rely on just one trick.  
They combine:
- Trusted platforms
- Fake login pages
- Malicious attachments
- Human psychology

Understanding these techniques helps users and defenders **spot attacks early and stop them before damage occurs**.
