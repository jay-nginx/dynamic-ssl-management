# Dynamic SSL Key/Cert Management with Nginx Plus
This repo has the necessary files to take you through the various stages of dynamically managing SSL keys and certificates in Nginx Plus. There are seperate .conf file for each step as we progress.
The intention of this repo is to demonstrate a specific functionality, please ensure you have necessary checks and security enabled when you use similar config in your Production Environments. 

Link to YouTube Video:
* [LINK](https://youtube.com/)

### Prerequisites

What you need to run this locally:

```
Ubuntu 18.04 with sudo access ( or other similar supported OS )
NGINX Plus ( version R18 or above )
```
#### Creating self-signed certificate on your VM

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/www.example123.com.key -out /etc/nginx/ssl/www.example123.com.crt
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/www.example123.blog.com.key -out /etc/nginx/ssl/www.example123.blog.com.crt
```

#### Editing your hosts file (based on your config/requirement)

```
/etc/hosts
<ip-address>  localhost www.example123.com  example123.com  www.example123.blog.com example123.blog.com www.example123.test.com example123.test.com
```

### Applying the configurations: Task 1

In this task we set up a local 

```
This task has the configuration file which enables you to listen to port 80 and forwards the request to the secure server with https://$server_name$request_uri;
Ensure you have the .crt & .key available at /etc/nginx/ssl/
```

### On success, remove the previous config and replace with config in: Task 2 (task2.conf)

In this task we include another server name which nginx will listen on:

```
Review the additional links at the bottom of the document to view some http >> https forwarding for multiple servers on same port. 
```

### On success, remove the previous config and replace with config in: Task 3 (task3.conf)

In this task we combine the server config into one server block:

```
In this config we introduce the variable $ssl_server_name instead of the name of .crt & .key
```


### On success, remove the previous config and replace with config in: Task 4 (task4.conf)

In this task we introduce the off-disk storage of cert & key with key-value store:

```
Review the additional links at the bottom of this document to get more details on how key-value store works.
This configuration replaces the link for on-disk .crt & .key with the in-memory data store. 
```

#### How to view the Key-Value datastore:

```
Viewing content of the key-value datastore:  curl -X GET 'http://localhost:8080/api/6/http/keyvals/ssl_crt'
Viewing content of the key-value datastore:  curl -X GET 'http://localhost:8080/api/6/http/keyvals/ssl_key'

```
#### How I uploaded by key & crt to my Key-Value datastore:
```
curl -d '{
"www.example123.com": "-----BEGIN CERTIFICATE-----\nMIIDuTCCAqGgAwIBAgIUaHlKYvY8ZUhX2hC6aybFoD+raF0wDQYJKoZIhvcNAQEL\nBQAwbDELMAkGA1UEBhMCQVUxDDAKBgNVBAgMA1ZJQzEMMAoGA1UEBwwDTUVMMQsw\nCQYDVQQKDAJGNTEOMAwGA1UECwwFTkdJTlgxDDAKBgNVBAMMA0pBWTEWMBQGCSqG\nSIb3DQEJARYHakBkLmNvbTAeFw0yMDA4MTQwMzM0MzJaFw0yMTA4MTQwMzM0MzJa\nMGwxCzAJBgNVBAYTAkFVMQwwCgYDVQQIDANWSUMxDDAKBgNVBAcMA01FTDELMAkG\nA1UECgwCRjUxDjAMBgNVBAsMBU5HSU5YMQwwCgYDVQQDDANKQVkxFjAUBgkqhkiG\n9w0BCQEWB2pAZC5jb20wggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDM\nzMZAicfy5m5OhxaLR0i5hB/i6PjlTqrn+tJa2mLbtujvSDHwPk+JtInpmtT3sh5u\nVYLDwNEFwrpP6ha+ZyW63mxiRvBWQwzK0EeJfwb6guWwpbmFgEU9HqX8FUj9+huF\nZZ7soD4YeQf+/S7YOJ2U+eOVgotX9Nbj5O0rV8W24meMfi+32D3lKi8yjaY1wb3c\ncCO+Pg+/y5Lql8SBcxNeBz6CssmovOqkLg7cammWVwCnYBOY1FAyKHBPfqGk2hjP\nS9D9pEBp03AIWLtlz/46EvTXPQeegsx+T27oYQqFqqPrGIcIK+NORK3bcb/r0eVq\ntlLA5jzdUw32fkZPVh0HAgMBAAGjUzBRMB0GA1UdDgQWBBSchTXsv3NAEUEJ8Tip\nT5ctzWjT1jAfBgNVHSMEGDAWgBSchTXsv3NAEUEJ8TipT5ctzWjT1jAPBgNVHRMB\nAf8EBTADAQH/MA0GCSqGSIb3EQEBCwUAA4IBAQAQXqukqwi70IQKsKYRRSXPj5iH\nuxrebEvHdNZY96hCdIl/OwjP+fASx8j3FQdsoY3qhyyqQxjuHVbEkflhXofM038s\nGZ/gtzONE7XFtUQlUz4CAYCETw5AC4Jv0Qyepkh/ZrEA5aajsS8rJfGG/HTnozJp\nft4lLqD/uOI++058bn2g8EfSls109t6wzTcaGFCh3JqtlJ/T1sLUbiu1+Vz3C3T9\ny0TntkM0DTU9TeAdFEsVq+ac4Um11C0zHKO+8YmupCExKiil+ZVG8I+Dq2enlENk\nsBA00w3V2qDco26bXcN61MURrLo2WD5y3zgm9JAiSa38Mndph3eexwI/8TQC\n-----END CERTIFICATE-----"
}' http://localhost:8080/api/6/http/keyvals/ssl_crt


