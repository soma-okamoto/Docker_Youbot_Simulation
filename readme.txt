

-------------------------------------------------------
docker build --no-cache -t youbot_ros .

$env:DISPLAY="host.docker.internal:0.0"
docker rm -f youbot_ros
docker run --gpus all -d --name youbot_ros `
  -e DISPLAY=$env:DISPLAY `
  -e LIBGL_ALWAYS_SOFTWARE="1" `
  -e QT_X11_NO_MITSHM="1" `
  -e MESA_LOADER_DRIVER_OVERRIDE="llvmpipe" `
  youbot_ros tail -f /dev/null

docker exec -it youbot_ros bash


export ROS_MASTER_URI=http://ros_dev:11311
export ROS_HOSTNAME=youbot_ros   # or  ROS_IP=<自コンテナのIP>

source /opt/ros/noetic/setup.bash
source /root/catkin_ws/devel/setup.bash
---------------------------------------------------------

rosrun gazebo_ros gazebo

source /opt/ros/noetic/setup.bash
source /root/catkin_ws/devel/setup.bash
grep -rl 'xacro.py' ~/catkin_ws/src | grep '\.launch' | xargs sed -i 's#$(find xacro)/xacro.py#$(find xacro)/xacro#g'
roslaunch youbot_gazebo_robot youbot.launch

rosrun rviz rviz

base_link

------------------------------------------
docker network create rosnet

docker network connect rosnet ros_dev
docker network connect rosnet youbot_ros

-----------------XLaunchsetting----------
Display Number =0
（Native OpenGL）
accell

XLaunchを起動⇒Dockerrun