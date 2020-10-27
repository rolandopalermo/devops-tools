# Ubuntu tools

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

## Environment variables
```bash
sudo nano /etc/environment
```