```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"Target": {
			"direction": "input",
			"type": "text"
		},


		"Results": {
			"direction": "output",
			"type": "text"
		},

		"data_label": {
			"direction": "output",
			"type": "text"
		}
	}
}
```

```pycanvasblock

#input is an email domain name like ibm.com and looks on google for pages where an email exists with this
#output are all the emails it finds
#note - this tool doesn't always work the best

import socket

target_data = in_data["Target"]
out_data['data_label'] = "emails"

import re
import sys
import urllib.request
import urllib.error
from sys import stdout

def StripTags(text):
		finished = 0
		while not finished:
			finished = 1
			start = text.find("<")
			if start >= 0:
				stop = text[start:].find(">")
				if stop >= 0:
					text = text[:start] + text[start+stop+1:]
					finished = 0
		return text


domain_name = target_data

d = set()

# Web search
page_counter_web = 0
try:
    while page_counter_web < 50:
        results_web = f'https://www.google.com/search?q=%40{domain_name}&hl=en&lr=&ie=UTF-8&start={page_counter_web}&sa=N'
        request_web = urllib.request.Request(results_web)
        request_web.add_header('User-Agent', 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36')
        opener_web = urllib.request.build_opener()

        text = opener_web.open(request_web).read().decode('utf-8')
        d.update(re.findall(r'([a-zA-Z0-9._%+-]+@' + re.escape(domain_name)+r')', StripTags(text)))

        page_counter_web += 10

except urllib.error.URLError as e:
    print(f"Cannot connect to Google Web: {e.reason}")

out_data['Results'] = list(d)

```
