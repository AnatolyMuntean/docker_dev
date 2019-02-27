Docker-ized development environment
===

Description
---
This package expands several docker containers to aid
web development without installing software of host system.

Currently, it is intended to host one single project,
specifically - `Drupal 8`.
Neither virtual hosts, nor multiple databases are intended.
Only one database/user is provided, more info in sections
below.

Environment provides:
- `Apache2 v2.4` web-server with `PHP v7.3.2`;
- Apache's `rewrite` module is enabled by default;
- enabled PHP extensions:
    - curl
    - bcmath
    - exif
    - gd
    - iconv
    - intl
    - mbstring
    - opcache
    - pdo_mysql
    - pdo_pgsql
    - soap
    - xml
    - xsl
    - xmlrpc
    - xdebug (via PEAR, version 2.7.0RC2)
    - zip
- `Postgres v11.2` database engine;
- bundled composer;

Pre-requisites
---
- `Docker` engine, to the time of writing, version `18.09.2`;
- `Docker compose`, to the time of writing, version `1.23.2`;

Installation
---
#### Docker installation
The installation section assumes Ubuntu distribution and
includes most of steps described in the official guide
at https://docs.docker.com/install/linux/docker-ce/ubuntu/.

Update the `apt` packages index:
```bash
sudo apt-get update
```

Install some required software, if unsure
whether they are already installed in your system:
```bash
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

Add the Docker's repositories GPG key:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Add the repository to the system repositories list:
```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Update the `apt` index again:
```bash
sudo apt-get update
```

Install the docker engine:
```bash
sudo apt-get install docker-ce docker-ce-cli
```

Initially the docker daemon won't be added as the
startup daemon, to do so set up it's service:
```bash
sudo systemctl enable docker
```

And, finally, start the daemon:
```bash
sudo /etc/inid.d/docker start
```

#### Docker compose
Docker compose is a tool to orchestrate the building and
controlling docker-ized containers.

Original reference for installing `docker-compose` taken
from https://docs.docker.com/compose/install/.

Download the binary:
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Make it executable:
```bash
sudo chmod +x /usr/local/bin/docker-compose
```

Next step is only necessary if for some reason the executable
is still not available in terminal:
```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

Check it:
```bash
docker-compose --version
```

Something similar should appear:
```bash
docker-compose version 1.23.2, build 1110ad01
```

Configuration
---
Prior to building/running the containers, rename or copy 
the `.env.dist` file to `.env`. Provide the path to your 
application sources root directory and database related
credentials in the respective lines.

Deployment
---
Clone this repository:
```bash
git clone git@github.com:AnatolyMuntean/docker_dev.git && cd docker_dev
```

Build and launch the docker containers:
```bash
sudo docker-compose up -d --build
```

Usage
---

The docker-ized containers are deployed with the following
names:  
Web server: `web`  
Database engine: `db`

The port mapping is defined as following:  
Web port: `89`  
Database port: `5499`  
Xdebug port: `9099` 

That is, to access the web application through the browser,
point the url to `localhost:81`.
To connect to PgSQL instance, use `localhost` as host
and port `5499` as port. Database credentials are as in
the `.env` file created lately.

Xdebug port is exposed at port `9099`.

To stop a container:
```bash
sudo docker-compose down -v
```

To execute a command **inside** a container:
```bash
sudo docker exec CONTAINER_NAME COMMAN_NAME
```