## Updating encryption key
When saving scan results to encrypted file without connection to migVisor console,
Metadata Collector(MC) uses public key provided with the distribution in `public_key.der` file. 

This is a default key and it can be replaced per customer.

Users can obtain their customer encryption key in several ways.

There is no need to restart application to update key it uses. 
It will read it only when started to save results.

### Manual key update
Log into [migVisor](console.migvisor.com) and click on your username at the top right corner of the screen,
from there in *Account settings* on tab *Encryption Control* you can download `public_key.der` file,
which can be used to replace existing file in migVisor Metadata Collector installation directory.

If you don't know where it is installed, you can copy key from the browser and paste it in the MC GUI.

Key can be validated against `SHA256` and `SHA1` hashes on the migVisor page.

#### Automated key update
In case you provide support for multiple customers, it may be a good idea to automate key retrieval and update process.

First of all you will need to get API access token of the customer user.

Then binary key can be retrieved via GET request:
```
curl -f --request GET \
  --url https://console.migvisor.com:8443/migvisor/encryption/key/binary \
  --header "Accept: application/octet-stream" \
  --header "Authorization: $token" \
  --output public_key.der
``` 
`ETag` and `If-None-Match` are also supported, so `curl` parameters `--etag-save` and `--etag-compare` whould work.  
