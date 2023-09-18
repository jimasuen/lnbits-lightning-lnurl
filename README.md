# lnbits-lightning-lnurl
Documentation on using LNbits Api to pay lighting addresses and LNURL

Link to the relevant part of the LNbits Api: https://legend.lnbits.com/docs#/default/api_payments_pay_lnurl_api_v1_payments_lnurl_post

#TLDR

```curl -X 'POST' 
  'https://your-lnbits-endpoint.com/api/v1/payments/lnurl' 
  -H 'Content-Type: application/json' 
  -d '{
    "description_hash": <string>,
    "callback": <string>,
    "amount": <int>,
    "comment": <string>,
    "description": <string>
}'
```


WHERE:

`description_hash` = the sha256 hash of metadata from decoded LNURL or lightning address  

`callback` = callback url from decoded LNURL or lightning address  

`amount` = amount to be paid; specified in millisats (sats to be paid * 1000)  

`comment` = transaction note that will appear in receiver's lightning wallet  

`description` = memo that will appear in your LNbits wallet  





# Paying Lightning Addresses

Lightning addresses are made up of a username and a domain name:

Eg. hello@lnexample.com

Where:
hello = username
lnexample.com = domain

All lightning addresses can be transformed into the same URL structure:

https://domain.com/.well-known/lnurlp/username

Eg. If your lightning address is hello@getalby.com, the URL would be:

https://getalby.com/.well-known/lnurlp/hello

See: https://github.com/andrerfneves/lightning-address/blob/master/README.md
