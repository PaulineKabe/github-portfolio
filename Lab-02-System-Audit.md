## Lab 02: System Audit

Completed by: Pauline Kabambi
Due Date: Feb 23. 2026

## Section 1: System Inventory

I. Summary 

<img width="782" height="281" alt="image" src="https://github.com/user-attachments/assets/49e2d052-d515-42ba-894d-1f87b5c5d543" />

II. Evidence from my computer 

<img width="1253" height="887" alt="image" src="https://github.com/user-attachments/assets/7c8e8f0d-04c7-48e0-bdfb-8e111408b462" />

## Section 2: Access Control Model

I. Operating System Identification of the access control model 

My system used the DAC (Discretionary Access Control), as shown below. It is a DAC system because. I can perform the following actions: 

i. can modify permissions. 
ii. Users with permission can grant access to others.
iii. Control is discretionary (left to the owner).

<img width="466" height="507" alt="image" src="https://github.com/user-attachments/assets/ad95105c-1377-4c5c-8fcc-4b49d17c24e4" />


II. What is DAC?

DAC (or Discretionary Access Control) is a type of access control. According to microsoft.com, it is any object that a system (or computer) owner protects and can grant access to users at their discretion.
DAC offers case-by-case control over resources.

Source: https://www.microsoft.com/en-us/security/business/security-101/what-is-access-control#:~:text=Different%20types%20of%20access%20control,How%20access%20control%20works


III. My system demonstrating the Principle of Least Privilege

As the screenshot below shows, on my Windows system, my account (MicrosoftAccount\paulinekabambi@gmail.com) is a member of the Administrators group by default. In this specific case, Windows enforces the privacy policy via User Account Control (UAC). Even as an administrator, I don't automatically have elevated privileges. For actions such as installing software or changing system settings, the system always requires explicit permission for privilege escalation. This demonstrates that administrator privileges are restricted until explicit authorization is granted, thus reducing the risk of unauthorized system-wide changes.

<img width="466" height="507" alt="image" src="https://github.com/user-attachments/assets/b6708370-6a61-47de-8c16-00b56e712d69" />

Process 1: Spotify 

<img width="825" height="156" alt="image" src="https://github.com/user-attachments/assets/e66137f4-0bf5-4e42-87ef-c709beb242ee" />

I noticed that my Spotify has been assigned seven (7) PID numbers. I rarely use Spotify on my computer; however, when I turn on my computer, it is one of the apps that opens automatically. Having the Spotify desktop app on my computer can pose (lower) security risks. An attacker could find a way to log into my account via credential stuffing, potentially steal personal information (email, birthday, payment details), or sell the account on the dark web.


Microsoft Edge 

<img width="512" height="357" alt="image" src="https://github.com/user-attachments/assets/952b8d8e-93fd-4e28-8534-4bf1695fa972" />

I often used Microsoft to access my Marymount University accounts since Canvas can’t be accessed with the Google Chrome browser. I also sometimes used it for personal purposes, but not as much as with Google Chrome. I had to do some research to understand how much vulnerability I am exposed to by using Edge. While Microsoft Edge has built-in security (e.g., SmartScreen) to block known malicious sites. In terms of denial-of-service, if I access the wrong site, it can cause my Edge to experience severe system slowdowns and consume all of my computer's CPU power. It would make the browser unusable. Additionally, if I have my card information saved in the browser, and a hacker gains access to my personal information, they can steal it, which represents a risk of data leakage.


3. Adobe Acrobat
   
<img width="787" height="90" alt="image" src="https://github.com/user-attachments/assets/577742e6-212b-4032-9503-82834c3eb722" />


My Adobe Acrobat has been assigned two (2) PID numbers (pictured below). I usually use Acrobat mainly for reading PDFs and signing documents because I use the free subscription option. The security risks may include data leakage and theft, as well as privilege escalation. A hacker can exploit vulnerabilities in JavaScript parsing within Acrobat to access and export sensitive documents from my computer to an external server. In terms of privilege escalation, an attacker may exploit a flaw while Acrobat is running with limited user rights, triggering a crash that allows them to escalate privileges to the Administrator level, which gives them full control over my laptop. 




