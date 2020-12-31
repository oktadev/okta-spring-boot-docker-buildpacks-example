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

Select **Web** > **Other**. Use `http://localhost:8080/login/oauth2/code/okta` as the Redirect URI. This will create an `.okta.env` file in your project's root directory. Source it and run it to confirm you can log in to Okta.

```bash
source .okta.env
./gradlew bootRun
```

Open your browser to `http://localhost:8080` and sign in.

Build an application image into local Docker:

```bash
./gradlew bootBuildImage --imageName=springbootdemo
```

Create an `.env` file in the project root and copy your Okta application settings from the `.okta.env` file:

```bash
OKTA_OAUTH2_ISSUER=https://dev-ZZZ.okta.com/oauth2/default
OKTA_OAUTH2_CLIENT_SECRET=XXX
OKTA_OAUTH2_CLIENT_ID=YYY
```

Run your bootiful application!

```bash
docker run -it -p8080:8080 --env-file .env springbootdemo 
```

## Deploy to Heroku

If you'd like to deploy your dockerized Spring Boot app to Heroku, you'll need to use [Heroku Buildpacks](https://jkutner.github.io/2020/05/19/spring-boot-buildpacks.html). This is because the Paketo buildpacks refuse to allocate heap on containers smaller than 1GB of RAM. A free Heroku dyno has 512MB.

First, you'll need to add the following to `src/main/resources/application.properties` so Spring Boot uses Heroku's `PORT` environment variable.

```properties
server.port=${PORT:8080}
```

Then, build your image with `--builder heroku/spring-boot-buildpacks`:

```bash
./gradlew bootBuildImage --imageName=springbootdemo --builder heroku/spring-boot-buildpacks
```

Create an app on Heroku:

```bash
heroku create
```

Then, log in to Heroku's container registry and push your app:

```bash
heroku container:login
docker tag springbootdemo registry.heroku.com/<your-app-name>/web
docker push registry.heroku.com/<your-app-name>/web
```

Set your Okta app settings as environment variables:

```bash
heroku config:set \
  OKTA_OAUTH2_ISSUER="https://{yourOktaDomain}/oauth2/default" \
  OKTA_OAUTH2_CLIENT_ID="{clientId}" \
  OKTA_OAUTH2_CLIENT_SECRET="{clientSecret}"
```

Next, release your container and tail the logs.

```bash
heroku container:release web
heroku logs --tail
```

You'll need to update your Okta OIDC app to have your Heroku app's redirect URIs as well.

- Login redirect URI: `https://<your-app-name>.herokuapp.com/login/oauth2/code/okta`
- Logout redirect URI: `https://<your-app-name>.herokuapp.com`

Run `heroku open` to open your app and sign in.

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

