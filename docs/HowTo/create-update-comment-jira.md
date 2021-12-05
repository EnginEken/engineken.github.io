### Creating Ticket

 - Öncelikle kullanılan jira APIsi kullanılarak ticketın create edilmesi gerekiyor. Bunun için de requests ve json kütüphanelerinin import edilmesi gerekiyor.
 - Sonrasında create edilecek ticketın gerekli alanlarının editlenmesini sağlayan bir data gerekiyor. Bu datada `issuetype`, `project` ve `summary/description/assignee` gibi değişkenlerin de yer alması gerekiyor.
!!! note
	`issuetype` ve `project` alanlarının var olan ticketlar üzerinden GET request ile öncelikle bulunması gerekmektedir.
 - Authentication için jira Basic Authentication destekliyor. Bunun için kullanıcı adı ve şifrenin base64 formatında encode edilmesi gerekiyor. Bu encode işleminin çıktısı authentication header olarak kullanılabilir.

```python linenums="1" title="Encoding username and password"
import requests, json, base64
message = "username:password"
message_bytes = message.encode('ascii')
base64_bytes = base64.b64encode(message_bytes)
base64_message = base64_bytes.decode('ascii')
print(base64_message) #Çıktısı authentication header için kullanılacak
```
 
 - Sonrasında APIye POST işlemi ile ticket create edilebilir. Aşağıdaki kod assigneesi "eeken" olan ve verilen `summary` ve `description` alanlarına sahip bir ticket create etmektedir.

```python title="Creating jira ticket" linenums="1"
import requests
import json
from requests.auth import HTTPBasicAuth
# Api link for jira 
url = 'https://jira.hepsiburada.com:8443/rest/api/2/issue/'

# request headers included with authorization. Basic means this is basic authentication and meaningless stuff is base64 encrypted result for "username:pass"
request_headers = {"Authorization": "Basic ZWVrZW46RW5nKkVrbjU=",
                   "Content-Type": "application/json",
                   "Accept": "application/json",
                   }
# You can send data with post or put requests. Created a payload which is included with necessary info to create ticket and make it str with json.dumps
payload = json.dumps( {
  "fields": {
    "issuetype": {
      "id": "3"
    },
    "project": {
      "id": "10032"
    },
	"summary": "Test Ortamı Yeni Salon Altyapı Hazırlığı",
    "description": "Test ortamı taşınması için Salon 2 ye sunucuların kablo altyapısının hazırlanması.",
    "assignee": {
      "name": "eeken",
    }
  }
} )

# This is the code snippet making things done. Sending post request to the 'url' with 'request_headers' to post 'payload'
response = requests.request(
   "POST",
   url,
   data=payload,
   headers=request_headers,
)

# This line is printing the answer to your request with readable format.
print(json.dumps(json.loads(response.text), sort_keys=True, indent=4, separators=(",", ": ")))

```

### Updating and Adding Comment

 - Ticketa comment eklenebilmesi için önce yukarıdaki kod ile create edilen ticketın `key` değerinin alınması gerekiyor. Bu değer ile ticket üzerinde işlem yapılacak.
 - Yine bir post işlemi ve eklenecek commenti data olarak request içerisinde göndererek bu işlem gerçekleştirilebilir.

```python linenums="1" title="Adding comment"
# If you want to edit the created ticket above, you need to provide the 'key' or 'id' value for that ticket. Key value is much more understandable like SYS-17895.
# Above ticket create request answer the request with key value for that ticket. So, you can get the value and assign to a variable with below codeline.
key = json.loads(response.text)['key']

# You need to add key value and "/comment" to your URL to add comment.
url_comment = url + str(key) + "/comment"

# Use the body value for the comment that you want to add
payload = json.dumps( {
  "body": "Altyapı hazırlanmış, sunucular taşınabilir hale gelmiştir.",
} )

# Send POST request to edit the ticket with the above comment.
response = requests.request(
   "POST",
   url_comment,
   data=payload,
   headers=request_headers,
)

print(json.dumps(json.loads(response.text), sort_keys=True, indent=4, separators=(",", ": ")))

```

- Comment ekledikten sonra artık ticketın statusu de değiştirilebilir. Bu işlem için API urline `/transition` eklenmelidir. 
- Yine bir post işlemi olduğu için de post data içerisinde `transition id` belirtilmelidir. Bu id ler yine kurulan jiradan jiraya değişmektedir. Yine get API callları ile bunlar var olan ticketlar için belirlenebilir. Bizim ortamımız için In Progress 61, Resolved 31, Closed is 41 idsine sahiptir.

```python linenums="1" title="Updating Ticket"
# You can change the ticket status with adding key + /transition to you URL
url_status = url + str(key) + "/transitions"

# For the transition to take place, you need to provide at least "update" or "fields" value.
# id value is 61 for the status 'In Progress', 31 for the status 'Resolved' and 41 for the status 'Closed' for our workplace. These values will be different for different workplaces.
payload = json.dumps( {
  "update": {},
  "transition": {
    "id": ""
  }
} )

# This loop is for changing case status to In Progress --> Resolved --> Closed respectively.
for i in range(3):
    payload = json.loads(payload)
    
    if i == 0:
        payload['transition']['id'] = "61"
        print(payload['transition']['id'])
    elif i == 1:
        payload['transition']['id'] = "31"
        print(payload['transition']['id'])
    elif i == 2:
        payload['transition']['id'] = "41"
        print(payload['transition']['id'])

    payload = json.dumps(payload)
    response = requests.request(
        "POST",
        url_status,
        data=payload,
        headers=request_headers,
    )
    print(response)
``` 

!!! note
	- Ticketa epic link vermek için data göndermemiz gereken alan "customfield_10961". Bu değer "fields" alanı altında yer alıyor.
	- Eğer epic link olarak "Network Configuration" vermek istiyorsak yukarıdaki fieldı "customfield_10961": "SYS-17387", şeklinde config etmemiz lazım.