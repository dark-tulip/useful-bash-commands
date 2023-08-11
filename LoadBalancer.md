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
