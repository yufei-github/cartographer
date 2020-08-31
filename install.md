Compiling Cartographer ROS
System Requirements
The Cartographer ROS requirements are the same as the ones from Cartographer.

The following ROS distributions are currently supported:
Kinetic
Melodic

ROS Melodic installation instructions

Configure your Ubuntu repositories
Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse." You can follow the Ubuntu guide for instructions on doing this.

Setup your sources.list
Setup your computer to accept software from packages.ros.org.


sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

Mirrors

Source Debs are also available

Set up your keys
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
If you experience issues connecting to the keyserver, you can try substituting hkp://pgp.mit.edu:80 or hkp://keyserver.ubuntu.com:80 in the previous command.

Alternatively, you can use curl instead of the apt-key command, which can be helpful if you are behind a proxy server:

curl -sSL 'http://keyserver.ubuntu.com/pks/lookup?op=get&search=0xC1CF6E31E6BADE8868B172B4F42ED6FBAB17C654' | sudo apt-key add -
Installation
First, make sure your Debian package index is up-to-date:

sudo apt update
There are many different libraries and tools in ROS. We provided four default configurations to get you started. You can also install ROS packages individually.

In case of problems with the next step, you can use following repositories instead of the ones mentioned above ros-shadow-fixed

Desktop-Full Install: (Recommended) : ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators and 2D/3D perception

sudo apt install ros-melodic-desktop-full
or click here

Desktop Install: ROS, rqt, rviz, and robot-generic libraries

sudo apt install ros-melodic-desktop
or click here

ROS-Base: (Bare Bones) ROS package, build, and communication libraries. No GUI tools.

sudo apt install ros-melodic-ros-base
or click here

Individual Package: You can also install a specific ROS package (replace underscores with dashes of the package name):

sudo apt install ros-melodic-PACKAGE
e.g.
sudo apt install ros-melodic-slam-gmapping
To find available packages, use:

apt search ros-melodic

Environment setup
It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:


echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
If you have more than one ROS distribution installed, ~/.bashrc must only source the setup.bash for the version you are currently using.

If you just want to change the environment of your current shell, instead of the above you can type:


source /opt/ros/melodic/setup.bash
If you use zsh instead of bash you need to run the following commands to set up your shell:


echo "source /opt/ros/melodic/setup.zsh" >> ~/.zshrc
source ~/.zshrc
Dependencies for building packages
Up to now you have installed what you need to run the core ROS packages. To create and manage your own ROS workspaces, there are various tools and requirements that are distributed separately. For example, rosinstall is a frequently used command-line tool that enables you to easily download many source trees for ROS packages with one command.

To install this tool and other dependencies for building ROS packages, run:


sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
Initialize rosdep
Before you can use many ROS tools, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. If you have not yet installed rosdep, do so as follows.


sudo apt install python-rosdep
With the following, you can initialize rosdep.


sudo rosdep init
rosdep update

Building & Installation
In order to build Cartographer ROS, we recommend using wstool and rosdep. For faster builds, we also recommend using Ninja.

sudo apt-get update
sudo apt-get install -y python-wstool python-rosdep ninja-build
Create a new cartographer_ros workspace in ‘catkin_ws’.

mkdir catkin_ws
cd catkin_ws
wstool init src
wstool merge -t src https://raw.githubusercontent.com/cartographer-project/cartographer_ros/master/cartographer_ros.rosinstall
wstool update -t src
Install cartographer_ros’ dependencies (proto3 and deb packages). The command ‘sudo rosdep init’ will print an error if you have already executed it since installing ROS. This error can be ignored.

src/cartographer/scripts/install_proto3.sh
sudo rosdep init
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
If you build cartographer from master. Change/remove the version in the cartographer_ros.rosinstall.

Additionally, uninstall the ros abseil-cpp using

Build and install.

catkin_make_isolated --install --use-ninja
