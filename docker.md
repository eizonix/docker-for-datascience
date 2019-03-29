README
    открываем терминал view - Terminal
    Пишем - docker ps

    docker run -p 8888:8888 j:17aba6048f44
    
Как попасть внутрь контейнера

    docker exec -it <mycontainer> bash
    вставляем
    beff1f708e85

    docker exec -it beff1f708e85 bash
    
    вошли в доккер

Вставлять файлы в работающий контейнер

    docker cp wine.data beff1f708e85:/home/jovyan/wine.data
    
Стопаем

    docker stop beff1f708e85

Запускаем

    docker run -p 8888:8888 jupyter/scipy-notebook:17aba6048f44

Присобачиваем папку синхронизации

     docker run -v /c/Users/klarovla/Documents/docker:/home/jovyan/ -p 8888:8888 jupyter/scipy-notebook:17aba6048f44

Запсукаемся с dockerfile

    создаем dockerfile вписываем там 
    
    FROM jupyter/scipy-notebook:17aba6048f44
    RUN pip install xgboost

    запускаем что то типо такого 
    docker build .

    Выдает контейнер a0b1ef927917

    Соответственно запускаем с нахванием этого контейнера 

    docker run -v /c/Users/klarovla/Documents/docker:/home/jovyan/ -p 8888:8888 a0b1ef927917

Можно контейнеру дать имя во время билда

    docker build -t my_notebook .

    Теперь его зовут my_notebook

    Стартуем его 
    docker run -v /c/Users/klarovla/Documents/docker:/home/jovyan/ -p 8888:8888 my_notebook

Для работы с оркестром всяких разных сервсиов можно забилдить все разом через docker-compose

    docker-compose up

Запускаем еще один сервис, ну давай postgre

    version: '3'
    services: 
    jupyter:
        build:
        context: .
        dockerfile: dockerfile
        volumes:
        - ./:/home/jovyan/
        ports:
        - "8888:8888"
    db:
        image: postgres
        restart: always
        ports: 
        - "5432:5432"

    Посмотрим на postgre

        открываем jupyter и импортируем 
        !pip install psycopg2
        import psycopg2
        conn_string = "host='http://192.168.99.100' dbname='postgres' user='postgres'"

        conn = psycopg2.connect(conn_string)

        cursor = conn.cursor()
    
        # execute our Query
        cursor.execute("SELECT * FROM my_table")
        - ошибка самой таблицы... все хорошо


    пишем на postgre dataframe

Сохранять данные в сервисе напрмер в postgres

    добавляем в compose

                version: '3'
                services: 
                jupyter:
                    build:
                    context: .
                    dockerfile: dockerfile
                    volumes:
                    - ./:/home/jovyan/
                    ports:
                    - "8888:8888"
                db:
                    image: postgres
                    restart: always
                    volumes: 
                    - pgdata:/var/lib/postgresql/data
                    ports: 
                    - "5432:5432"
                volumes: 
                    pgdata:
