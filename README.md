# POP3 Protocol Analysis and Security Assessment

# System Setup

Before starting the capturing of POP3 packets, we need to do the following steps:

1. We need to configure the mail server which we are using, to use POP3 instead of IMAPv4. For example, in Gmail, it can be done by following the below steps:
    - Open Gmail on web -> Click **Settings** icon -> Click **See all Settings** -> Go to **Forwarding and POP/IMAP** -> Click the checkbox for ‘**Enable POP for all mail**’.
<br>
    <img width="534" alt="image" src="https://github.com/user-attachments/assets/c3f01133-f62e-461b-a294-5be0c8412a98">
<br>
<br>
    
2. After configuring the mail server, we need to download **Thunderbird** email client or any other user agent and log in using the Gmail account configured in step 1.
<br>
   <img width="524" alt="image" src="https://github.com/user-attachments/assets/d6ce8b1d-30e1-42e9-9951-bf995d8adeac">
<br>
<br> 

3. By default, **TLS (Transport Layer Security)** is used. Hence, we need to acquire the encryption key for decrypting the packets. This can be done by setting a new environment variable named `SSLKEYLOGFILE` and setting its path to a log file (e.g., `key.log`) created in your system.
<br>
<img width="524" alt="image" src="https://github.com/user-attachments/assets/31f16bf3-cfe5-414f-bc64-4f9104ab1320">
<br>
<br>

# Packet Capture Methodology

1. After completing all the steps mentioned in system setup, open **Wireshark**.
<br>
<img width="524" alt="image" src="https://github.com/user-attachments/assets/25b440b6-e90d-4f13-8cff-8e50f7bcdaca">
<br>
<br>

2. Since all the mails will be sent through SSL/TLS, we need to decrypt them using the file created in system setup for the environment variable `SSLKEYLOGFILE`. To do this, follow these steps:
    - Open **Edit** section in Wireshark -> Go to **Protocols** -> Open **TLS** -> Browse for the log file (`key.log`) created earlier and apply these changes.

<br>
<img width="524" alt="image" src="https://github.com/user-attachments/assets/12405bc4-9cbc-430d-92b2-c88b53b7a100">
<br> 
<br>

3. In Wireshark, start the capture in **Wi-Fi** or **Ethernet**, depending on your network connection.
<br>
<img width="524" alt="image" src="https://github.com/user-attachments/assets/20b5a5ad-e98e-4c07-a18f-24c38a65bd39">
<br>
<br>

4. Now, open the **Thunderbird** mail app to view POP3 mails. In addition to email retrieval, the encryption key will be retrieved and stored into the `key.log` file, which will be used in Wireshark for decrypting the POP3 packets.
<br>
<img width="524" alt="image" src="https://github.com/user-attachments/assets/4212af4f-5247-4b4f-8e1e-1ef49fd9b155">
<br>
<br>

5. After that, add a **display filter** for POP3 packets. This will show all the packets related to POP3.
<br>
<img width="524" alt="image" src="https://github.com/user-attachments/assets/7a60180c-978c-4c63-8fb5-05ac625a0cbd">
<br>
<br>

# Outcome of Project

We were able to capture the key packets exchanged during the three main phases of the POP3 session. Below is the sequence and description of packets exchanged during the POP3 session we established:

### Authorization Phase:
Initially, a **CAPA** command is sent to find the capability list, which includes the commands that the client can use to access emails.

The authentication in Thunderbird is done through **OAuth2** and the **AUTH** command, rather than just sending `USER` and `PASS` commands. This is done for security purposes by Google.



### Transaction Phase:
After the user is authenticated, the client sends a **STAT** command to request the status of the mailbox, such as the number of messages and their total size.


After checking the status of the mailbox, the client sends a **LIST** command to retrieve a list of messages and their sizes in the mailbox.


After getting the list of mails, the client sends a **UIDL** command to request the unique identifier for each message in the mailbox. This helps in identifying whether a message has been downloaded before.


The client retrieves message number ‘1’ by sending the **RETR** command.



### Update Phase:
After the client finishes viewing their emails, the **QUIT** command is issued to terminate the connection with the server.



