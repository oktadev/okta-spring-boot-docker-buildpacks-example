# Spring Boot + Docker Example

This example shows how to create a Spring Boot application, secure it, and build it with Docker--all just in 5 minutes! 

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage and secure users and roles in any application.

For further details, please read the [tutorial published on the Okta developer blog][article].

**Prerequisites**:

* [Java 11+][java11]
* Unix-like shell
* [Docker][install-docker] installed
* [Okta CLI][okta-cli] installed

* [Getting Started](#getting-started)
* [Links](#links)
* [Help](#help)
* [License](#license)

## Getting Started

Clone this example's source code:

```bash
git clone https://github.com/oktadeveloper/okta-spring-boot-docker-buildpacks-example.git
cd okta-spring-boot-docker-buildpacks-example
```

Signup with Okta and create your first application:

```bash
okta register
okta apps create
```

Build an application image into local Docker:

```sh
./gradlew bootBuildImage --imageName=springbootdemo
```

Create an `.env` file in the project root and copy your Okta application settings from the `.okta.env` file:

```sh
OKTA_OAUTH2_ISSUER=https://dev-xxxxxx.okta.com/oauth2/default
OKTA_OAUTH2_CLIENT_SECRET=viVC58i6MzQHzAz9BeXjzhWpSz8qbg6U5B4RXnre
OKTA_OAUTH2_CLIENT_ID=zoa1bzlj7DWmzSI8o5d6
```

Run your bootiful application!

```sh
docker run -it -p8080:8080 --env-file .env springbootdemo 
```
## Links

This example uses the following open source libraries from Okta:

* [Okta Spring Boot Starter](https://github.com/okta/okta-spring-boot)
* [Okta CLI](https://github.com/okta/okta-cli)

## Help

Please post any questions as comments on [this repo's blog post][article], or use our [Okta Developer Forums](https://devforum.okta.com/).

## License
Apache 2.0, see [LICENSE](LICENSE).

[article]: http://developer.okta.com/blog/2020/12/28/spring-boot-docker
[okta-cli]: https://github.com/okta/okta-cli
[install-docker]: https://docs.docker.com/get-docker/
[java11]: https://adoptopenjdk.net/

