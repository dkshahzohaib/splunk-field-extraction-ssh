# 🔍 Splunk Field Extraction – SSH Logs (SOC Use Case)

## 📌 Overview
This project demonstrates how to extract meaningful fields from raw SSH authentication logs using Splunk. In real-world security environments, logs are often unstructured, which makes it difficult to quickly identify malicious activity. The goal of this project is to transform raw log data into structured, searchable fields that improve visibility, detection, and investigation.

## 🎥 Video Demonstration
A full walkthrough of this project is available here:  
👉 https://www.linkedin.com/posts/syed-zohaib-708211187_cybersecurity-splunk-socanalyst-ugcPost-7451842238701092864-y3Qe?utm_source=share&utm_medium=member_desktop&rcm=ACoAACv947gB_GcXdSPeZqdeQqbzWJXFP8ShwUs

## 🧪 Sample Logs
```

Thu Apr 18 2026 10:28:12 mailsv1 sshd[5276]: Failed password for invalid user appserver from 194.8.74.23 port 3351 ssh2
Thu Apr 18 2026 10:28:12 mailsv1 sshd[1039]: Failed password for root from 194.8.74.23 port 3768 ssh2
Thu Apr 18 2026 10:28:12 mailsv1 sshd[5258]: Failed password for invalid user testuser from 194.8.74.23 port 3626 ssh2
Thu Apr 18 2026 10:28:12 mailsv1 sshd[1165]: Failed password for apache from 194.8.74.23 port 4604 ssh2
Thu Apr 18 2026 10:28:12 mailsv1 sshd[3760]: Failed password for invalid user mongodb from 194.8.74.23 port 2472 ssh2

````

## 🔍 Field Extraction
Using Splunk’s SPL and regex, the following fields were extracted:
- **src_ip** → Source IP address (e.g., 194.8.74.23)  
- **port** → Source port number (e.g., 3351, 3768)

## ⚙️ Splunk Query
```spl
index=auth_logs "Failed password"
| rex "from\s+(?<src_ip>\d{1,3}(?:\.\d{1,3}){3})"
| rex "port\s+(?<port>\d+)"
| stats count by src_ip, port
| sort -count
````

## 🧠 Methodology

The process followed in this project:

1. Ingested SSH logs into Splunk (`auth_logs` index)
2. Queried failed login events
3. Applied regex-based field extraction for IP and port
4. Structured data using Splunk fields
5. Aggregated results to identify patterns
6. Analyzed repeated login attempts from the same IP

## 🚨 Use Case: Brute-Force Detection

The logs simulate a brute-force attack scenario where a single external IP attempts multiple login attempts using different usernames. By extracting and analyzing the `src_ip` and `port` fields, it becomes easier to detect:

* Repeated failed login attempts
* Multiple usernames targeted
* Suspicious access patterns

These are key indicators of a brute-force attack.

## 🛠️ Skills Demonstrated

* Log analysis and parsing
* Regex field extraction
* Splunk SPL querying
* Security monitoring and detection
* SOC analyst workflow

## 📊 Importance

Raw logs are difficult to analyze at scale. By converting unstructured logs into structured data:

* Detection becomes faster and more accurate
* Analysts can quickly identify threats
* Automation and alerting become possible

## 🚀 Future Improvements

* Create alert rules for brute-force detection
* Correlate failed and successful login events
* Build dashboards for visualization
* Expand to other log sources (Windows Event Logs, Web logs)

## 📁 Project Structure

```
splunk-field-extraction-ssh/
│── README.md
│── queries/
│     └── ssh_field_extraction.spl
│── sample_logs/
│     └── ssh_logs.txt
│── screenshots/
│     └── extraction_example.png
```

## 🧾 Conclusion

This project highlights how simple field extraction techniques can significantly improve security monitoring. By transforming raw SSH logs into structured data, analysts can efficiently detect brute-force attacks and respond proactively.

---

⭐ If you found this useful, consider giving the repo a star!


