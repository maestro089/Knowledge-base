``` docker
version: "3.9"  
	services:  
		postgres:  
		image: postgres:13.3  
		environment:  
			POSTGRES_DB: "db"  
			POSTGRES_USER: "admin"  
			POSTGRES_PASSWORD: "admin"  
		ports:  
		- "5432:5432"
```

Список контейнеров:
```shell
docker ps
```
Для подключения через консоль к контейнеру:
```shell
docker exec -it 5f8f2edc8f66 bash
```
Подключение к бд
```shell
psql -h localhost -U admin -d db
```