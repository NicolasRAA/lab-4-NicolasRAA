# Лабораторная работа №4. Docker

## Оглавление
1. [Цель работы](#цель-работы)
2. [Ход выполнения работы](#ход-выполнения-работы)
   - [Установка Docker](#установка-docker)
   - [Работа с Docker: пример cowsay](#работа-с-docker-пример-cowsay)
   - [Задание: Создание и работа с образом aafire](#задание-создание-и-работа-с-образом-aafire)
3. [Заключение](#заключение)

---

## Цель работы

Установить Docker, создать Docker-образы, запустить контейнеры с приложениями `cowsay` и `aafire`, настроить сеть между контейнерами и протестировать их взаимодействие.

---

## Ход выполнения работы

### Установка Docker

#### 1. **Обновление системы**  
   ```bash
   sudo apt-get update && sudo apt-get upgrade -y
   ```
   ![sudo apt update and upgrade](https://github.com/user-attachments/assets/8d6d1812-049a-4dc4-ad11-e952dc36997c)
   -> Эта команда обновляет список пакетов и устанавливает последние версии всех установленных пакетов. Это важно для обеспечения безопасности и стабильности системы, так как обновления могут содержать исправления ошибок и уязвимостей.

#### 2. Установка Docker
   - Устанавление Docker из официальных репозиториев Ubuntu, коммандой:
   ```bash
   sudo apt-get install -y docker.io
   ```
   ![sudo install docker](https://github.com/user-attachments/assets/347f1c42-ca94-4869-bc1a-de614de9a015)

#### 3. Запуск и проверка версии Docker
   - Команды запускают Docker, добавляют его в автозагрузку и выводят текущую установленную версию.
   ![Verificando la instalacion de docker](https://github.com/user-attachments/assets/6add3d15-9c28-4945-ba6d-83e288950477)
   - Показана версия Docker, что подтверждает успешную установку.

---

### Работа с Docker: пример cowsay

#### 1. Создание `Dockerfile`
   - Я создал `Dockerfile` с помощью команды:
   ![Creando el dockerfile (anterior)](https://github.com/user-attachments/assets/67972bb1-f501-4b07-b6b0-81f690ef1502)
   - Внутри этого файла я указал следующий текст:
   ![Dockerfile](https://github.com/user-attachments/assets/0b10e370-50d4-4fbd-a255-511842c4aa68)
    - Базовый образ: `ubuntu:latest`.  
    - Установка программ `cowsay` и `fortune`.

#### 2. Добавление пользователя в группу Docker
   ```bash
   sudo usermod -aG docker $USER
   ```
   - Эта команда добавляет текущего пользователя в группу Docker, что позволяет запускать команды Docker без необходимости использовать sudo каждый раз.  
   - Затем, я проверил статус Docker:
   ![Dandole admin a docker y verificando su estado](https://github.com/user-attachments/assets/b8660571-fedd-410e-9301-508aaec0f1ab)  
    - В выводе было указано, что Docker активен (`running`) и работает корректно.
   
#### 3. Сборка образа
   - Создан образ с именем `cowsay`.
   ![docker build -t cowsay](https://github.com/user-attachments/assets/0d4e5d2b-1ebd-451c-a93d-6c46875fc43e)  
   - Запуск контейнера с командой `cowsay`: `docker run cowsay /usr/games/cowsay "Moo"`  
   ![docker run cowsay moo](https://github.com/user-attachments/assets/be1f3407-7941-4b48-8396-cd89a03329cd)  
    - Появилась ASCII-картинка коровы с текстом "Moo". ♥  

#### 4. Интерактивный запуск контейнера
   - Затем я запустил контейнер в интерактивном режиме:
   ```bash
   docker run -it cowsay
   ```
   - Внутри контейнера я выполнил команду:
   ![запустить напрямую moo](https://github.com/user-attachments/assets/fd6bf99f-f5d5-45a0-b2fc-f73f030701f8)  
      - Запуск контейнера в интерактивном режиме с флагом `-it` позволяет взаимодействовать с терминалом контейнера. Контейнер будет работать до тех пор, пока я не остановлю его вручную.

#### 5. Просмотр активных контейнеров
   - Я проверил запущенные контейнеры с помощью команды `docker ps`, и там увидел контейнер с образом cowsay, и отобразилась информация о работающих контейнерах.
   ![docker ps](https://github.com/user-attachments/assets/d0d8f022-53cd-43ec-af58-b76d9e7b9e33)  
   - Я остановил контейнер, запущенный с флагом `-it` коммандой `docker stop dc1464eeced2`.
   ![apagando el contenedor](https://github.com/user-attachments/assets/3c353746-d8df-487e-b026-b96d52141667)  
      - В результате выполнения команды `docker ps` больше не было активных контейнеров.  

---

### Задание: Создание и работа с образом aafire

#### 1. Создание `Dockerfile`
   - Я создал директорию и `Dockerfile` для приложения `aafire`:
   ![Creando directorio y dockerfile](https://github.com/user-attachments/assets/bac64f3e-0ba2-4faf-9c7d-93796c7c0237)  
   - Внутри `Dockerfile` я указал следующее:
   ```bash
   FROM ubuntu:20.04
   RUN apt-get update && \
      apt-get install -y libaa-bin iputils-ping && \
      apt-get clean
   CMD ["aafire"]
   ```
   -> Этот `Dockerfile` создает образ на основе Ubuntu 20.04, устанавливает необходимые пакеты для работы `aafire` и утилиту `ping`, а затем задает команду по умолчанию для запуска `aafire`.  
  
#### 2. Сборка Docker-образа
   - Я собрал образ с помощью команды:  
   ![docker build -t aafire](https://github.com/user-attachments/assets/4cdd7e53-0dc0-4a48-b050-bee8b2ab9f4c)  
      - Появились сообщения `Successfully built 1e9a42004f9a` и `Successfully tagged aafire:latest`, то есть образ `aafire` успешно создан.  
      ![Successfully build and tagged](https://github.com/user-attachments/assets/2b104b54-335d-4579-8550-f3ca65e12049)  

#### 3. Запуск контейнера
   - Я запустил контейнер с aafire коммандой:
   ```bash
   docker run -it --name aafire_container -e TERM=xterm aafire
   ```
   ![docker run aafire](https://github.com/user-attachments/assets/95a6709a-bbf0-454c-b446-2564e585798e)
   -> Этот командный запуск создает контейнер с именем `aafire_container`, устанавливает переменную окружения `TERM` (`-e TERM=xterm` обеспечивает корректное отображение ASCII-графики в терминале) и запускает приложение aafire.  
      - Вывод: огня из aafire 🔥  
      ![aafire (fuego)](https://github.com/user-attachments/assets/171c469e-3f3a-4724-962c-8d56febcae54)  
   - Я проверил все контейнеры с помощью команды `docker ps -a`.
     ![docker ps -a](https://github.com/user-attachments/assets/0c9d4405-bf77-4f61-a84e-7708d0cbd9af)  
      - В выводе я увидел свой контейнер aafire.  

---

 > Настройка сети между контейнерами

#### 4. Запуск двух контейнеров одновременно
  - Я запустил два контейнера aafire в разных терминалах:
    - В одном терминале я выполнил команду:
    ```bash
    docker run -it --name aafire_container1 --rm aafire
    ```
    - В другом терминале я выполнил команду:
    ```bash
    docker run -it --name aafire_container2 --rm aafire
    ```

    -> Эти команды создают и запускают два контейнера с именами `aafire_container1` и `aafire_container2`. Флаг `--rm` указывает Docker автоматически удалять контейнер после его остановки. Оба контейнера запускают приложение `aafire` и отображают анимацию огня одновременно.

#### 5. Создание сети Docker и Подключение контейнеров к сети
  - Я создал сеть для контейнеров с помощью команды:
    ```bash
    docker network create myNetwork
    ```

    -> Эта команда создает новую виртуальную сеть Docker, которая позволяет контейнерам общаться друг с другом. Создание сети необходимо для настройки связи между контейнерами.  
  - Я подключил контейнеры к созданной сети:
    ```bash
    docker network connect myNetwork aafire_container1
    docker network connect myNetwork aafire_container2
    ```

    -> Эти команды подключают контейнеры `aafire_container1` и `aafire_container2` к сети `myNetwork`, что позволяет им обмениваться данными и использовать утилиты, такие как `ping`, для проверки соединения.
  - Я проверил настройки созданной сети с помощью команды:
    ```bash
    docker network inspect myNetwork
    ```
    
    - В выводе я увидел, что оба контейнера `aafire_container1` и `aafire_container2` подключены к сети `myNetwork`.

#### 6. Установка утилит внутри контейнеров
  - Я выполнил команду для доступа к контейнеру `aafire_container1`:
    ```bash
    docker exec -it aafire_container1 bash
    ```
    - Внутри контейнера я выполнил команду:
    ```bash
    apt-get update && apt-get install -y iproute2
    ```
    
    -> Эта команда обновляет список пакетов и устанавливает пакет `iproute2`, который необходим для работы с сетевыми интерфейсами и маршрутизацией. Установка этого пакета была необходима для выполнения команды `ping`.  
  - Я выполнил ту же команду для контейнера `aafire_container2`.

#### 7. Проверка соединения между контейнерами
  - Я проверил IP-адреса контейнеров с помощью команд:
    ```bash
    docker exec aafire_container1 ip a
    ```
    - Я увидел IP-адрес контейнера1 `172.17.0.2`.
    ```bash
    docker exec aafire_container2 ip a
    ```
    - Я увидел IP-адрес контейнера2 `172.17.0.3`.
   
    - Я протестировал соединение между контейнерами с помощью команды ping:
    ```bash
    docker exec -it aafire_container1 ping -c 10 172.17.0.3
    ```
    
    - Показано, что *"10 packets transmitted, 10 received"*, что подтверждает успешное соединение между контейнерами.

  - Я выполнил ту же команду для контейнера2:
    ```bash
    docker exec -it aafire_container2 ping -c 10 172.17.0.2
    ```
    
    - Также показано, что *"10 packets transmitted, 10 received"*, что подтверждает успешное соединение в обе стороны.
    
#### 8. Остановка контейнеров
  - Я снова проверил запущенные контейнеры с помощью команды:
    ```bash
    docker ps
    ```
    
    - Я там увидел `aafire_container1` и `aafire_container2`.  
  - В конце я остановил оба контейнера с помощью команды:
    ```bash
    docker stop aafire_container1 aafire_container2
    ```

---

## Заключение
В ходе выполнения лабораторной работы 4 я изучил основы работы с Docker, включая создание и управление контейнерами, а также настройку сетевого взаимодействия между ними. Я научился создавать Dockerfile для сборки образов, запускать контейнеры и устанавливать необходимые пакеты для работы приложений внутри контейнеров.  

Особое внимание было уделено созданию сети для контейнеров, что позволило мне протестировать их взаимодействие с помощью утилиты `ping`. Это продемонстрировало, как контейнеры могут обмениваться данными и работать в одной сети, что является важным аспектом при разработке распределенных приложений.  

В результате выполнения лабораторной работы я получил практические навыки работы с Docker, что является важным шагом в освоении технологий контейнеризации и DevOps. Эти знания будут полезны для дальнейшего изучения и применения в реальных проектах.  

