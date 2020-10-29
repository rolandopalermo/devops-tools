# Ubuntu tools

## Table of contents
- [OpenJDK-8](#openjdk-8)
- [Maven](#maven)
- [Docker](#docker)
- [Docker-compose](#docker-compose)
- [PostgreSQL](#postgresql)
  * [Backuping and restoring](#backuping-and-restoring)
- [Environment variables](#environment-variables)
- [Nginx, Lets Encrypt and Certbot](#nginx-lets-encrypt-and-certbot)
  * [Ubuntu 18.x](#ubuntu-18x)
  * [Ubuntu 20.x](#ubuntu-20x)
  * [Lets Encrypt](#lets-encrypt)
  
## OpenJDK-8
```bash
sudo apt install openjdk-8-jdk
java -version
```

OpenJDK 8 can be found at `/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java`

```bash
sudo nano /etc/environment
```

```diff
+ JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
```

```bash
source /etc/environment
echo $JAVA_HOME
```

## Maven
```bash
cd /opt/
wget http://www-eu.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
sudo tar -xvzf apache-maven-3.3.9-bin.tar.gz
sudo mv apache-maven-3.3.9 maven
sudo nano /etc/profile.d/mavenenv.sh
```

```diff
+ export M2_HOME=/opt/maven
+ export PATH=${M2_HOME}/bin:${PATH}
```

```bash
sudo chmod +x /etc/profile.d/mavenenv.sh
source /etc/profile.d/mavenenv.sh
mvn --version
```

## Docker
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
sudo systemctl status docker
sudo usermod -aG docker ${USER}
su - ${USER}
id -nG
```

## Docker-compose
```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

## PostgreSQL
- Install
```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```
- Allow remote connections

```bash
nano /etc/postgresql/10/main/pg_hba.conf
```

```diff
+ host    all             all             0.0.0.0/0               md5
```

```bash
nano /etc/postgresql/10/main/postgresql.conf
```

```diff
+ listen_addresses = '*'          # what IP address(es) to listen on;
```

- Restart server
```bash
sudo service postgresql restart
```

### Backuping and restoring
- Backuping
```bash
sudo -i -u postgres
pg_dump --format=c dbname > /tmp/dbname-`date +%Y-%m-%d-%H%M%S`
```

- Restoring
```bash
sudo -i -u postgres
dropdb dbname
createdb dbname
pg_restore -O -x -d dbname < backup-file
```

## Environment variables
```bash
sudo nano /etc/environment
```

## Nginx, Lets Encrypt and Certbot

### Ubuntu 18.x
- Nginx
```bash
sudo apt-get update && sudo apt-get install nginx
sudo ufw app list
sudo ufw allow 'Nginx HTTP'
sudo ufw status
systemctl status nginx
```

- Certbot
```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt install python-certbot-nginx
```

### Ubuntu 20.x
- Nginx and Certbot
```bash
sudo apt install certbot python3-certbot-nginx
```

### Lets Encrypt
```bash
sudo nano /etc/nginx/sites-available/default
```

Find the existing server_name line and replace the underscore with your domain name:
```diff
+ server_name example.com www.example.com;
```

```bash
sudo nginx -t
sudo systemctl reload nginx
sudo ufw allow 'Nginx Full' && sudo ufw delete allow 'Nginx HTTP'
sudo certbot --nginx -d example.com
```

To renew an existing certificate, run:
```bash
sudo certbot renew --dry-run
```
