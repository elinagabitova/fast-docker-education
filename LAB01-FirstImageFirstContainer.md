## LAB-01: Первый обрзаи первый контейнер

Этот сценарий лабораторной работы покажет, как создать образ из Dockerfile, а также как запустить docker контейнер для этого образа.

### Поехали!

- Создайте директорию (“example”) на своем ПК.
- Создайте файл (“index.py”) в директории “example” как в коде под этим шагом (Flask-приложение, который выведет "Hello World!" в браузере).

```
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():
    return "Hello World!"
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=int("5000"), debug=True)
```

- Создайте “Dockerfile” (файл без расширения) в директории “example” как в коде под этим шагом (он копирует в /app директорию в контейнере, запускает requirements.txt, открывает 5000 порт и запускает index.py).

```
FROM python:alpine3.7
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
EXPOSE 5000
CMD python ./index.py
```

- Создайте файл “requirements.txt” и скопируйте внутрь него строчку под этим шагом (в нем только “flask”).

```
flask
```

- В данный момент у нас есть 3 файла в директории “example”.

 ![image](https://user-images.githubusercontent.com/10358317/113274100-99299900-92dd-11eb-9431-a1839dd0b280.png)



- Запустите в консоли заранее зайдя в ней в директорию “example” “docker build --tag my-python-app .” ( Создает образ из Dockerfile и дает образу имя “my-python-app”. Все работает оп порядку - первым скачивается образ python, что запускается на Alpine, и теперь он готов к запуску “CMD python ./index.py” во время запуска контейнера).

```
docker build --tag my-python-app .
```

![image](https://user-images.githubusercontent.com/10358317/113274060-8c0caa00-92dd-11eb-8ac3-285d1552c54d.png)


- Запустите в консоли “docker run --name python-app -p 5000:5000 my-python-app” (запускает контейнер из созданного образа “my-python-app”, имя контейнера - “python-app”, хостовой порт 5000 привязывается к порту контейнера 5000).

```
docker run --name python-app -p 5000:5000 my-python-app
```

![image](https://user-images.githubusercontent.com/10358317/113274079-92028b00-92dd-11eb-9902-da00b07602bb.png)


- Откройте браузер (http://127.0.0.1:5000) для просмотра результата. Вы создали первый Docker образ и запустили первый Docker контейнер. Поздравляю! 😊 

 ![image](https://user-images.githubusercontent.com/10358317/113274597-2967de00-92de-11eb-8a76-1b1adde27f3a.png)

 

