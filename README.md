# Dynamic SSL Key/Cert Management with Nginx Plus
This repo has the necessary files to take you through the various stages of dynamically managing SSL keys and certificates in Nginx Plus. There are seperate .conf file for each step as we progress.
The intention of this repo is to demonstrate a specific functionality, please ensure you have necessary checks and security enabled when you use similar config in your Production Environments. 

Link to YouTube Video:
<link goes here>

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


### Applying the configurations: Task 1

In this task we set up a local 

```
Give an example
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
