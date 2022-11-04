# Полезные bash команды

### ssh connection by alias
```
cd .ssh
nano config
```
```
Host gitlab-runner
  Hostname 192.168.10.10
  User gitlab-runner
  IdentitiesOnly=yes
  Port 22
```


#### Удалить все images
`docker rmi -f $(docker images -aq)`
#### To delete all containers including its volumes use,
`docker rm -vf $(docker ps -aq)`

### OS architecture
``` bash
dpkg --print-architecture
uname -a
lsb_release -a
```
#### SELENOID
$ docker run -d --name selenoid-ui  \
    --link selenoid                 \
    -p 10101:10101                    \
    aerokube/selenoid-ui --selenoid-uri=http://selenoid:4444

 docker run -d --name selenoid-ui --link selenoid -p 8080:8080 aerokube/selenoid-ui --selenoid-uri=http://selenoid:4444
    
docker pull aerokube/ggr-ui:latest-release

## Install specific version of node js
``` bash
sudo npm cache clean -f
sudo npm install -g n
sudo n stable
or 
sudo n 14.18.1
```

## Live Server VS code
<img src="https://user-images.githubusercontent.com/89765480/184475711-fe4d7636-1a12-41d4-8f6d-96145907f5b9.png" width="900px" height="300px">
Enable auto save on VS code
File -> Autosave (after delay)


## Убить поднятые приложения по заданному порту
``` bash
sudo kill -9 `sudo lsof -t -i:4200` > /dev/null
sudo kill -9 `sudo lsof -t -i:1313` > /dev/null
```

## Colored echo. 
``` bash
DRAW='\033[92m'
NO_COLOR='\033[0m' # No Color
echo -e ${DRAW}" :: SERVER :: pulled and recreated"${NC}
```

## Update version of java
``` bash
sudo apt-get install openjdk-11-jdk
sudo update-alternatives --config java
```

## Install mysql server
sudo apt-get update
sudo apt-get install mysql-server

## Mysql
``` sql
sudo mysql --user=root mysql

SHOW VARIABLES WHERE Variable_name = 'port';  // default 3306
SELECT DATABASE();
SELECT USER();

CREATE DATABASE testdb;
USE testdb;
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
CREATE USER 'testuser'@'localhost' IDENTIFIED BY '111';
GRANT ALL PRIVILEGES ON *.* TO 'testuser'@'localhost';
```

## Alias for sublime text macos (.zshrc)
``` bash
cd ~
nano .zshrc
# paste your path to sublime
alias subl="/Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl"
# activate alias
source .zshrc
```

# Настройка гитлаб раннера на локальном сервере

- Узнать архитектуру
`Ubuntu 22.04.01 LTS amd64`
```bash
# Download the binary for your system
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

# Give it permissions to execute
sudo chmod +x /usr/local/bin/gitlab-runner

# Create a GitLab CI user
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash

# Install and run as service
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start

# Register the runner
sudo gitlab-runner register --url http://gitlab.companyname/ --registration-token $REGISTRATION_TOKEN
```
