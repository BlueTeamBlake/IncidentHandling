# 🕵️ Investigating with Splunk – TryHackMe Writeup

## 🧠 Scenario Overview

In this lab, we take on the role of a SOC Analyst assisting a colleague, Johny, who has identified anomalous behavior on a number of Windows machines. It appears that an adversary gained access to these hosts and successfully installed a backdoor. Logs from the suspected systems were ingested into **Splunk** for centralized investigation. Our mission: analyze the logs and uncover the attacker's activity.

---

## 🔍 Investigation Questions and Answers

### 1. **How many events were collected and ingested in the index `main`?**  
Loading up our instance and setting our index as 'main' shows us our total events.

![Total Events](question1_answer.png)

**Answer:** `12256`

### 2. **What is the name of the new backdoor user created by the adversary?**  
For this I searched by event ID, found the one corresponding to user creation and our answer was in the event logs with a sneaky mistype.

![Backdoor User](question2_answer.png)

**Answer:** `A1berto`

### 3. **What is the full path of the registry key updated for the backdoor user?**  
Here I wanted to search with our user "A1berto" along with the EventID for reg keys being updated. If the ID's are correct we should find our answer.

![Reg Key Path](question3_answer.png)

**Answer:**  
`HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto`

### 4. **Which legitimate user was the adversary trying to impersonate?** 
This one doesn't need much of an explanation hopefully or a photo. 

**Answer:** `Alberto`

### 5. **What command was used to add the backdoor user from a remote computer?**  
I got stuck here for a good minute trying to find the right thing to search. I ended up searching the fields for "net user /add A1berto paw0rd1" and scrolled through a few logs to get our answer as seen in the previous screenshots and found the answer.

![Full CMD line](question4_answer.png)

**Answer:**
```
C:\windows\System32\Wbem\WMIC.exe" /node:WORKSTATION6 process call create "net user /add A1berto paw0rd1
```

### 6. **How many login attempts from the backdoor user were observed?**  
**Answer:** `0`

### 7. **Which infected host executed suspicious PowerShell commands?**  
**Answer:** `James.browne`

### 8. **How many PowerShell events were logged for the malicious execution?**  
**Answer:** `79`

### 9. **What is the full URL that the encoded PowerShell script attempted to contact?**  
**Answer:**  
`hxxp[://]10[.]10[.]10[.]5/news[.]php`

---

## 📝 Summary

This investigation highlighted how Splunk can be used to rapidly correlate logs across multiple systems, enabling identification of:
- Unauthorized user creation
- Registry modifications
- PowerShell abuse
- Network exfiltration attempts
