# IDOR - Santa’s Little IDOR
Learn about IDOR while helping pentest the TrypresentMe website.

## Learning Objectives
- Understand the concept of authentication and authorization
- Learn how to spot potential opportunities for Insecure Direct Object References (IDORs)
- Exploit IDOR to perform horizontal privilege escalation
- Learn how to turn IDOR into SDOR (Secure Direct Object Reference)

Insecure direct object references (IDOR) are a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. The term IDOR was popularized by its appearance in the OWASP 2007 Top Ten.

---

## Why IDOR Is So Easy to Miss

Have you ever seen a link that looks like this:

`https://awesome.website.thm/TrackPackage?packageID=1001?`

When you see a link like this, have you ever wondered what would happen if you simply changed the packageID to 11 or 12? In its simplest form, this can be a potential case for IDOR. 

IDOR stands for Insecure Direct Object Reference and is a type of access control vulnerability. Web applications often use references to determine what data to return when you make a request. However, if the web server doesn't perform checks to ensure you are allowed to view that data before sending it, it can lead to serious sensitive information disclosure.

If the user wants to know the status of their package and makes a web request, the simplest method is to allow the user to supply their packageID. We recover data from the database using the simplest SQL query of:

`SELECT person, address, status FROM Packages WHERE packageID = value;`

However, since packageID is a sequential number, it becomes pretty obvious to guess the packageIDs of other customers, and since the web application isn't verifying that the person making the request is the same person as the one who owns the package, an IDOR vulnerability appears, allowing attackers to recover the details for packages belonging to other users.
Even worse is when a feature like this doesn't require a user to authenticate, then there would be no way to even tell who is making the request!

The full term, Insecure Direct Object Reference, sounds fancy, but it doesn’t really describe what’s going wrong. The “Direct Object Reference” part just means that a system uses an ID (like /user/1) to point to something. That’s not the problem. The real issue is that the system doesn’t check whether the person making the request is allowed to access it.

A lot of people try to “fix” IDORs by hiding or encoding IDs. For example, changing /user/1 to /user/ea21f09b2. That might make it look harder to guess, but if the server still isn’t checking permissions, it’s just as insecure. The vulnerability isn’t about how the object is referenced, it’s about missing authorization checks.

The real issue is **missing authorization checks on the server**.

That’s why we can call it an Authorization Bypass instead. It explains exactly what’s happening: someone is bypassing the rules that decide who can see or change something. Whether the ID is a number, a hash, or a random string, the risk stays the same if the server doesn’t verify access.

---

## Identity Defines Our Reach

To understand the root cause of IDOR, it is important to understand the basic principles of authentication and authorization:
- **Authentication:** The process by which you verify who you are. For example, supplying your username and password.
- **Authorization:** The process by which the web application verifies your permissions. For example, are you allowed to visit the admin page of a web application, or are you allowed to make a payment using a specific account?

You may think that authentication only happens once when you supply your username and password, but that is actually not the case! After providing your credentials, you receive a cookie or a token, called session information. Every subsequent request you make to the application includes this session information, which is verified by the application. This initial verification process is still authentication and happens for each request. This is why websites will often redirect you back to the login page. It means your session information has expired, and thus, you need to reauthenticate with your credentials to receive new session information.

Authorization cannot happen before authentication. If the application doesn't know who you are, it cannot verify what permissions your user has.

---

## Privilege Escalation Types

- **Vertical privilege escalation:** This refers to privilege escalation where you gain access to more features. For example, you may be a normal user on the application, but can perform actions that should be restricted for an administrator.
- **Horizontal privilege escalation:** This refers to privilege escalation where you use a feature you are authorized to use, but gain access to data that you are not allowed to access. For example, you should only be able to see your accounts, not someone else's accounts.

IDOR is usually a form of **horizontal privilege escalation**. You are allowed to make use of the track package functionality. But you should be restricted to only performing that tracking action for packages you own.

---

The simplest form of IDOR is simply changing the user_id to something else means we can see other users' data. Some IDORs might be slightly more hidden.

Encoding isn't the only thing that can be used to hide potential IDOR vulnerabilities. Sometimes the values may look like a hash, such as MD5 or SHA1.

If we understand what value was used to generate the hash, we can perform an IDOR attack by simply replicating the hashing function. Using something like a hash identifier can help you quickly understand what hashing algorithm is being used and can often tell you what data was hashed.

**UUID:** Universal Unique Identifier is a 128-bit value used to uniquely identify an object, entity or information within a particular system or knowledge database.

---

The best way to stop IDOR is to make sure the server checks who is asking for the data every time. It's not enough to hide or change the ID number; the system must confirm that the logged-in user is authorized to see or change that information.

Don't rely on tricks like Base64 or hashing the IDs; those can still be guessed or decoded. Instead, keep all the real permission checks on the server. Whenever a request comes in, check: "Does this user own or have permission to view this item?"

Use random or hard-to-guess IDs for public links, but remember that random IDs alone don't make your app safe. Always test your app by trying to open another user's data and making sure it's blocked.

Finally, record and monitor failed access attempts; they can be early signs of someone trying to exploit an IDOR.

---

> 🔐 **Key Takeaway:**  
> IDOR vulnerabilities occur when applications trust user-controlled identifiers without verifying ownership or permissions on the server.

---

# Answer the questions below

**What does IDOR stand for?**

`Insecure Direct Object Reference`

**What type of privilege escalation are most IDOR cases?**

`Horizontal`

**Exploiting the IDOR found in the view_accounts parameter, what is the user_id of the parent that has 10 children?**

`15`