curl -d '{
"www.example123.com": "-----BEGIN PRIVATE KEY-----\nMIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQDMzMZAicfy5m5O\nhxaLR0i5hB/i6PjlTqrn+tJa2mLbtujvSDHwPk+JtInpmtT3sh5uVYLDwNEFwQpP\n6ha+ZyW63mxiRvBWQwzK0EeJfwb5guWwpbmFgEU9HqX8FUj9+huFZZ7soD4YeQf+\n/S9YOJ2U+eOVgotX9Nbj5O0rV8W24meMfi+32D3lKi8yjaY1wb3ccCO+Pg+/y5Lq\nl8SBcxNeBz6CssmovOqkLg7cammWVwCnYBOY1FAyKHBPfqGk2hjPS9D9pEBp03AI\nWLtlz/46EvTXPQeegsx+T27oYQqFqqPrGIcIK+NORK3bcb/r0eVqtlLA5jzdUw32\nfkZPVh0HAgMBAAECggEAAOpldIJpL2/STG2ULzk5XQL3NYd/HV9VqkXXzOovCPZv\nYip7dqyApIf3GeFEVHsqYanmNMPW62SqjCLqyR0i8Qvvhhz1FB2mn/2AZF/6AwGP\nz1NeWfdx180cRt09f00v9E+4/yvUOz3HSF+PZw4RvknDt7ZmsxT6JPqwCMKSsrec\nWqPdvZh4FgVzjFlS0W1jvXMrsNavto2SvYksjM4qtMVR5jPhUkV2g617lcHBBYXB\nGl77YtIV3DksQBq8UnyN5tnTModAXqCvwtv+YwXbrM50f5FiftwEnbo8gm7Pa+o9\nTwtd7EbczHYWAtMTW4w+NV32lbJ9EkMBx4HWYRDtIQKBgQD6kPzqtl18npxG0fgk\nBh9URq0PLuH1y47pFWPRAEMCFJFGRJUmg6+ChqSCxy76KnWJYR73X6ssPFA15ezP\n/A5sJm6Cvss5othNG3quhePRm/AuNkgKzdSD9awN/vaapnbX6H+OCwMmx3sRhldk\nLqS7YOJ2U+eORYrthcFHYZBNdwKBgQDRPbcb4Zi9Wi4ss7MRsaMj01/KSSH0LMdU\nHOw7Ku/WzV6q5hAS2TonVlTk/jgiSuALsoV5ApQDXOopkcXeki/QIM2JWYp7FnC/\nu8jdregR01GwxAIJtSWRRl1GHY9CrbJKc5dA8fz8BnOSzLoXC0ml4eakwwXXGQYf\n/b34BfhQ8QKBgQDCW0KDPQ1BA/ruiCH1N3aHtYa5l0EYmnvQ2pGhZZWUgIWrPrl+\ntXinQ29KLdyHmfWvyVDuyxuIZYRGOoH1VmuNgkYITpxuqZ0kN0knJJ3xUgb8oYhC\nMSRd82sxNArvLJ5UnXiLookgRG12y4DwKanJ8RRxsD9W2aCI205v4wK+wwKBgQC+\nPcB4RxsaPh3xYskS81GCx6aXQvruCLCKl2lpOlaqFDtYYqiGmp63GVVChqj+9NjT\nidK0/VUZ4aa9eN5QyNVUBB8cHB8+Xl7Q1KmCdBWl7148u1mm/d5UQYeYslOIqmiK\nLKJ+2AXOFweJlz4yqX6ipcuQTgjHUucwuwG3uaXV4QKBgQDxtUTcTuvvR/lOL3Ue\nH+1jXKvWu1zKkHDGqdi5RSRfybRtbjFoFaukMkz7SCvmMtJMG5HYeF8Oe5h8A7Mm\nvAzBA00wsmC3gCLlD8PTIFqsfI1j0rVnm4u3+7Jdgqj7BrYoOECjN2BFmX6psNaK\nkugyiNxkgBce5APw5stKnKDw+g==\n-----END PRIVATE KEY-----"
}' http://localhost:8080/api/6/http/keyvals/ssl_key

```




# Project Title

One Paragraph of project description goes here

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them

```
Give examples
```

### Installing

A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```

### And coding style tests

Explain what these tests test and why

```
Give an example
```

## Deployment

Add additional notes about how to deploy this on a live system

## Built With

* [Dropwizard](http://www.dropwizard.io/1.0.2/docs/) - The web framework used
* [Maven](https://maven.apache.org/) - Dependency Management
* [ROME](https://rometools.github.io/rome/) - Used to generate RSS Feeds

## Contributing

Please read [CONTRIBUTING.md](https://gist.github.com/PurpleBooth/b24679402957c63ec426) for details on our code of conduct, and the process for submitting pull requests to us.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags). 

## Authors

* **Billie Thompson** - *Initial work* - [PurpleBooth](https://github.com/PurpleBooth)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* Hat tip to anyone whose code was used
* Inspiration
* etc
