# Задание 1:
* **1 создать сервис, состоящий из 2 различных контейнеров: 1 - веб, 2 - БД**
* **2 далее необходимо создать 3 сервиса в каждом окружении (dev, prod, lab)**
* **3 по итогу на каждой ноде должно быть по 2 работающих контейнера**
* **4 выводы зафиксировать**

# Задание 2*:
* **1 нужно создать 2 ДК-файла, в которых будут описываться сервисы**
* **2 повторить задание 1 для двух окружений: lab, dev**
* **3 обязательно проверить и зафиксировать результаты, чтобы можно было выслать преподавателю для проверк**

# Выполнение

## Создаем файл конфигурации docker-compose.yaml
* **nano docker-compose.yaml**
  
![docker-compose yaml](https://github.com/user-attachments/assets/c64b1ba5-c8b0-4d93-b0e6-c87eaff8e6f2)

## Запускаем
* **docker compose up -d**
## Останавливаем
* **docker compose down**

## Инициализация кластера Docker Swarm.
* **docker swarm init**
## Вводим команду для связи во второй виртуальной машине
* **docker swarm join --token SWMTKN-1-5tsb9tsra8av1b7xl6m7k5hjmdaf7wftidxlqftsyeimgc2kel-495rg3l5hianb341s835j3pmi 10.0.2.15:2377**
## Смотрим ноды
* **docker node ls**
![docker-swarm](https://github.com/user-attachments/assets/b7993413-b481-49b3-9989-dffe16b288b0)


## Создаем сеть
* **docker network create --driver overlay test-network --attachable**
## Создаем 3 контейнера в режиме репликации
* **docker service create --name mariadb_service --replicas 3 -e MYSQL_ROOT_PASSWORD=12345 -p 3306:3306 --network test-network mariadb:10.10.2**
## Смотрим запущенные контейнеры
* **docker service ps mariadb_service** 
![контейнеры в docker service](https://github.com/user-attachments/assets/2280eb74-1f3c-44aa-8eb2-f1d0453b7b46)

## Вешаем метку
* **docker node update --label-add env=prod gb-VirtualBox**
## Запускаем на заданной метке
* **docker service create --name nginx_servece --replicas 3 -p 8080:8080 --network test-network --constraint node.label.env==prod nginx:latest**
## Проверка
* **docker service ps nginx_servece**
![docker-swarm_service](https://github.com/user-attachments/assets/90ebed9d-01ee-4031-9033-1e36bd961473)
![DB_plus_adminer](https://github.com/user-attachments/assets/2a52cfe4-c04e-43e2-bdbe-a27903f1469b)




 

  
