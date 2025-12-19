# Network Discovery - Scan-ta Clause
Discover how to scan network ports and uncover what is hidden behind them.

## Learning Objectives
- Learn the basics of network service discovery with Nmap
- Learn core network protocols and concepts along the way
- Apply your knowledge to find a way back into the server

---

# Discovering Exposed Services

When the access to a server is lost, but at least it's still active, and its IP address is known to us, we can counterattack and find our way back.

1. Know your target. In our case, it is the tbfc-devqa01 server with the 10.67.179.101 IP address.
2. Scan the IP for open ports, especially common ones like 22 for SSH and 80 for HTTP.
3. Explore what's behind the open ports, maybe it's a vulnerable web server running on port 80.
4. Exploit the exposed service, find a way in, and kick out the bad bunnies from the QA server.

---

## The Simplest Port Scan

There are many tools you can use to scan for open ports, from preinstalled Netcat on Linux and PowerShell on Windows, to specialized, powerful tools like Nmap and Naabu.

We will be using `Nmap` here.

Open a new command line terminal and run the following command:

<img width="908" height="604" alt="image" src="https://github.com/user-attachments/assets/1974c554-0606-4b8c-8511-27aa74fe0814" />

The command scanned the top 1000 most commonly used ports and reported if any services were running there. The only results you received are an opened SSH port for remote machine access and a HTTP port for a website. This means you can now access the server via SSH (if you know the password), or open the website by visiting http://10.67.179.101 from within the AttackBox.

<img width="1263" height="745" alt="image" src="https://github.com/user-attachments/assets/b97edbcc-97b7-4d1f-a29c-a58675f4ec39" />

**HTTP:** Hypertext Transfer Protocol (HTTP) is the protocol that specifies how a web browser and a web server communicate. Your web browser requests content from the TryHackMe web server using the HTTP protocol as you go through this room.

**SSH:** Secure Shell (SSH) refers to a cryptographic network protocol used in secure communication between devices. SSH encrypts data using cryptographic algorithms, such as Advanced Encryption System (AES) and is often used when logging in remotely to a computer or server.

---

## Scanning Whole Range

It seems like the website is defaced by bad bunnies, and we don't know the key to enter the admin panel! But worry not. We scanned just 1000 ports, but there are actually 65535 ports where other services can hide! Now let's add the -p- argument to scan all ports, and --script=banner to see what's likely behind the port.

<img width="908" height="451" alt="image" src="https://github.com/user-attachments/assets/9a264431-d0b9-4ea4-92d6-433f2649c8b6" />

Looks like you found a running FTP server and some custom TBFC application. Even though FTP runs on port 21 by default, it's possible to change the port to any other one, such as 21212. Let's try accessing the FTP in anonymous mode with the `ftp` command and see if we can find our way in!

<img width="823" height="480" alt="image" src="https://github.com/user-attachments/assets/046bb99c-4a41-4668-abae-41b964b4ab7b" />

**FTP:** File Transfer Protocol (FTP) is a protocol designed to help the efficient transfer of files between different and even non-compatible systems. It supports two modes for file transfer: binary and ASCII (text).

---

## Port Scan Modes

There is nothing more we can see on the FTP server, so let's move on to the custom TBFC app on port 25251. Since it is not a well-known service like HTTP or FTP, your web browser or FTP client won't know how to access it. Luckily, you can always use Netcat (nc), a universal tool to interact with network services.

<img width="662" height="241" alt="image" src="https://github.com/user-attachments/assets/7c3e519e-5f6b-44be-a2e5-48e8f71d6e72" />

Once you received the key, press CTRL+C to exit the Netcat client.

---

## TCP and UDP Ports

Till now, we have scanned only TCP ports, but there are also _**65535**_ ports for UDP, another transport protocol. And there is a chance HopSec secrets are hiding there, too! You can switch to UDP scan by specifying the -sU flag.

<img width="902" height="341" alt="image" src="https://github.com/user-attachments/assets/8b0af3af-7510-4011-9e54-52dab0058d67" />

After a minute you should see an open port 53 associated with DNS - a protocol that drives the modern web by connecting domains to IPs, and many more. Let's ask the DNS server if it knows the key by using `dig` - a command to perform advanced DNS queries.

<img width="805" height="68" alt="image" src="https://github.com/user-attachments/assets/3a365b67-97cc-4ecd-b81d-c1fa82cb3b45" />

---

# On-Host Service Discovery

Now that you know all three keys to the tbfc-devqa01 QA server, it's time to call your TBFC teammates and kick out the bad bunnies.

But first, log in to the server's admin panel by visiting http://10.67.179.101 from within the AttackBox and access the secret admin console by submitting the combined keys.

## Listing Listening Ports

Once you have access to the console, there is no need to scan the ports, as you can simply ask the OS to list its open ports, also called listening ports. You can do it by running `ss -tunlp` (or `netstat` on older systems) inside the Secret Admin Console of the web app. In the output, you may see exactly the same services you scanned before listening on 0.0.0.0, but also some listening on 127.0.0.1 (available only from the host itself)

With root permissions, you can also view the process column. However, for now, let's focus on the **3306** port, which is the default MySQL database port. Usually databases require a password for remote clients, but allow unauthenticated logins from localhost. Since you are already inside the host, let's see the database content by using the `mysql` program.

<img width="386" height="139" alt="image" src="https://github.com/user-attachments/assets/d4543ee0-14a3-4fc4-b821-026bbe6bdc2a" />

We have finally exposed all bunnies' secrets and regained full access to the QA server.

---

# Answer the Questions Below

**Which port was the MySQL database running on?**
`3306`
