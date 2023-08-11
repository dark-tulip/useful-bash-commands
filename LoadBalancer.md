## RoundRobin load balancer on Nginx

### up localhost servers
```bash
echo "Server #1" > s1
echo "Server #2" > s2
echo "Server #3" > s3
```
```bash
php -S localhost:20001 s1
php -S localhost:20002 s2
php -S localhost:20003 s3
```
### create config file
```
username@username:~/nginx-load-balancer$ cat load-balancer.conf 
events {

}

http {
	upstream php_servers {
		server localhost:20001;
		server localhost:20002;
		server localhost:20003;
	}
	server {
		listen 8888;
		location / {
			proxy_pass http://php_servers;
		}
	}
}
```
### link config file
```bash
sudo nginx -c ~/nginx-load-balancer/load-balancer.conf 
```
### reload if you need
```
sudo nginx -s reload
```
### Test:
```bash
> while sleep 0.5; do curl http://localhost:8888; done
Server #1
Server #2
Server #3
Server #1
Server #2
Server #3
Server #1
Server #2
Server #3
...
Server #1
Server #2
Server #3
```
### shutdown two servers
```bash
> while sleep 0.5; do curl http://localhost:8888; done
Server #1
Server #1
Server #1
...
Server #1
```
## Sticky session (ip_hash)
- запросы от одного и того же клиента падают тому же серверу
- если сервер упал, ip_hash перераспределяется на другой
```bash
events {

}

http {
	upstream php_servers {
		ip_hash;
		server localhost:20001;
		server localhost:20002;
		server localhost:20003;
	}
	server {
		listen 8888;
		location / {
			proxy_pass http://php_servers;
		}
	}
}
```
```
> while sleep 0.5; do curl http://localhost:8888; done
Server #3
Server #3
Server #3
...
Server #3
```
## Least connected
- по числу подключений или нагрузки
- директива least_conn
```bash
events {}

http {
	upstream php_servers {
		least_conn;
		server localhost:20001;
		server localhost:20002;
		server localhost:20003;
	}
	server {
		listen 8888;
		location / {
			proxy_pass http://php_servers;
		}
	}
}
```
```bash
cat slow_server.php 
<?php
	sleep(10);
	echo "Slow server is UP!!!\n";

```
- запрос попадет на медленный сервер периодически, в основном на быстрые №2 и №3
```
# FIRST THREAD
> while sleep 0.5; do curl http://localhost:8888; done
Slow server is UP!!!
Server #2
Server #3
Slow server is UP!!!
Server #2
Server #3

# SECOND THREAD works faster
> while sleep 0.5; do curl http://localhost:8888; done
Server #2
Server #3
Server #2
Server #3
Server #2
Server #3
Server #2
