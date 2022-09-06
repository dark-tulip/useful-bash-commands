# Полезные bash команды

### OS architecture
``` bash
dpkg --print-architecture
uname -a
lsb_release -a
```

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

