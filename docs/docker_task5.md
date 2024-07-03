# Отчёт по заданию №5.7
    <h2 align="center">Отчет по практической работе №5.7</h3>
    1. <b>Установка Docker Engine и Docker Compose:</b>
    <p>
    <a href="https://docs.docker.com/engine/install/ubuntu/">Официальная страница установки Docker Engine</a>
    <p>
    Есть несколько способов установки Engine, ниже приведен способ установки с помощью репозитория докера, скорее всего взятый с pdf-файла, прилагаемый к видеоуроку Романа.
    <p>
    <font color="green">sudo apt-get update</font>
    <p>
        Устанавливаем программы, ключ. На официальной странице приложение gnupg не устанавливается.
    <p>
       <font color="green">sudo apt-get install ca-certificates curl</font> <font color=red>gnupg</font>
    <p>
    <font color="green">sudo install -m 0755 -d /etc/apt/keyrings
    <p>
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
    <p>
    sudo chmod a+r /etc/apt/keyrings/docker.asc</font>
    <p>
    <font color="green">echo \<br>
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \<br>
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \<br>
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    <p>
       sudo apt-get update</font>
    <p>Устанавливаю пакеты Docker:<p>
    <font color="green">sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin</font><p>
    Docker Engine установил.
    
    <p>Чтобы не выполнять каждый раз команду docker через sudo:
    <p>
        <font color="green">sudo groupadd docker</font>
    <p>
    <font color="green">sudo usermod -aG docker $USER</font>
    <p>
    <font color="green">newgrp docker</font>
    </p>
    
    <p><b>Установка Docker Compose:</b>
    <p>
    Compose, я устанавливал два раза. В последний раз, как устанавливал: <a href="https://github.com/docker/compose/releases">на этой странице</a>
    посмотрел последнюю версию (v2.27.1), скопировал и вставил в команду номер версии (красным цветом, что я вставил):
    <p>
    <font color="green">sudo curl -L "https://github.com/docker/compose/releases/download/</font><font color=red>v2.27.1</font><font color=green>/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose</font><p>
    Второй и более понятный способ: 
    <a href="https://github.com/docker/compose/releases">страница релизов</a> &rarr; <a href="https://github.com/docker/compose/tags">Tags</a>  &rarr;  Downloads.  Копируем путь, вставляем в команду, между кавычками:<p>
        <font color="green">sudo curl -L
            "</font><font color=red>https://github.com/docker/compose/releases/download/v2.27.2/docker-compose-linux-x86_64</font><font color=green>" -o /usr/local/bin/docker-compose</font></p>
    <p>Третий способ из <a href="https://docs.docker.com/compose/install/linux/#install-using-the-repository">официальной страницы</a>, самый простой:
    <p>
        <font color=green>sudo apt-get update<p>
        sudo apt-get install docker-compose-plugin</font>
    <p>
    Далее:
    <p><font color=green>sudo chmod +x /usr/local/bin/docker-compose</font></p>
    <img src="../images/version.png">
    <p><font color=red>Docker CLI, как я понимаю, почти не отличается от обычного терминала, поэтому его не обязательно устанавливать.</font>

    <p>
    2. <b>Разработка простой программы:</b><p>
    task5 - корневой каталог для этого задания. Внутри task5 создал каталог scripts для всех скриптов.
    <p>
        <font color="green">mkdir scripts</font><p>
        Баш скрипт:<p>
        <font color="green">#!/bin/bash<br>
        echo "Скрипт1"</font>
    </p>
    На мой взгляд можно было бы потратить 1-2 урока на python или просто pdf-инструкцию, чтобы писать простейшие скрипты. Зато на Git зачем-то потратили целый модуль, хотя помоему к программированию никакого отношения не имеет. Да и Docker, я так и не понял зачем он нужен.

<p><b>Репозиторий:</b><br>Создал репозиторий <a href="https://github.com/DmitriyZubkov41/task5">https://github.com/DmitriyZubkov41/task5</a>.
</p>
<p>3. <b>Создание образа для программы:</b><br>
Меня заинтересовал проект Антона Писаренко - <a href="https://github.com/AntonSHBK/mobile_robot_base_ros1">Простая имитационная модель мобильного робота в ROS и Gazebo</a>. Dockerfile на 99% состоит из его dockerfile.
</p>
<p>
Внутри каталога task5 создаём еще один каталог docker для всех докер-файлов.
<p>
    <a href="https://github.com/DmitriyZubkov41/task5/blob/main/docker/Dockerfile">Код Dockerfile:</a><p>
