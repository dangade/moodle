docker-moodle
=============
[![Build Status][![](https://images.microbadger.com/badges/version/dangade/moodle.svg)](https://microbadger.com/images/dangade/moodle "Get your own version badge on microbadger.com")[![][![](https://images.microbadger.com/badges/version/dangade/moodle.svg)](https://microbadger.com/images/dangade/moodle "Get your own version badge on microbadger.com")

A Dockerfile that installs and runs the latest Moodle 3.8 stable, with external MySQL Database.

Versions:
- Ubuntu:latest (currently v.18.04 LTS)
- PHP:latest (currently v.7.2)
- MYSQL:8 (currently v.8.0.19)
- Moodle:38 stable (currently v.3.8.1)

## Installation

```
git clone https://github.com/dangade/moodle
cd moodle
docker build -t moodle .
```

## Usage

Test Environment

When running locally or for a test deployment, use of localhost is acceptable.

To spawn a new instance of Moodle:

```
docker run -d --name DB -p 3306:3306 -e MYSQL_DATABASE=moodle -e MYSQL_ROOT_PASSWORD=moodle -e MYSQL_USER=moodle -e MYSQL_PASSWORD=moodle mysql:5
docker run -d -P --name moodle --link DB:DB -e MOODLE_URL=http://localhost:8080 -p 8080:80 dangade/moodle
```

You can visit the following URL in a browser to get started:

```
http://localhost:8080 
```

### Production Deployment

For a production deployment of moodle, use of a FQDN is advised. This FQDN should be created in DNS for resolution to your host. For example, if your internal DNS is company.com, you could leverage moodle.company.com and have that record resolve to the host running your moodle container. The moodle url would then be, `MOODLE_URL=http://moodle.company.com`
In the following steps, replace MOODLE_URL with your appropriate FQDN.

In some cases when you are using an external SSL reverse proxy, you should enable `SSL_PROXY=true` variable.

* Deploy With Docker
```
docker run -d --name DB -p 3306:3306 -e MYSQL_DATABASE=moodle -e MYSQL_ROOT_PASSWORD=moodle -e MYSQL_USER=moodle -e MYSQL_PASSWORD=moodle mysql:8
docker run -d -P --name moodle --link DB:DB -e MOODLE_URL=http://moodle.company.com -p 80:80 dangade/moodle
```

* Deploy with Docker Compose (recommended)

Pull the latest source from GitHub:
```
git clone https://github.com/dangade/moodle.git
```

Update the `moodle_variables.env` file with your information. Please note that we are using v3 compose files, as a stop gap link env variable are manually filled since v3 no longer automatically fills those for use.

Once the environment file is filled in you may bring up the service with:
`docker-compose up -d`

## Caveats
The following aren't handled, considered, or need work: 
* moodle cronjob (should be called from cron container)
* log handling (stdout?)
* email (does it even send?)

## Credits

This is a update of [Jonathan Hardison's](https://github.com/jmhardison/docker-moodle) Project.
