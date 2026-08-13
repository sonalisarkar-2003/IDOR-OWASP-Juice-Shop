# IDOR- OWASP JUICE SHOP

**Target:** OWASP Juice Shop

**Tester:** Sonali Sarkar

**Date:** 25th June 2026

**Scope:** Internal Lab Testing

## 📌Executive Summary

A security assessment was performed on **OWASP Juice Shop.** An **Insecure Direct Object Reference (IDOR)** vulnerability was identified by modifying the **basket ID (object)** in an intercepted HTTP request using **Burp Suite.**

By manipulating the basket ID, unauthorized access to another user’s (admin) basket was achieved.

**Overall Risk Level:** 🔴 Critical

## 🎯Objective

•	Identify IDOR vulnerability.

•	Perform exploitation via parameter tampering

•	Perform exploitation

•	Assess impact

•	Recommend remediation

## 🧪Methodology
 
|      Phase        |         Description            |
| ----------------- | ------------------------------ |
| Reconnaissance    | Started Juice Shop container and accessed via browser |
| Scanning          | Observed exposed web services on port 3000    |             
| Enumeration       | Created account & added items to basket          |                            
| Exploitation      | Intercepted basket request in Burp Suite & modified the basket ID|
| Post-Exploitation | Accessed admin basket items    |


## 🖥️Lab Setup

| Machine          | IP Address     | Role     |
| ---------------- | -------------- | -------- |
| Kali Victim      | 192.168.56.103 | Target   |  


## 🔍Tools Used

•	Podman

•	OWASP Juice Shop

•	Burp Suite

## 🔎Findings

**🔴 Finding:** Insecure Direct Object Reference (IDOR) Vulnerability

**Severity:** Critical

**Description:** IDOR is a vulnerability that arises when attackers can access or modify objects by manipulating identifiers used in a web application's URLs or parameters. It occurs due to missing access control checks, which fail to verify whether a user should be allowed to access specific data.

IDOR is classified under **Broken Access Control** (A01:2025) in the **OWASP Top 10.**

### 📡Evidence

***Juice Shop Deployment in Victim Machine:***
<img width="940" height="709" alt="image" src="https://github.com/user-attachments/assets/0c763cc8-0905-4755-a2ba-174626decff5" />

***User account created in OWASP Juice shop:***
<img width="742" height="460" alt="image" src="https://github.com/user-attachments/assets/97e7ce28-495c-4479-92f1-ad38b94ccb58" />

***User Basket Items Added:***
<img width="940" height="579" alt="image" src="https://github.com/user-attachments/assets/ce57f6ec-cf2f-438d-8823-6a5f5e4da205" />

***Burpsuite Intercepted the Basket request:***
<img width="940" height="472" alt="image" src="https://github.com/user-attachments/assets/601b64ac-f445-4b9a-90fa-906a545c71ab" />

***Altering Basket ID from 6 to 1:***
<img width="653" height="361" alt="image" src="https://github.com/user-attachments/assets/2be04f8e-f997-4d7f-bbe4-95c2bc81f628" />

***Unauthorized access granted to Admin’s Basket:***
<img width="655" height="356" alt="image" src="https://github.com/user-attachments/assets/c9a1d644-00bc-4c27-be4e-1af9b831d6b5" />

***Admin Basket Items Displayed:***
<img width="1035" height="486" alt="image" src="https://github.com/user-attachments/assets/e8272021-d526-4930-a3bc-66ded481237f" />

### 💻 Exploitation Steps

•	Start Juice Shop container on Victim VM using command:

***podman start juice-shop***

•	Open the application in firefox by navigating to http://192.168.56.103:3000

•	Create a new legitimate account (eg sonali@juice.sh-op)

•	Add items into the basket.

•	Enable **Burpsuite** from **FoxyProxy** and enable the **Burpsuite intercept.**

•	Click on the basket icon

•	**Capture** the request GET/rest/basket/6 in Burpsuite.

•	Exploit **IDOR** by tampering with **basket(Object)I**D modifying it from **6 to 1** and forward the modified request to the server. 

•	Observe server response with **HTTP 200 OK**

•	Turn off the intercept and confirm unauthorized access to **admin’s basket.**

### ✅ Result
•	Unauthorized access confirmed

•	Admin basket data retrieved

### ⚠️ Impact

•	High risk of unauthorized access

•	Risk of data manipulation or theft

•	Potential privilege escalation

## 📊Risk Rating

| Metric     | Value       |
| ---------- | ----------- |
| Likelihood | High        |
| Impact     | High        |
| Risk Level | 🔴 Critical |


## 🛠️Remediation

•	Implement **server-side authorization**

•	**Verify ownership** before granting access

•	Use **UUIDs** instead of sequential IDs

•	Perform regular access control testing

## 🔁Retesting

•	Verify basket endpoint enforces authorization

•	Confirm unauthorized basket IDs return **access denied**

## 📌Conclusion

The exploitation of the **IDOR vulnerability** in Juice Shop demonstrates how insecure object references can lead to unauthorized access. Immediate remediation is required to secure API endpoints and enforce strict access controls.

## 📎Appendix
**Commands Used:**

*sudo apt install podman -y*

*podman pull docker.io/bkimminich/juice-shop*

*podman run -d --name juice-shop -p 3000:3000 docker.io/bkimminich/juice-shop*

*podman ps*

Accessed *http://192.168.56.103:3000*