<code><pre>
    # Строим на базе образа ROS 1 версия noetic-desktop
    # Эта версия - последняя поддерживаемая в ubuntu 20.04
    FROM osrf/ros:noetic-desktop
    
    # set environment variables
    ENV PYTHONDONTWRITEBYTECODE 1 # Предотвращает создание .pyc файлов
    ENV PYTHONUNBUFFERED 1        # Предотвращает буферизацию вывода Python, улучшая логирование
    
    #Этот блок вроде предназначен для тех кто имеет в компьютере графический чипсет от NVIDIA
    # Поэтому думаю можно все закомментировать
    # nvidia-container-runtime
    #ENV NVIDIA_VISIBLE_DEVICES \
    #   ${NVIDIA_VISIBLE_DEVICES:-all}
    #ENV NVIDIA_DRIVER_CAPABILITIES \
    #   ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics
    
    # set work directory
    #RUN mkdir /workspace/src/mobile_robot_base_ros1 -p
    WORKDIR /home/dmitriy/task5
    
    # Additional commands (installing dependencies)
    RUN apt-get update && \
        apt-get install -y \
        curl \                
        wget \                
        python3-pip \         
        python3-tk            # Пакеты длля tkinter
    
    # Gazebo 11
    RUN sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
    RUN wget https://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
    RUN sudo apt-get install -y libgazebo11-dev 
    # RUN sudo apt-get install -y ros-noetic-gazebo-ros
    # RUN sudo apt-get install -y gazebo11
    
    # Дополнительные пакеты Gazebo для работы с ROS
    # RUN curl -sSL http://get.gazebosim.org | sh
    RUN apt-get update && \
        apt-get install -y \
    #     gazebo11 \
    #     libgazebo11-dev 
        ros-noetic-gazebo-ros \
        ros-noetic-gazebo-ros-pkgs \
        ros-noetic-gazebo-ros-control
    
    # Установка дополнительных пакетов ROS для управления и работы с роботами
    RUN apt-get update && \
        apt-get install -y \
        ros-noetic-control-toolbox \
        ros-noetic-realtime-tools \
        ros-noetic-ros-controllers \
        ros-noetic-xacro \
        python3-wstool \
        ros-noetic-tf-conversions \
        ros-noetic-kdl-parser \
        liburdfdom-tools
    
    # Установка инструментов для разработки
    RUN apt-get update && \
        apt-get install -y \
        nano \
        git \
        tmux \
        tree 
    
    
    # Установка зависимостей Python
    COPY ./requirements.txt /tmp/requirements.txt
    RUN pip install --upgrade pip
    RUN pip install -r /tmp/requirements.txt
    
    # Настройка bashrc
    RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
    RUN echo "alias dros='source devel/setup.bash'" >> ~/.bashrc
    
    RUN echo "Сделали"
    </pre></code>
</p>
Берём образ ROS1 из docker hub, устанавливаем Gazebo 11 согласно официальному мануалу. Непонятно откуда взялся список дополнительных пакетов ros. Помучился с указанием пути к requirements.txt в инструкции COPY. Странно.<br>
С инструкцией WORKDIR, думал имеет какое-то отношение к файловой системе хоста, сейчас бы написал <b>task5</b>.<br> 
Далее в task5:<br>
    <font color="green">touch requirements.txt<br>
        nano requirements.txt
    </font>
</p>
Собираем образ rosgazebo:<br>
<font color=green>dmitriy@945G-M3:~/task5$ docker build -t rosgazebo -f docker/Dockerfile .</font><br>
Непонятно для чего точка стоит в конце, ведь указал путь к dockerfile, но без этой точки команда не работает.
</p>
<p>
4. Запуск и тестирование программы в Docker-контейнере:<p>
<p>
<img src="../images/script.sh.png">
</p>
5. Docker Compose
<p>
Создал docker-compose.yml на основе docker-compose.yml Антона, запустил его, ничего интересного. Решил свой написать. Поскольку compose - многоконтейнерное приложение, то решил запустить 3 контейнера, каждый в своём окне через tmux и чтобы на каждом выполнился автоматом скрипт и сохранилась командная строка контейнера. Такие были мои хотелки.<br>
Если попробовать выполнить мои желания в одном контейнере командой docker run:
<ol>
<li><font color="green">docker run -v $HOME/task5:/home/dmitriy/task5 -it rosgazebo /bin/bash && bash scripts/script.sh</font><p>
<img src="../images/dockerrun1.png">
<br>Тут в результате (контейнер и при выходе - выполнение скрипта) мне всё непонятно. Если считать, что как только команда выполнилась, то контейнер умирает, то результат на мой взгляд должен быть таким: только и сразу выполнение скрипта.

<li><font color="green">docker run -v $HOME/task5:/home/dmitriy/task5 -it rosgazebo bash scripts/script.sh && /bin/bash</font><p><img src="../images/dockerrun2.png"><br>
Здесь на мой взгляд должно быть то, что я хочу: выполнение скрипта и по команде /bin/bash сохранение контейнера.

<li><font color="green">docker run -v $HOME/task5:/home/dmitriy/task5 -it rosgazebo /bin/bash && bash scripts/script.sh && /bin/bash</font><p><img src="../images/dockerrun3.png">
</ol>
<p>
    Больше недели экспериментировал с командами run compose, файлами и по отдельности выполнять контейнер из docker-compose.yml командой docker run -rm name_konteyner. Но в идеале никак не получается.
<p><b>Итоговый код docker-compose.yml:</b><br>
<code><pre>
    version: '4'
    services:
      Kont1:
        image: rosgazebo
        volumes:
          - $HOME/task5:/home/dmitriy/task5
        stdin_open: true # docker run -i
        tty: true        # docker run -t
        command: sh -c "bash scripts/script.sh" #скрипт выполняется, но контейнера нет.
      Kont2:
        image: rosgazebo
        volumes:
          - $HOME/task5:/home/dmitriy/task5
        stdin_open: true # docker run -i
        tty: true        # docker run -t
        command: sh -c "bash scripts/script2.sh"
</pre></code>
<p>
<font color="green">cd docker<br>
docker-compose build<br>
docker-compose up</font>
<p>
    <img src="../images/dockercomposeup1.png">
