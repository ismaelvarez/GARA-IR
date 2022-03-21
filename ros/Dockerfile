FROM ros:melodic-ros-base-bionic

# Optris Camera Drivers

RUN apt update && apt install -y apt-utils curl gnupg wget git

#RUN bash -c 'echo -n 1 > /sys/module/uvcvideo/parameters/nodrop'

RUN sudo usermod -a -G video root

RUN apt-get install -y cmake freeglut3-dev libusb-1.0-0-dev libudev-dev libdlib-dev

ADD driver /tmp

RUN dpkg -i /tmp/*

RUN mkdir /root/thermal

ADD config /root/thermal

RUN sh -c 'echo "yaml https://raw.githubusercontent.com/evocortex/optris_drivers/master/libirimager.yaml " > /etc/ros/rosdep/sources.list.d/19-libirimager.list'

RUN ["/bin/bash", "-c", "source /opt/ros/melodic/setup.bash && rosdep update"]

RUN apt-get update

RUN apt-get install -y ipython python-rospkg python-pip

RUN pip install pyyaml

RUN mkdir -p /root/catkin_ws/src

RUN ["/bin/bash", "-c", "source /opt/ros/melodic/setup.bash && cd /root/catkin_ws && catkin_make"]

RUN cd /root/catkin_ws/src && git clone https://github.com/RobotWebTools/web_video_server.git

RUN cd /root/catkin_ws/src && git clone https://github.com/ros-perception/image_pipeline.git && cd image_pipeline && git checkout melodic

RUN ["/bin/bash", "-c", "source /opt/ros/melodic/setup.bash && cd /root/catkin_ws/src && git clone https://github.com/ismaelvarez/optris_drivers"]

RUN cd /root/catkin_ws/src && git clone https://github.com/ros/geometry.git && cd geometry && git checkout melodic-devel

RUN cd /root/catkin_ws/src && git clone https://github.com/ismaelvarez/gara_messages.git

RUN cd /root/catkin_ws/src && git clone https://github.com/ismaelvarez/thermal_processing.git

RUN cd /root/catkin_ws/src && git clone https://github.com/ismaelvarez/gara_infrared_camera.git

RUN ["/bin/bash", "-c", "source /opt/ros/melodic/setup.bash && cd /root/catkin_ws && rosdep install --from-paths src --ignore-src -r -y"]

RUN ["/bin/bash", "-c", "source /opt/ros/melodic/setup.bash && cd /root/catkin_ws && catkin_make"]

ADD config/optris_imager.yaml /tmp/.ros/camera_info/

RUN chmod 777 /tmp/.ros/camera_info/optris_imager.yaml

RUN ir_download_calibration

RUN ["/bin/bash", "-c", "source /opt/ros/melodic/setup.bash && source /root/catkin_ws/devel/setup.bash && rosrun gara_infrared_camera gara_infrared_camera &"]
