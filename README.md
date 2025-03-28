<p align="center">
  <img src="https://github.com/user-attachments/assets/4928343a-31cf-4033-a166-3a77eafc74e1" alt="SOC140 - Phishing Mail Detected">
</p>

<h1>SOC165 - Possible SQL Injection Payload Detected EventID: 115 Writeup</h1>

## Alert Triggered!

- This case involves investigating an incident with a **Possible SQL Injection Payload Detected**.

<img width="1399" alt="Screenshot 2025-03-28 at 3 50 42 AM" src="https://github.com/user-attachments/assets/eef22ca7-b509-43d5-8389-21f6a9f34ccf" />

- After taking ownership of the alert, I started the playbook and created a case. I took note of the data surrounding the alert, including Hostnames, Destination IP Address, Source IP Address, and the Requested URL.

## VirusTotal Scan

- I scanned the source IP using VirusTotal to gather more information. The results showed that **4/94 security vendors flagged this IP address as malicious.**

<img width="1436" alt="Screenshot 2025-03-28 at 3 59 10 AM" src="https://github.com/user-attachments/assets/31c4585c-22c2-4a1a-8a1a-a51e9e1f698b" />


- To further confirm, I checked additional threat intelligence sources like **WHOIS* and **AbuseIPDB.**

<img width="414" alt="Screenshot 2025-03-28 at 4 01 28 AM" src="https://github.com/user-attachments/assets/59429372-24bf-4528-ad85-81c42a16bf73" />
<img width="1440" alt="Screenshot 2025-03-28 at 4 04 43 AM" src="https://github.com/user-attachments/assets/445996fc-a30f-4312-8cb1-877c615514d0" />


## Log Management

- Using the log management page, I queried for raw logs to analyze the attack further.

<img width="510" alt="Screenshot 2025-03-28 at 4 11 58 AM" src="https://github.com/user-attachments/assets/e238a8e0-aa6e-4c54-ae8a-9a758e1abfd3" />



- The HTTP request appeared to be URL encoded. I decoded it using **CyberChef** and identified a classic SQL Injection attempt:

```sql
" OR 1=1 --
```

<img width="1440" alt="Screenshot 2025-03-28 at 4 14 10 AM" src="https://github.com/user-attachments/assets/16b7c764-8357-4922-967b-469b3849521c" />


- The HTTP Response Status was **500 (Internal Server Error)**, indicating the SQL Injection attempt was **unsuccessful.**

## Playbook Steps

- Based on the analysis, the behavior was deemed **Malicious**.

- The attack type was confirmed as **SQL Injection.**

- After checking, it was confirmed that this was **Not a Planned Test.**

- The attack was identified as **Internet to Company Network**.

- Due to the response code **500**, it was confirmed that the attack was **Unsuccessful.**
## Artifacts

- **Source IP:** 167.99.169.17 (Flagged as Malicious)
- **Destination IP:** 172.16.17.18 (WebServer1001)
- **URL:** https://172.16.17.18/search/?q=%22%20OR%201%20%3D%201%20--%20-



## Analyst Notes

-  I decoded the requested URL using **URL Decoding** and identified the payload sent by the attacker. After decoding, it was confirmed to be a SQL Injection attempt. 
  By filtering logs using the source address from the Log Management page, I observed additional requests that confirmed the SQL Injection vulnerability.
The response status returned was **500 (Internal Server Error)**, indicating that the attack was unsuccessful. Based on this analysis, no further malicious activity was detected.


## Closing the Alert

- After completing the investigation, I closed the alert as a **True Positive.**


## Conclusion

- This exercise enhanced my skills in detecting and analyzing SQL Injection attacks using practical log analysis and threat intelligence. By following the LetsDefend playbook, I improved my ability to respond to real-world incidents.

## Connect
[LinkedIn - Kency Francois](https://linkedin.com/in/kency-francois)

---
