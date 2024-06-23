# Строим на базе ROS 1 версия noetic-desktop
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
#COPY /requirements.txt /tmp/requirements.txt
# COPY ../requirements.txt /tmp/requirements.txt
#COPY requirements.txt /tmp/requirements.txt
COPY ./requirements.txt /tmp/requirements.txt
#COPY /home/dmitriy/task5/requirements.txt /tmp/requirements.txt
RUN pip install --upgrade pip
RUN pip install -r /tmp/requirements.txt

# Настройка bashrc
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
RUN echo "alias dros='source devel/setup.bash'" >> ~/.bashrc

RUN echo "Сделали"
