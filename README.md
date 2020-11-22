
Build a Secure Spring Boot Application and Run It in Docker
=========================
_#springboot #docker #buildpacks #okta #oauth2 #kotln_

This example shows how to bootstrap a Spring Boot application with Okta OAuth 2.0, build it using Buildpacks and run in the Docker - all just in 5 minutes!


> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage and secure users and roles in any application.

For further details, please read the [tutorial published on Okta Blog][article].

**Prerequisites**
* [Java 11+][java11]
* Unix-like shell
* [Docker][install-docker] installed
* [Okta CLI][okta-cli] installed


## Run this application

Clone source code
```bash
git clone https://github.com/ruXLab/spring-boot-docker-buildpacks.git
```

Signup with Okta and create your first application:
```bash
okta register
okta apps create
```

Build application image into local Docker
```sh
./gradlew bootBuildImage --imageName=springbootdemo
```

Create `.env` file in the project root and put your Okta application parameters from `.okta.env` file:
```sh
OKTA_OAUTH2_ISSUER=https://dev-xxxxxx.okta.com/oauth2/default
OKTA_OAUTH2_CLIENT_SECRET=viVC58i6MzQHzAz9BeXjzhWpSz8qbg6U5B4RXnre
OKTA_OAUTH2_CLIENT_ID=zoa1bzlj7DWmzSI8o5d6
```

Run your application
```sh
docker run -it -p8080:8080 --env-file .env springbootdemo 
```


## Help
Please post any questions as comments on [this repo's blog post][article], or use our [Okta Developer Forums](https://devforum.okta.com/).


## License
Apache 2.0, see [LICENSE](LICENSE).


[article]: https://developer.okta.com/blog/2020/11/21/spring-boot-docker
[okta-cli]: https://github.com/okta/okta-cli
[install-docker]: https://docs.docker.com/get-docker/
[java11]: https://adoptopenjdk.net/

