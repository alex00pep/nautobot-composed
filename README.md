# nautobot-composed
A fully automated installation of Nautobot using Docker Compose and custom scripts

Network to Code has an existing published Nautobot Docker Image on Docker Hub. See [here](https://hub.docker.com/repository/docker/networktocode/nautobot). This project uses Docker Compose. The Docker compose file in this project pulls that Nautobot Docker image using the latest stable Nautobot release along with several other Docker images required for Nautobot to function. See the diagram below. This project is for those looking for a multi-container single-node install for Nautobot often coupled with backup & HA capabilities from their hypervisor manager.

![Container Stack](docs/img/container_stack.png)

By default, this project deploys the Nautobot application, a single worker container, Redis containers, and PostgresQL. It does not deploy NGINX, SSL, or any Nautobot plugins. However, the project is extensible to allow users to tailor to their specific requirements. For example, if you need to deploy [SSL](docs/create_ssl_cert.md) or [plugins](docs/plugins.md), see the docs linked. The web server used on the application is [pyuwsgi](https://uwsgi-docs.readthedocs.io/en/latest/).


## Environment Variable Controls

There are two variables that should be set for the Docker-Compose file. The following table covers the environment variables, default setting, and what it does.

| Environment Variable | Default | Notes                                                                                        |
| -------------------- | ------- | -------------------------------------------------------------------------------------------- |
| PYTHON_VER           | 3.11     | This sets the Python version within the container version. 3.8 - 3.11 are currently supported |
| NAUTOBOT_IMAGE       | 1.6.4   | The version of Nautobot to use in the container image                                        |

## NOTE - Celery Worker

This docker-compose includes the Celery worker that was introduced with Nautobot 1.1.0. Please comment out or remove the celery worker in the `docker-compose.yml` if you are using a pre-1.1.0 release.

## NOTE - MySQL

This documentation is now written assuming the latest Docker Compose methodology of using the Docker Compose Plugin instead of the independent docker-compose executable. See the [Docker Compose Plugin installation notes](https://docs.docker.com/compose/install/)
If you want to use MySQL for the database instead of postgres, In step 7 below instead use

```
docker compose -f docker-compose-dev.yml up
```

## Docker Compose

The provided Docker Compose makes use of environment variables to control what is to be used. This is tightly coupled with the Docker image that is provided on Docker Hub.

## Getting Started - Plugins

The installation of plugins has a slightly more involved getting started process. See see the [Plugin documentation.](docs/plugins.md).
And also more than the inpiration repository: https://github.com/nautobot/nautobot-docker-compose.git
## Getting Started

1. Have [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on the host
2. Clone this repository to your Nautobot host into the current user directory.

```
git clone https://github.com/alex00pep/nautobot-composed.git
```

2.2 Create a pseudo-secret for use with Docker Compose for all passwords you need. As example i will generate for PgAdmin4 initial user onboarding.
 The user is admin@example.com.
```bash
echo "admin" > pgamin-secret
```
3. Navigate to the new directory from the git clone

```
cd nautobot-composed
```

4. Copy `env/nautobot.env.example` to `env/nautobot.env`

```
cp env/nautobot.env.example env/nautobot.env
```

5. Update the `.env` file for your environment. **THESE SHOULD BE CHANGED** for proper security!


7. Run `docker-compose up` to start Nautobot

```
export NAUTOBOT_IMAGE=1.6.4;export PYTHON_VER=3.11;docker-compose up -d
```
## Super User Account

### Create Super User via Environment

The Docker container has a Docker entrypoint script that allows you to create a super user by the usage of Environment variables. This can be done by updating the example `.env` file environment option of `NAUTOBOT_CREATE_SUPERUSER` to `True`. This will then use the information supplied to create the super user.

## Navigate to the app
Open Nautobot Web App: use http://<yourhost:8080>/ and login with admin/admin

Open another tab for PgAdmin4: use http://<yourhost:8084>/ and login with admin/admin
## Send Feedback and Support the project
