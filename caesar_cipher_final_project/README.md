# caesar_cipher
Выпускной проект по курсу "DevOps Engineer" от LevelUp <br><br>
<b>= Проект: "Шифр Цезаря" = <br></b> 
<b>= Версия: 1.0 = <br></b> 
<b>Стек технологий:</b> ubuntu / python / html / docker / docker-compose / jenkins /ansible <br> 
<b>Описание работы приложения:</b> Приложение представляет собой веб-сервис по шифрованию любого сообщения состоящего из русских/латинских букв и/или цифр с помощью "шифра Цезаря" с помощью указанного цифрового ключа. Приложение стартует в докер-контейнере и доступно на 80 порту прод-сервера. Cервис по расшифровке доступен на 8081 порту на том же сервере. В качестве оркестрации используется docker-compose. Автобилд-пайплан осуществляется с помощью Jenkins при коммите в репозитории (вебхук).<br>
<b>Cервер с работающим приложением:</b><br>
<br>
<b>Состав репозитория:</b><br>
-app.py # стартует python web-сервер <br>
-index.html # фронтенд приложения <br>
-cgi-bin/form.py # бэкенд приложения - обработка данных форм <br>
-Dockerfile # собирает докер-образ приложения <br>
-docker-compose.yml # деплой приложения в оркестраторе <br>
-Jenkinsfile # скрипт пайплайна по сборке приложения в Jenkins <br>
-ansible_playbook # плейбук для ansible <br>
-screenshots # скриншоты приложения <br>
-README.md # описание приложения и отчет ВКР <br>
<br>
<b>Как пользоваться приложением:</b><br>
<b>Для шифровки:</b><br>
-ввести в соответствующие поля: цифровой ключ (любое число, определяющее сдвиг буквы в алфавите) и сообщение для кодирования<br>
-нажать на кнопку Отправить<br>
-в появившемся окне можно скопировать зашифрованное сообщение<br>
<b>Для расшифровки:</b><br>
-ввести в соответствующие поля: цифровой ключ (которым было зашифровано сообщение) и зашифрованное сообщение<br>
-нажать на кнопку Отправить<br>
-в появившемся окне можно увидеть расшифрованное сообщение<br>
<b>Как развернуть приложение:</b><br>
-на Linux сервере предварительно нужно установить docker, docker-compose (можно воспользоваться ansible плейбуком) <br>
-сделать <b>git clone</b> текущего репозитория<br>
-перейти в каталог caesar_cipher_final_project: <b>cd ./caesar_cipher</b> <br>
-запустить docker-compose командой: <b>docker-compose up -d</b> <br>
-сервисы будут доступны по адресам: http://ваш_ip:80 и http://ваш_ip:8081 соответственно <br>
<br>
<b>Отчет о проделанной работе в рамках ВКР + листинг команд:</b><br>
-в gcp создан инстанс e2-standart-2 c ОС Linux Ubuntu 20.04LTS <br>
-в gcp настройках фаервола открываем порты: 80,8080,8081 <br>
-базовая настройка системы:<br>
<b>sudo su</b><br>
<b>dpkg-reconfigure tzdata</b><br>
<b>apt update -y && apt install mc net-tools -y </b><br> 
-установка docker&docker-compose:<br>
<b>apt install docker.io -y && apt install docker-compose -y</b></br>
-установка JRE (нужно для Jenkins):<br>
<b>apt install default-jre -y</b></br>
-установка Jenkins:<br>
<b>wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add - <br>
sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list' <br>
apt update -y && apt install jenkins <br>
systemctl start jenkins <br></b>
-заходим на веб-интерфейс Jenkins на порте 8080 <br>
<b>cat /var/lib/jenkins/secrets/initialAdminPassword</b> #копируем пароль разблокировки <br>
-в Jenkins создаем пользователя и пароль, устанавливаем базовый набор плагинов <br>
-добавляем пользователя jenkins в группу docker (чтобы мог запускать docker):<br>
<b>usermod -a -G docker jenkins</b><br>
-настройка сокета:<br>
<b>chmod 777 /var/run/docker.sock</b><br>
<b>/etc/init.d/jenkins stop <br>
/etc/init.d/jenkins start </b><br>
-в веб-интерфейсе Jenkins устанавливаем плагины Docker plugin, docker-build-step, Docker pipeline, GitHub Integration Plugin<br>
-создаем Pipeline где указываем наш git-репозиторий, ветку main, путь к Jenkinsfile, а также в настройках сборки: GitHub hook trigger for GITScm polling <br>
-в github в настройках репозитория, ищем раздел Webhooks->Add webhook. Добавляем адрес Jenkins сервера, Content type: applications/json <br>
-запускаем сборку, проверяем работоспособность приложения <br>
-далее пайплайн запускается автоматически при коммите в репозитории<br>
