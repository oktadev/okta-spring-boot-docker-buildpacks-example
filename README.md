# Spring Boot + Docker Example

This example shows how to create a Spring Boot application, secure it, and build it with Docker--all just in 5 minutes! For further details, please read [How to Docker in Spring Boot][article].

**Prerequisites:**

* [Java 11+][java11]
* Unix-like shell
* [Docker][install-docker] installed
* [Okta CLI][okta-cli] installed

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage and secure users and roles in any application.

* [Getting Started](#getting-started)
* [Deploy to Heroku](#deploy-to-heroku)
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

## Deploy to Heroku

If you'd like to deploy your dockerized Spring Boot app to Heroku, you'll need to use [Heroku Buildpacks](https://jkutner.github.io/2020/05/19/spring-boot-buildpacks.html). This is because the Paketo buildpacks refuse to allocate heap on containers smaller than 1GB of RAM. A free Heroku dyno has 512MB.

First, you'll need to add the following to `src/main/resources/application.properties` so Spring Boot uses Heroku's `PORT` environment variable.

```properties
server.port=${PORT:8080}
```

Build your image with `--builder heroku/spring-boot-buildpacks`:

```sh
./gradlew bootBuildImage --imageName=springbootdemo --builder heroku/spring-boot-buildpacks
```

Create an app on Heroku:

```shell
heroku create
```

Then, log in to Heroku's container registry and deploy your app:

```sh
heroku container:login
docker tag springbootdemo registry.heroku.com/<your-app-name>/web
docker push registry.heroku.com/<your-app-name>/web
heroku container:release web
heroku logs --tail
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

