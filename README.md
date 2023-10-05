# Fast-Docker в переводе на русский язык
Этот репозиторий - перевод [Fast-docker](https://github.com/omerbsezer/Fast-Docker#motivation) на русский язык с комментариями и дополнениями. Он поможет начинающим DevOps/DevSecOps/IT-специалистам в понимании docker: Dockerfile, Image, Container, Commands, Volumes, Docker-Compose, Networks, Swarm, Stack. Так же в нем есть лабораторные, описывающие возможные сценарии использования docker для быстрого освоения материала.

**Теги:** Docker-Image, Dockerfile, Containerization, Docker-Compose, Docker-Volume, Docker-Network, Docker-Swarm, Service, Cheatsheet.

# Лабораторные:
- [LAB-01: Создание первого docker-образа и контейнера с помощью DockerFile](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB01-FirstImageFirstContainer.md)
- [LAB-02: Привязка томов (Volumes) к различным контейнерам](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB02-DockerVolume.md)
    - [LAB-02.1: Привязка каталогов к контейнеру](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB02-DockerVolume.md#app_mount)
- [LAB-03: Docker-Compose - создание 2 разных контейнеров: контейнер WordPress в связке с контейнером MySql](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB03-DockerCompose.md)
- [LAB-04: Создание кластера Docker Swarm With на 5 nodes, используя PlayWithDocker : 3 x WordPress контейнера and 1 x MySql контенйер с помощью Docker-Compose файла](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB04-DockerStackService.md)
- [LAB-05: Запуск Docker Free в локальном Registry, изменение тега образа, отправка образов в локальный Registry, извлечение образов из локального Registry and удаление образов из локального Registry](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB05-DockerLocalRegistry.md)
- [LAB-06: Перенос файлов между хост-компьютером и Docker-контейнером](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB06-DockerTransferringContent.md)
- [LAB-07: Создание Docker-контейнера с использованием Dockerfile для сборки C++ в Ubuntu 18.04](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB07-DockerfileForLinuxC++Build.md)
- [LAB-08: Создание Docker-контейнера с использованием Dockerfile для сборки C++ в Windows](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB08-DockerfileForWindowsC++Build.md)
- [LAB-09: Конфигурация Docker (Proxy, Registry)](https://github.com/omerbsezer/Fast-Docker/blob/main/LAB09-DockerConfiguration.md)
- [Часто используемые команды Docker (cheetsheet)](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerCommandCheatSheet.md)

# Содержание
- [Мотивация](#мотивация)
    - [Потребность в Docker](#потребность)
    - [Преимущества](#преимущества)
    - [Проблемы, которые Docker не решает](#проблемы)
- [Что такое Docker?](#whatisdocker)
    - [Архитектура](#architecture)
    - [Установка](#installation)
    - [Движок Docker (Deamon, REST API, CLI)](#engine)
    - [Docker Registry и Docker Hub](#registry)
    - [Структура комманд Docker](#command)
    - [Docker контейнер](#container)
    - [Docker тома/Привязка Mounts](#volume)
    - [Сеть в Docker](#network)
    - [Логирование в Docker](#log)
    - [Статистика Docker/Лимит на CPU/память](#stats)
    - [Docker Environment Variables](#variables)
    - [Docker File](#file)
    - [Docker Образ](#image)
    - [Docker Compose](#compose)
    - [Docker Swarm](#swarm)
    - [Docker Stack / Docker Service](#stack)
- [Play With Docker](#playwithdocker)
- [Часто используемые команды Docker](#cheatsheet)
- [Другие ползеные ресурсы про Docker ](#resource)
- [References](#references)

## Мотивация <a name="мотивация"></a>
Почему нам следует использовать Docker? «Docker изменил способ создания и доставки приложений. Он произвел полную революцию в мире контейнеризации."

### Потребность в Docker <a name="потребность"></a>
- Установка всех зависимостей, подготовка нового окружения для ПО (каждый раз установка среды для тестирования требует много времени) 
- Потребность в запуске приложений на разных платформах (Ubuntu, Windows, Raspberry Pi).
    - Вопрос: Что если, он не запуститься на другой ОС?
- Интеграционное тестирование CI/CD: Мы можем провести модульное тестирование и тестирование компонентов с помощью Jenkins. What если тестирование интеграционное? 
    - Расширение цепочки: Jenkins- Docker образ - Docker контейнер - Автоматическое тестирование
- Легко ли переносить наши программные продукты на разные ПК? (особенно на этапе разработки и тестирования)
- Разработка, тестирование и поддержка кода как одного монолитного приложения может оказаться проблематичным, если приложению требуется больше функций/сервисов. Требуется преобразовать одно большое монолитное приложение в микросервисы.

### Преимущества <a name="преимущества"></a>

- Не нуждается в установке зависимостей
- Готов к запуску на разных ОС, разных платформах
- Обеспечивает согласованную среду
- Обеспечивает более эффективное использование системных ресурсов
- Простота в использовании и обслуживании
- Эффективное использование системных ресурсов
- Изоляция компонентов ПО
- Обеспечивает более быстрые циклы доставки программного обеспечения
- Контейнеры обеспечивают мнгновенную возможность переноса приложений.
- Позволяет разработчикам легко упаковывать, отправлять и запускать любое приложение в виде легкого, портативного и независимого контейнера.
- Микросервисная архитектура (Монолитные приложения в Микросервисную архитектуру, например [Cloud Native App](https://docs.microsoft.com/en-us/dotnet/architecture/cloud-native/introduction))

### Проблемы, которые Docker не решает <a name="проблемы"></a>
- Docker НЕ решает ваши проблемы с безопасностью
- Docker НЕ превращает приложения волшебным образом в микросервисы.
- Docker не заменяет виртуальные машины


## Что такое Docker?  <a name="whatisdocker"></a>
- Docker — это инструмент, который сокращает разрыв между этапами разработки и развертывания в цикле разработки программного обеспечения.
- Docker похож на виртуальную машину, но у него больше функций, чем у виртуальных машин (без ядра, только небольшие приложения и файловые системы)
    - В ядре Linux (2000s) добавлены следующие 2 функции:
        - Пространства имен (Namespaces): изоляция процессов.
        - Контрольные группы: изоляция и ограничение использования ресурсов (ЦП, память) для каждого процесса. 
- Без Docker каждая ВМ потребляет 30% ресурсов (Память, ЦП)

![image](https://user-images.githubusercontent.com/10358317/113183089-ef51fa00-9253-11eb-9ade-771905ce8ebd.png) (Ref: Docker.com)

### Architecture  <a name="architecture"></a>

![image](https://user-images.githubusercontent.com/10358317/113183210-0db7f580-9254-11eb-9716-0de635f3cbdf.png) (Ref: docs.docker.com)


### Installation  <a name="installation"></a>

- Linux: Docker Engine
    - https://docs.docker.com/engine/install/ubuntu/
- Windows: Docker Desktop for Windows 
    - WSL: Windows Subsystem for Linux, 
    - WSL2: virtualization through a highly optimized subset of Hyper-V to run the kernel and distributions, better than WLS.
        - https://docs.docker.com/docker-for-windows/install/
- Mac-OS: Docker Desktop for Mac
    - https://docs.docker.com/docker-for-mac/install/

### Docker Engine (Deamon, REST API, CLI)  <a name="engine"></a>
- There are mainly 3 components in the Docker Engine:
    - Server is the docker daemon named docker daemon. Creates and manages docker images, containers, networks, etc.
    - Rest API instructs docker daemon what to do.
    - Command Line Interface (CLI) is the client used to enter docker commands.

![image](https://user-images.githubusercontent.com/10358317/113183406-45bf3880-9254-11eb-8d13-e68c83f3d349.png) (Ref: Docker.com)

### Docker Registry and Docker Hub  <a name="registry"></a>

- https://hub.docker.com/

![image](https://user-images.githubusercontent.com/10358317/113183434-4eb00a00-9254-11eb-9275-9b1ccf705d5b.png) 

[App: Running Docker Free Local Registry, Tagging Container, Pushing to Local Registry, Pulling From Local Registry and Deleting Images from Local Registry](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerLocalRegistry.md)

### Docker Command Structure  <a name="command"></a>

- docker [ManagementCommand] [Command] 

```
docker container ls -a
docker image ls
docker volume ls
docker network ls
docker container rm -f [containerName or containerID]
```

![image](https://user-images.githubusercontent.com/10358317/113183615-81f29900-9254-11eb-8695-360680931866.png)

![image](https://user-images.githubusercontent.com/10358317/113183721-9fbffe00-9254-11eb-9841-79bf037db34a.png)

### Docker Container  <a name="container"></a>

![image](https://user-images.githubusercontent.com/10358317/113183556-730be680-9254-11eb-8bdd-84c5cf5b86c6.png) (Ref: docker-handbook-borosan)

- When we create the container from the image, in every container, there is an application that is set to run by default app. 
    - When this app runs, the container runs.
    - When this default app finishes/stops, the container stops. 
- There could be more than one app in docker image (such as: sh, ls, basic commands)
- When the Docker container is started, it is allowed that a single application is configured to run automatically.

```
docker container run --name mywebserver -d -p 80:80 -v test:/usr/share/nginx/html nginx
docker container ls -a
docker image pull alpine
docker image push alpine
docker image build -t hello . (run this command where “Dockerfile” is)
(PS: image file name MUST be “Dockerfile”, no extension)
docker save -o hello.tar test/hello
docker load -i <path to docker image tar file>
docker load -i .\hello.tar
```

Goto: [App: Creating First Docker Image and Container using Docker File](https://github.com/omerbsezer/Fast-Docker/blob/main/FirstImageFirstContainer.md)

#### Docker Container: Life Cycle

![image](https://user-images.githubusercontent.com/10358317/113186436-f67b0700-9257-11eb-9b2e-41ccf056e88b.png) (Ref: life-cycle-medium)

```
e.g. [imageName]=alpine, busybox, nginx, ubuntu, etc.
docker image pull [imageName]
docker container run [imageName]
docker container start [containerId or containerName]
docker container stop [containerId or containerName]
docker container pause [containerId or containerName]
docker container unpause [containerId or containerName]
```

#### Docker Container: Union File System  <a name="container-filesystem"></a>
- Images are read only (R/O).
- When containers are created, new read-write (R/W) thin layer is created.

![image](https://user-images.githubusercontent.com/10358317/113183883-d8f86e00-9254-11eb-994b-30c17fe9429b.png) (Ref: docs.docker.com)

### Docker Volumes: Why Volumes needed?
- Containers do not save the changes/logs when erased if there is not any binding to volume/mount. 
- For persistence, volumes/mounts MUST be used. 
- e.g. Creating a log file in the container. When the container is deleted, the log file also deleted with the container. So volumes/binding mounts MUST be used to provide persistence!

![image](https://user-images.githubusercontent.com/10358317/113184189-2d035280-9255-11eb-9409-578ad1f2bd4b.png) (Ref: udemy-course:adan-zye-docker)

### Docker Volumes/Bind Mounts  <a name="volume"></a>

- Volumes and binding mounts must be used for saving logs, output files, and input files.
- When volumes bind to the directory in the container, this directory and volume are synchronized.

```
docker volume create [volumeName]
docker volume create test
docker container run --name [containerName] -v [volumeName]:[pathInContainer] [imageName]
docker container run --name c1 -v test:/app alpine
```
Goto: [App: Binding Volume to the Different Containers](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerVolume.md)

#### Bind Mount
```
docker container run --name [containerName] -v [pathInHost]:[pathInContainer] [imageName]
docker container run --name c1 -v C:\test:/app alpine
```
![image](https://user-images.githubusercontent.com/10358317/113184347-57eda680-9255-11eb-811c-9f55efd11deb.png) (Ref: Docker.com)

Goto: [App: Binding Mount to Container](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerVolume.md#app_mount)
Goto: [App: Transferring Content between Host PC and Docker Container](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerTransferringContent.md)

### Docker Network <a name="network"></a>
- Docker containers work like VMs.
- Every Docker container has network connections
- Docker Network Drivers:
    - None
    - Bridge
    - Host
    - Macvlan
    - Overlay
    
#### Docker Network: Bridge
- Default Network Driver: Bridge (--net bridge)

```
docker network create [networkName]
docker network create bridge1
docker container run --name [containerName] --net [networkName] [imageName] 
docker container run --name c1 --net bridge1 alpine sh
docker network inspect bridge1
docker container run --name c2 --net bridge1 alpine sh
docker network connect bridge1 c2
docker network inspect bridge1
docker network disconnect bridge1 c2
```

- Creating a new network using customized network parameters:

```
docker network create --driver=bridge --subnet=10.10.0.0/16 --ip-range=10.10.10.0/24 --gateway=10.10.10.10 newbridge
```

![image](https://user-images.githubusercontent.com/10358317/113184949-1b6e7a80-9256-11eb-9a0c-fe5c62404a06.png) (Ref: Docker.com)

#### Docker Network: Host
- Containers reach host network interfaces (--net host)

```
docker container run --name [containerName] --net [networkName] [imageName] 
docker container run --name c1 --net host alpine sh
```

![image](https://user-images.githubusercontent.com/10358317/113185061-43f67480-9256-11eb-9b94-83735ce980ce.png) (Ref: Docker.com)

#### Docker Network: MacVlan
- Each Container has its own MAC interface (--net macvlan)

![image](https://user-images.githubusercontent.com/10358317/113185105-52dd2700-9256-11eb-84f2-ef1880eb4f4c.png) (Ref: Docker.com)

#### Docker Network: Overlay 
- Containers that work on different PCs/hosts can work as the same network (--net overlay)

![image](https://user-images.githubusercontent.com/10358317/113185192-6e483200-9256-11eb-8cb4-d8aa170d1a1e.png) (Ref: Docker.com)

#### Port Mapping/Publish:
- Mapping Host PC's port to container port:
 
 ```
-p [hostPort]:[containerPort], --publish [hostPort]:[containerPort] e.g. -p 8080:80, -p 80:80
docker container run --name mywebserver -d -p 80:80 nginx
```

### Docker Log  <a name="log"></a>

- Docker Logs show /dev/stdout, /dev/stderror

```
docker logs --details [containerName]
```

![image](https://user-images.githubusercontent.com/10358317/113289697-d8151a00-92f0-11eb-86e6-6280c4bf2e77.png)


### Docker Stats/Memory-CPU Limitations  <a name="stats"></a>

![image](https://user-images.githubusercontent.com/10358317/113289735-e9f6bd00-92f0-11eb-940b-13113a5a5da2.png)

![image](https://user-images.githubusercontent.com/10358317/113289755-efec9e00-92f0-11eb-9f49-333a4608c523.png)

![image](https://user-images.githubusercontent.com/10358317/113289773-f5e27f00-92f0-11eb-8c4f-2db75f17baeb.png)


### Docker Environment Variables  <a name="variables"></a>

![image](https://user-images.githubusercontent.com/10358317/113289974-40fc9200-92f1-11eb-9f12-1125ec32eabf.png)


### Docker File  <a name="file"></a>

![image](https://user-images.githubusercontent.com/10358317/113185932-54f3b580-9257-11eb-9f50-0d18512a0c40.png)

Goto: [App: Creating Docker Container using Dockerfile to Build C++ on Ubuntu18.04](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerfileForLinuxC%2B%2BBuild.md)

Goto: [App: Creating Docker Container using Dockerfile to Build C++ on Windows](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerfileForWindowsC%2B%2BBuild.md)

#### Sample Docker Files

```
FROM python:alpine3.7
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 5000
CMD python ./index.py
```

```
FROM ubuntu:18.04
RUN apt-get update -y
RUN apt-get install default-jre -y
WORKDIR /myapp
COPY /myapp .
CMD ["java","hello"]
```

- Multistage Docker File (Creating temporary container):
    - In the example, JDK (Java Development Kit) based temporary image (~440MB) container is created for compilation.
    - Compiled files are copied into JRE (Java Runtime Environment) based image (~145MB). Finally, we have only JRE based image.
```
COPY --from=<stage 1> stage1/src stage2/destination
```
- In the example below, 'compiler' is 'stage1'.
```
FROM mcr.microsoft.com/java/jdk:8-zulu-alpine AS compiler
COPY /myapp /usr/src/myapp
WORKDIR /usr/src/myapp
RUN javac hello.java

FROM mcr.microsoft.com/java/jre:8-zulu-alpine 
WORKDIR /myapp
COPY --from=compiler /usr/src/myapp .
CMD ["java", "hello"]
```

### Docker Image  <a name="image"></a>

- Create Image using Dockerfile
```
docker image build -t hello . (run this command where “Dockerfile” is)
(PS: image file name MUST be “Dockerfile”, no extension)
docker image pull [imageName]
docker image push [imageName]
docker image tag [imageOldName] [imageNewName]
(PS: If you want to push DockerHub, [imageNewName]=[username]/[imageName]:[version])
docker save -o hello.tar test/hello
docker load -i <path to docker image tar file>
docker load -i .\hello.tar
```

![image](https://user-images.githubusercontent.com/10358317/113186047-748ade00-9257-11eb-9c1c-1604d53523e8.png)

Goto: [App: Creating First Docker Image and Container using Docker File](https://github.com/omerbsezer/Fast-Docker/blob/main/FirstImageFirstContainer.md)

### Docker Compose  <a name="compose"></a>
- Define and run multi-container applications with Docker.
- Easy to create Docker components using one file: Docker-Compose file
- It is a YAML file that defines components: 
    - Services, 
    - Volumes, 
    - Networks, 
    - Secrets
- Sample "docker-compose.yml" file: 

```
version: "3.8"

services:
  mydatabase:
    image: mysql:5.7
    restart: always
    volumes: 
      - mydata:/var/lib/mysql
    environment: 
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - mynet
  mywordpress:
    image: wordpress:latest
    depends_on: 
      - mydatabase
    restart: always
    ports:
      - "80:80"
      - "443:443"
    environment: 
      WORDPRESS_DB_HOST: mydatabase:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    networks:
      - mynet
volumes:
  mydata: {}
networks:
  mynet:
    driver: bridge
```

- After saving the file as "docker-compose.yml", run the following commands where the docker-compose file is, to create containers, volumes, networks:

```
docker-compose up -d
docker-compose down
```
Goto: [App: Docker-Compose File - Creating 2 Different Containers:  WordPress Container depends on MySql Container](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerCompose.md)

### Docker Swarm  <a name="swarm"></a>

One of the Container Orchestration tool: 
- Automating and scheduling the 
    - deployment, 
    - management, 
    - scaling, and
    - networking of containers
- Container Orchestration tools:
    - Docker Swarm,
    - Kubernetes,
    - Mesos

![image](https://user-images.githubusercontent.com/10358317/113186661-3b06a280-9258-11eb-9bb8-3ad38d3c55fb.png) (Ref: udemy-course:adan-zye-docker)

### Docker Stack / Docker Service  <a name="stack"></a>
- With Docker Stack, multiple services can be created with one file.
- It is like a Docker-Compose file but it has more features than a Docker-compose file: update_config, replicas.
- But it is running on when Docker Swarm mode is activated.
- Network must be 'overlay'.

#### Creating, Listing, Inspecting
```
docker service create --name testservice --replicas=5 -p 8080:80 nginx
docker service ps testservice (listing running containers on which nodes)
docker service inspect testservice
```
#### Scaling
```
docker service scale testservice=10 (scaling up the containers to 10 replicas)
```
#### Updating
```
docker service update --detach --update-delay 5s --update-parallelism 2 --image nginx:v2 testservice (previous state: testservice created, now updating)
docker service update --help (to see the parameters of update)
```
#### Rollbacking
```
docker service rollback --detach testservice (rollbacking to previous state)
```

![image](https://user-images.githubusercontent.com/10358317/113303356-4a8df600-9301-11eb-9114-38872ca01f29.png)

Goto: [App: Creating Docker Swarm Cluster With 5 PCs using PlayWithDocker : 3 x WordPress Containers and 1 x MySql Container using Docker-Compose File](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerStackService.md)

## Play With Docker  <a name="playwithdocker"></a>

- https://labs.play-with-docker.com/

![image](https://user-images.githubusercontent.com/10358317/113187037-ae101900-9258-11eb-9789-781ca2f6a464.png)

## Docker Commands Cheatsheet <a name="cheatsheet"></a>

Goto: [Docker Commands Cheatsheet](https://github.com/omerbsezer/Fast-Docker/blob/main/DockerCommandCheatSheet.md)

## Other Useful Resources Related Docker  <a name="resource"></a>
- Original Docker Document: https://docs.docker.com/get-started/
- Cheatsheet: https://github.com/wsargent/docker-cheat-sheet
- Workshop: https://dockerlabs.collabnix.com/workshop/docker/
- All-in-one Docker Image for Deep Learning: https://github.com/floydhub/dl-docker
- Various Dockerfiles for Different Purposes: https://github.com/jessfraz/dockerfiles
- Docker Tutorial for Beginners [FULL COURSE in 3 Hours]- Youtube: https://www.youtube.com/watch?v=3c-iBn73dDE&t=6831s
- Docker and Kubernetes Tutorial | Full Course [2021] - Youtube: https://www.youtube.com/watch?v=bhBSlnQcq2k&t=3088s

## References  <a name="references"></a>
- [Docker.com](https://www.docker.com/)
- [docs.docker.com](https://docs.docker.com/get-started/overview/)
- [docker-handbook-borosan](https://borosan.gitbook.io/docker-handbook/docker-images)
- [life-cycle-medium](https://medium.com/future-vision/docker-lifecycle-tutorial-and-quickstart-guide-c5fd5b987e0d)
- [Infoworld](https://www.infoworld.com/article/3310941/why-you-should-use-docker-and-containers.html)
- [ItNext](https://itnext.io/getting-started-with-docker-facts-you-should-know-d000e5815598)
- [udemy-course:adan-zye-docker](https://www.udemy.com/course/adan-zye-docker/)
