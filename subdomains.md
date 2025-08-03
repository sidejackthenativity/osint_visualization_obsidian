```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"Target": {
			"direction": "input",
			"type": "text"
		},

		"APIKEY": {
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

#Target input is a target domain like ibm.com
#APIKEY input is the api key for dns dummpster
#output is results and label for reporting purposes


target_data = in_data["Target"]
apikey_data = in_data["APIKEY"]
url = "https://api.dnsdumpster.com/domain/"+str(target_data)


import requests


headers = {
    "X-API-Key": apikey_data
}

response = requests.get(url, headers=headers)
data = response.json()

hosts = []

for section in ['a', 'mx', 'ns']:
    if section in data:
        hosts.extend(entry['host'] for entry in data[section] if 'host' in entry)

out_data['Results'] = ','.join(map(str, hosts))
out_data['data_label'] = "domains"




```
