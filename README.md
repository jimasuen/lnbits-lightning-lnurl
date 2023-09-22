# lnbits-lightning-lnurl
Documentation on using LNbits API to pay lightning addresses and LNURL

Link to the relevant part of the LNbits API: 
https://legend.lnbits.com/docs#/default/api_payments_pay_lnurl_api_v1_payments_lnurl_post  

For testing purposes, you can inspect your LNURL and lightning address and view the expected `description_hash` using this tool:
https://jimasuen.github.io/tools/decoder/



# STEPS:

1. Decode LNURL using LNbits API. Skip this if you're paying a lightning address.


    ```
    curl -X 'POST'
    https://your-lnbits-endpoint.com/api/v1/payments/decode
    -H "X-Api-Key: <INVOICE KEY>"
    -H "Content-type: application/json"
    -d '{"data": <LNURL>}'
    ```
    
    
    Sample response:
    
    ```
    {
        "domain": "https://decoded-lnurl-domain.com"
    }
    ```


2. Retrieve callback URL and metadata from lightning address or LNURL.


    cURL for lightning address:
    
    
    
    ``` curl 'https://lightning-wallet-domain.com/.well-known/lnurlp/username' ```
    
    
    OR
    
    
    cURL for LNURL:
    
    
    ``` curl 'https://decoded-lnurl-domain.com' ```
    
    
    Sample response:
    
    ```
    {
        "status": "OK",
        "tag": "payRequest",
        "commentAllowed": 255,
        "callback": "https://lightning-wallet-domain.com/lnurlp/username/callback",
        "metadata": "[[\"text/identifier\",\"username@lightning-wallet-domain.com\"],[\"text/plain\",\"Payment\"]]",
        "minSendable": 1000,
        "maxSendable": 11000000000,
        "payerData": {
            "name": {
                "mandatory": false
            },
            "email": {
                "mandatory": false
            },
            "pubkey": {
                "mandatory": false
            }
        }
    }
    ```



3. Process payment using LNbits API:


    ```
    curl -X 'POST' 
      'https://your-lnbits-endpoint.com/api/v1/payments/lnurl'
      -H "X-Api-Key: <ADMIN KEY>"
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
    
    `description_hash` = the sha256 hash of parsed metadata from decoded LNURL or lightning address  
    
    `callback` = callback url from decoded LNURL or lightning address  
    
    `amount` = amount to be paid; specified in millisats (sats to be paid * 1000)  
    
    `comment` = transaction note that will appear in receiver's lightning wallet (write what you want)  
    
    `description` = memo that will appear in your LNbits wallet (write what you want)  




# Relevant links:

1. On lightning addresses: https://github.com/andrerfneves/lightning-address/blob/master/README.md
2. On LNURL payRequest: https://github.com/lnurl/luds/blob/luds/06.md
3. Tool to view expected `description_hash` of parsed metadata from decoded LNURL or lightning address: https://jimasuen.github.io/tools/decoder/
