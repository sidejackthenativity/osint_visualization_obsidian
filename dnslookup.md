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

#Input takes hostname and returns Ipv4 addresses associated with it as a string seperated by commas
#Input takes Ipv4 address and returns hostname associated with it
#Anything that throws an error or is not unresolvable returns the string unresolvable

import socket

target_data = in_data["Target"]
out_data['data_label'] = "dns"

try:
		# Try to interpret as an IPv4 address
		socket.inet_aton(target_data)
		# If this doesn't raise an exception, it's a valid IPv4 address
		try:
				hostname, _, _ = socket.gethostbyaddr(target_data)
				out_data["Results"] = hostname
		except socket.herror:
				out_data["Results"] = "unresolvable"

except OSError:
		# Not an IPv4 address, treat as hostname
		try:
			hostname, aliaslist, ipaddrlist = socket.gethostbyname_ex(target_data)
			out_data["Results"] = ",".join(str(ip) for ip in ipaddrlist)

		except socket.gaierror:
				out_data["Results"] = "unresolvable"


```
