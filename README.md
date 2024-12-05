# Лабораторная работа 4
## Выполнила: Журбина Марина Андреевна К3139
### Создание контейнера
1. Создаем Dockerfile и вписываем туда следующий код:
```
   FROM ubuntu:latest
   RUN apt-get update && apt-get install -y libaa-bin && apt-get install -y iputils-ping
   CMD ["aafire"]
```
  
2. Строим образ, а затем создаем и запускаем контейнер на основе образа: 
```
   sudo docker build -t aafire-image .
   sudo docker run -it --name aafire1 aafire-image
```
  В результате получаем:
     
![image](https://github.com/user-attachments/assets/712c8242-10a8-416f-8fec-9cf035705409)



### Настраивание соединения между 2 контейнерами

1. Удалим предыдущий контейнер:
```
  sudo docker stop aafire1 
  sudo docker rm aafire1 
``` 
2. Создадим два новых контейнера, дописав sleep infinity, чтобы контейнер не останавливал свою работу, чтобы связать их в одну сеть:
```
  sudo docker run -d --name aafire1 aafire-image sleep infinity
  sudo docker run -d --name aafire2 aafire-image sleep infinity
``` 
3. Создаем сеть myNetwork и подключаем к ней, созданные ранее контейнеры:
```
   docker network create myNetwork
   docker network connect myNetwork aafire1
   docker network connect myNetwork aafire2
```
4. Проверим, что ping идет между контейнерами:
```
   sudo docker exec -it aafire1 bash
   ping aafire2
```
   В результате получаем:
   
![image](https://github.com/user-attachments/assets/a86b9e74-b35e-4a1f-8b36-ecaa67d3e13b)

5. Аналогично для контейнера aafire2:
```
   sudo docker exec -it aafire2 bash
   ping aafire1
```
![image](https://github.com/user-attachments/assets/1ccb5fbc-aa0b-4704-8ed6-1d39166d9902)



**Вывод:** Было создано и настроено два контейнера aafire1 и aafire2, которые были добавлены в одну сеть, а также проверена связь между контейнерами командой ping.
