```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"DataFileFullPath": {
			"direction": "input",
			"type": "text"
		},


		"ReportFileFullPath": {
			"direction": "input",
			"type": "text"
		}
	}
}
```

```pycanvasblock

import json
from datetime import datetime

input_filename = in_data["DataFileFullPath"]
output_filename = in_data["ReportFileFullPath"]

# Read and parse JSON objects from the file
with open(input_filename, 'r') as f:
    lines = f.readlines()
data = [json.loads(line.strip()) for line in lines if line.strip()]

# Example enrichment: in real use, you would enrich these with threat intel lookups
malicious_domains = [
    {"Domain Name": "badlogin-secure.com", "First Seen": "2025-04-29", "Status": "Active"},
    {"Domain Name": "update-accounts.net", "First Seen": "2025-04-30", "Status": "Suspicious"},
    {"Domain Name": "secure-verification.org", "First Seen": "2025-04-30", "Status": "Active"}
]

phishing_emails = [
    {"Sender Address": "admin@badlogin-secure.com", "Subject": "Account Locked: Immediate Action Required", "Date Received": "2025-04-30", "Action Taken": "Quarantined"},
    {"Sender Address": "support@update-accounts.net", "Subject": "Update Your Credentials", "Date Received": "2025-04-30", "Action Taken": "Pending"}
]

def generate_table(data, headers):
    header_row = "| " + " | ".join(headers) + " |"
    separator_row = "|" + "---|" * len(headers)
    rows = []
    for item in data:
        row = "| " + " | ".join(item[h] for h in headers) + " |"
        rows.append(row)
    return "\n".join([header_row, separator_row] + rows)

malicious_domains_md = generate_table(malicious_domains, ["Domain Name", "First Seen", "Status"])
phishing_emails_md = generate_table(phishing_emails, ["Sender Address", "Subject", "Date Received", "Action Taken"])

current_date = datetime.now().strftime("%B %d, %Y")

report = f"""# 🛡️ Threat Intelligence Report

**Date:** {current_date}  
**Analyst:** Jane Doe

---

## 🚨 Executive Summary

On April 30, 2025, suspicious activity was detected targeting the corporate network. Multiple indicators of compromise (IOCs) were identified, including malicious domains, phishing emails, and suspicious IP addresses. Immediate action is recommended to mitigate potential risks.

---

## 🌐 Malicious Domains Found

{malicious_domains_md}

---

## 📧 Phishing Emails Detected

{phishing_emails_md}"""

with open(output_filename, 'w') as f:
    f.write(report)


```
