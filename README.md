# Dynamic SSL Key/Cert Management with Nginx Plus
This repo has the necessary files to take you through the various stages of dynamically managing SSL keys and certificates with Nginx Plus. There are seperate .conf file for each step as we progress.
The intention of this repo is to demonstrate a specific functionality, please ensure you have necessary checks and security enabled when you use similar config in your Production Environments. 

Link to YouTube Video:
* [LINK](https://youtu.be/aeLE988jmlo)

### Prerequisites

What you need to run this locally:

1. Ubuntu 18.04 with sudo access ( or other similar supported OS )
1. NGINX Plus ( version R18 or above )

#### Creating self-signed certificate on your VM

```
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/www.example123.com.key -out /etc/nginx/ssl/www.example123.com.crt
$ sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/www.example123.blog.com.key -out /etc/nginx/ssl/www.example123.blog.com.crt
```

#### Editing your hosts file (based on your config/requirement)

```
$ sudo vi /etc/hosts
<local-ip-address>  localhost www.example123.com  example123.com  www.example123.blog.com example123.blog.com www.example123.test.com example123.test.com
```

### Task 1: Set up http > https forwarding (task1.conf)


This task has the configuration file which enables you to listen to port 80 on <local-ip-address> and forwards the request to the secure server with `https://$server_name$request_uri;`
Please ensure you have the .crt & .key available at `/etc/nginx/ssl/`


### Task 2: Serve Multiple SSL Connections (task2.conf)

In this task we include another server name which nginx will listen for by including another `server` block in the configuration. 


### Task 3: Replace cert/key name with variable (task3.conf)

In this task we combine the multiple server configs into one server block. However, you may still need multiple server blocks based on your requirements. 
In this config we introduce the variable `$ssl_server_name` instead of the name of .crt & .key


### Task 4: Key-Value: Storing certs/keys in memory (task4.conf)

In this task we introduce the off-disk storage of cert & key with key-value data store. 
Review the additional links at the bottom of this document to get more details on how key-value store works.
This configuration replaces the link for on-disk .crt & .key with the in-memory data store. 
Additionally, we set-up the N+ Dashboard to view the metrics generated. 


#### How to view the Key-Value datastore:


Viewing content of the ssl_crt key-value datastore:  `$ curl -X GET 'http://localhost:8080/api/6/http/keyvals/ssl_crt'`
Viewing content of the ssl_key key-value datastore:  `$ curl -X GET 'http://localhost:8080/api/6/http/keyvals/ssl_key'`


#### How I uploaded by key & crt to my Key-Value datastore:

Please Note: The following keys are just to demonstrate an example. It will not work if you copy & paste the content. 
Insert the content of your generated -self signed certificate in the example below.

```
$ curl -d '{
"www.example123.com": "-----BEGIN CERTIFICATE-----\nMIIDuTCCAqGgAwIBAgIUaHlKYvY8ZUhX2hC6aybFoD+raF0wDQYJKoZIhvcNAQEL\nBQAwbDELMAkGA1UEBhMCQVUxDDAKBgNVBAgMA1ZJQzEMMAoGA1UEBwwDTUVMMQsw\nCQYDVQQKDAJGNTEOMAwGA1UECwwFTkdJTlgxDDAKBgNVBAMMA0pBWTEWMBQGCSqG\nSIb3DQEJARYHakBkLmNvbTAeFw0yMDA4MTQwMzM0MzJaFw0yMTA4MTQwMzM0MzJa\nMGwxCzAJBgNVBAYTAkFVMQwwCgYDVQQIDANWSUMxDDAKBgNVBAcMA01FTDELMAkG\nA1UECgwCRjUxDjAMBgNVBAsMBU5HSU5YMQwwCgYDVQQDDANKQVkxFjAUBgkqhkiG\n9w0BCQEWB2pAZC5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDM\nzMZAicfy5m5OhxaLR0i5hB/i6PjlTqrn+tJa2mLbtujvSDHwPk+JtInpmtT3sh5u\nVYLDwNEFwrpP6ha+ZyW63mxiRvBWQwzK0EeJfwb6guWwpbmFgEU9HqX8FUj9+huF\nZZ7soD4YeQf+/S7YOJ2U+eOVgotX9Nbj5O0rV8W24meMfi+32D3lKi8yjaY1wb3c\ncCO+Pg+/y5Lql8SBcxNeBz6CssmovOqkLg7cammWVwCnYBOY1FAyKHBPfqGk2hjP\nS9D9pEBp03AIWLtlz/46EvTXPQeegsx+T27oYQqFqqPrGIcIK+NORK3bcb/r0eVq\ntlLA5jzdUw32fkZPVh0HAgMBAAGjUzBRMB0GA1UdDgQWBBSchTXsv3NAEUEJ8Tip\nT5ctzWjT1jAfBgNVHSMEGDAWgBSchTXsv3NAEUEJ8TipT5ctzWjT1jAPBgNVHRMB\nAf8EBTADAQH/MA0GCSqGSIb3EQEBCwUAA4IBAQAQXqukqwi70IQKsKYRRSXPj5iH\nuxrebEvHdNZY96hCdIl/OwjP+fASx8j3FQdsoY3qhyyqQxjuHVbEkflhXofM038s\nGZ/gtzONE7XFtUQlUz4CAYCETw5AC4Jv0Qyepkh/ZrEA5aajsS8rJfGG/HTnozJp\nft4lLqD/uOI++058bn2g8EfSls109t6wzTcaGFCh3JqtlJ/T1sLUbiu1+Vz3C3T9\ny0TntkM0DTU9TeAdFEsVq+ac4Um11C0zHKO+8YmupCExKiil+ZVG8I+Dq2enlENk\nsBA00w3V2qDco26bXcN61MURrLo2WD5y3zgm9JAiSa38Mndph3eexwI/8TQC\n-----END CERTIFICATE-----"
}' http://localhost:8080/api/6/http/keyvals/ssl_crt


$ curl -d '{
"www.example123.com": "-----BEGIN PRIVATE KEY-----\nMIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQDMzMZAicfy5m5O\nhxaLR0i5hB/i6PjlTqrn+tJa2mLbtujvSDHwPk+JtInpmtT3sh5uVYLDwNEFwQpP\n6ha+ZyW63mxiRvBWQwzK0EeJfwb5guWwpbmFgEU9HqX8FUj9+huFZZ7soD4YeQf+\n/S9YOJ2U+eOVgotX9Nbj5O0rV8W24meMfi+32D3lKi8yjaY1wb3ccCO+Pg+/y5Lq\nl8SBcxNeBz6CssmovOqkLg7cammWVwCnYBOY1FAyKHBPfqGk2hjPS9D9pEBp03AI\nWLtlz/46EvTXPQeegsx+T27oYQqFqqPrGIcIK+NORK3bcb/r0eVqtlLA5jzdUw32\nfkZPVh0HAgMBAAECggEAAOpldIJpL2/STG2ULzk5XQL3NYd/HV9VqkXXzOovCPZv\nYip7dqyApIf3GeFEVHsqYanmNMPW62SqjCLqyR0i8Qvvhhz1FB2mn/2AZF/6AwGP\nz1NeWfdx180cRt09f00v9E+4/yvUOz3HSF+PZw4RvknDt7ZmsxT6JPqwCMKSsrec\nWqPdvZh4FgVzjFlS0W1jvXMrsNavto2SvYksjM4qtMVR5jPhUkV2g617lcHBBYXB\nGl77YtIV3DksQBq8UnyN5tnTModAXqCvwtv+YwXbrM50f5FiftwEnbo8gm7Pa+o9\nTwtd7EbczHYWAtMTW4w+NV32lbJ9EkMBx4HWYRDtIQKBgQD6kPzqtl18npxG0fgk\nBh9URq0PLuH1y47pFWPRAEMCFJFGRJUmg6+ChqSCxy76KnWJYR73X6ssPFA15ezP\n/A5sJm6Cvss5othNG3quhePRm/AuNkgKzdSD9awN/vaapnbX6H+OCwMmx3sRhldk\nLqS7YOJ2U+eORYrthcFHYZBNdwKBgQDRPbcb4Zi9Wi4ss7MRsaMj01/KSSH0LMdU\nHOw7Ku/WzV6q5hAS2TonVlTk/jgiSuALsoV5ApQDXOopkcXeki/QIM2JWYp7FnC/\nu8jdregR01GwxAIJtSWRRl1GHY9CrbJKc5dA8fz8BnOSzLoXC0ml4eakwwXXGQYf\n/b34BfhQ8QKBgQDCW0KDPQ1BA/ruiCH1N3aHtYa5l0EYmnvQ2pGhZZWUgIWrPrl+\ntXinQ29KLdyHmfWvyVDuyxuIZYRGOoH1VmuNgkYITpxuqZ0kN0knJJ3xUgb8oYhC\nMSRd82sxNArvLJ5UnXiLookgRG12y4DwKanJ8RRxsD9W2aCI205v4wK+wwKBgQC+\nPcB4RxsaPh3xYskS81GCx6aXQvruCLCKl2lpOlaqFDtYYqiGmp63GVVChqj+9NjT\nidK0/VUZ4aa9eN5QyNVUBB8cHB8+Xl7Q1KmCdBWl7148u1mm/d5UQYeYslOIqmiK\nLKJ+2AXOFweJlz4yqX6ipcuQTgjHUucwuwG3uaXV4QKBgQDxtUTcTuvvR/lOL3Ue\nH+1jXKvWu1zKkHDGqdi5RSRfybRtbjFoFaukMkz7SCvmMtJMG5HYeF8Oe5h8A7Mm\nvAzBA00wsmC3gCLlD8PTIFqsfI1j0rVnm4u3+7Jdgqj7BrYoOECjN2BFmX6psNaK\nkugyiNxkgBce5APw5stKnKDw+g==\n-----END PRIVATE KEY-----"
}' http://localhost:8080/api/6/http/keyvals/ssl_key

```


## Useful Links
* [Configuring HTTP Servers](http://nginx.org/en/docs/http/configuring_https_servers.html)
* [NGINX SSL Module](http://nginx.org/en/docs/http/ngx_http_ssl_module.html#ssl)
* [NGINX Key-Value Module](http://nginx.org/en/docs/http/ngx_http_keyval_module.html)
* [Admin Guide on Key-Value Store](https://docs.nginx.com/nginx/admin-guide/security-controls/blacklisting-ip-addresses/)

## Troubleshooting

* Check the prmisssions on the .crt & key if you see errors in NGINX with `$ssl_server_name` variable. 
* Ensure that you have verified that the .crt/.key files are both valid. 

## Built With

* [Ubuntu](https://ubuntu.com/) - My favourite Linux OS for testing
* [NGINX Plus](https://www.nginx.com/free-trial-request/) - NGINX Plus Trial

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Authors

* **Jay Desai** - *Other Repos* - [jay-nginx](https://github.com/jay-nginx)
