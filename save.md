```canvasblocksettings
{
	"type": "workflow",
	"ioConnections":
	{
		"input": {
			"direction": "input",
			"type": "text"
		},

		"data_label": {
			"direction": "input",
			"type": "text"
		},


		"location": {
			"direction": "input",
			"type": "text"

		}

	}



}
```
#this takes data, it's type and location and saves it
#the data type of label is used by the WriteReport module to determine where data goes in the report

```pycanvasblock

import os
import random
import string
import json

save_path = in_data["location"]
file_name = f"Saved_Data.md"
file_absolute = os.path.join(save_path, file_name)

print(file_absolute)

# Prepare the data to be saved
entry = {
    "data_label": in_data["data_label"],
		"array": in_data["input"]
}

# Append the entry as a JSON line to the file
with open(file_absolute, 'a', encoding='utf-8') as file:
    file.write(json.dumps(entry) + '\n')

```
