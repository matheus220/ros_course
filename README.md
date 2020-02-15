# Programação para robótica - Practical classes
### An introduction to Robot Operating System (ROS)
#### Content based on the book ROS Robot Programming (English) by Pyo, Yoonseoketal et al.

*******
Tabelas de conteúdo 
 1. [Part 1 - Installing ROS](#parte1)
 2. [Part 2 - ROS Commands](#parte2)
 3. [Part 3 - Basic ROS Programming](#parte3)
 4. [Part 4 - 3D Modeling and Simulation](#parte4)
 5. [Part 5 - Mobile Robots](#parte5)
*******

<div id='parte1'/>  

## Part 1 - Installing ROS

Let us install ROS Kinetic. You will be able to install ROS Kinetic without much difficulty by following the instructions below.

It is possible to install ROS Kinetic in various versions of the operating system. You can use any of them, but we recommend version 16.04 to follow this tutorial.

This process has been based on the official installation page, which can be found at http://wiki.ros.org/kinetic/Installation/Ubuntu.

#### Setup your sources.list

We will add the ROS repository address to ros-latest.list.

```sh
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

#### Set up your keys

We will add a public key in order to download the package from the ROS repository with the following command.

Note that the following key may change depending on the situation of the server’s operation, so please refer to the official [Wiki page](http://wiki.ros.org/kinetic/Installation/Ubuntu).

```sh
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

#### Updating the Package Index (recommend)

Now that we have put the ROS repository address in the source list we must perform indexing of the package list again.

```sh
sudo apt-get update && sudo apt-get upgrade -y
```

#### Installing ROS Kinetic Kame

We will install the ROS packages for desktop using the following command. This will include ROS, rqt, RViz, robot-related libraries, simulation, navigation, etc.

```sh
sudo apt-get install ros-kinetic-desktop-full
```

#### Dependencies for building packages

To create and manage your own ROS workspaces, there are various tools and requirements that are distributed separately. To install this tool and other dependencies for building ROS packages, run:

```sh
sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

#### Initializing rosdep

Be sure to initialize rosdep before using ROS. The rosdep is a feature that enhances user convenience by easily installing dependent packages when using or compiling core components of ROS.

```sh
sudo rosdep init && rosdep update
```

#### Load the Environment File

This command imports the environment setting file. Environment variables such as ROS_ROOT, ROS_PACKAGE_PATH, etc. are defined.

```sh
source /opt/ros/kinetic/setup.bash
```

#### Creating and Initializing a Workspace Folder

ROS uses a ROS dedicated build system called catkin. To use this, you need to create and initialize a catkin workspace folder as follows. This configuration only needs to be performed once, unless you create a new workspace folder.

```sh
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/src
catkin_init_workspace
```

Now that we have created the catkin workspace folder, let’s build it. Currently, the catkin workspace only contains the ‘src’ folder and the ‘CMakeLists.txt’ file inside, but as a test use the 'catkin_make' command to build.

```sh
cd ~/catkin_ws/
catkin_make
```

Lastly, we will import the setting file associated with the catkin build system.

```sh
source ~/catkin_ws/devel/setup.bash
```

#### ROS Settings

The following command needs to be executed every time we open a new terminal window in order to apply the settings to current terminal window.

```sh
source /opt/ros/kinetic/setup.bash
source ~/catkin_ws/devel/setup.bash
```

To avoid this repetitive task, we can set it to import a setting file when we open a new terminal.

```sh
nano ~/.bashrc
```

Without modifying any of these settings, scroll down to the very bottom of the 'bashrc' file and append the following lines:

```sh
# Set ROS Kinetic
source /opt/ros/kinetic/setup.bash
source ~/catkin_ws/devel/setup.bash

# Set ROS Network
export ROS_HOSTNAME=xxx.xxx.xxx.xxx
export ROS_MASTER_URI=http://${ROS_HOSTNAME}:11311

# Set ROS alias command
alias cw='cd ~/catkin_ws'
alias cs='cd ~/catkin_ws/src'
alias cm='cd ~/catkin_ws && catkin_make'
```

#### Installing QtCreator

The Integrated Development Environment (IDE) provides the development environment so that the user can perform tasks related to program development, such as coding, debugging, compiling, distribution in a single program. Most developers have at least a couple of their favorite IDEs.

```sh
sudo apt-get install qtcreator
```

```sh
qtcreator
```

<div id='parte2'/>  

## Part 2 - ROS Commands

In order to use ROS properly, we will need to familiarize ourselves with not only the basic Linux commands but also with the ROS specific commands.

 * **ROS Shell Commands** : roscd, rosls, rosed, roscp, rospd, rosd.
 * **ROS Execution Commands** : roscore, rosrun, roslaunch, rosclean.
 * **ROS Information Commands** : rostopic, rosservice, rosnode, rosparam, rosbag, rosmsg, rossrv, rosversion, roswtf.
 * **ROS Catkin Commands** : catkin_create_pkg, catkin_make, catkin_eclipse, catkin_prepare_release, catkin_generate_changelog, catkin_init_workspace, catkin_find.

```sh
roscore
rosrun turtlesim turtlesim_node
rosrun turtlesim turtle_teleop_key
```

<div id='parte3'/>  

## Part 3 - Basic ROS Programming

### Creating and Running Publisher and Subscriber Nodes

#### Creating a Package

```sh
cd ~/catkin_ws/src
catkin_create_pkg ros_tutorials_topic message_generation std_msgs roscpp
```

#### Modifying the Package Configuration File (package.xml)

```sh
cd ros_tutorials_topic
nano package.xml
```

ros_tutorials_topic/package.xml

```xml
<?xml version="1.0"?>
<package>
  <name>ros_tutorials_topic</name>
  <version>0.1.0</version>
  <description>ROS tutorial package to learn the topic</description>
  <license>Apache License 2.0</license>
  <author email="pyo@robotis.com">Yoonseok Pyo</author>
  <maintainer email="pyo@robotis.com">Yoonseok Pyo</maintainer>
  <url type="bugtracker">https://github.com/ROBOTIS-GIT/ros_tutorials/issues</url>
  <url type="repository">https://github.com/ROBOTIS-GIT/ros_tutorials.git</url>
  <url type="website">http://www.robotis.com</url>
  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>roscpp</build_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>message_generation</build_depend>
  <run_depend>roscpp</run_depend>
  <run_depend>std_msgs</run_depend>
  <run_depend>message_runtime</run_depend>
  <export></export>
</package>
```

#### Modifying the Build Configuration File (CMakeLists.txt)

```sh
nano CMakeLists.txt
```

ros_tutorials_topic/CMakeLists.txt

```txt
cmake_minimum_required(VERSION 2.8.3)
project(ros_tutorials_topic)


## A component package required when building the Catkin.
## Has dependency on message_generation, std_msgs, roscpp.
## An error occurs during the build if these packages do not exist.
find_package(catkin REQUIRED COMPONENTS message_generation std_msgs roscpp)


## Declaration Message: MsgTutorial.msg
add_message_files(FILES MsgTutorial.msg)


## an option to configure the dependent message.
## An error occurs duing the build if "std_msgs" is not installed.
generate_messages(DEPENDENCIES std_msgs)


## A Catkin package option that describes the library, the Catkin build dependencies,
## and the system dependent packages.
catkin_package(
  LIBRARIES ros_tutorials_topic
  CATKIN_DEPENDS std_msgs roscpp
)


## Include directory configuration.
include_directories(${catkin_INCLUDE_DIRS})


## Build option for the "topic_publisher" node.
## Configuration of Executable files, target link libraries, and additional dependencies.
add_executable(topic_publisher src/topic_publisher.cpp)
add_dependencies(topic_publisher ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_publisher ${catkin_LIBRARIES})


## Build option for the "topic_subscriber" node.
add_executable(topic_subscriber src/topic_subscriber.cpp)
add_dependencies(topic_subscriber ${${PROJECT_NAME}_EXPORTED_TARGETS}
${catkin_EXPORTED_TARGETS})
target_link_libraries(topic_subscriber ${catkin_LIBRARIES})

```

#### Writing the Message File

The following option is added to the CMakeLists.txt file.

```txt
add_message_files(FILES MsgTutorial.msg)
```

```sh
roscd ros_tutorials_topic
mkdir msg
cd msg
nano MsgTutorial.msg
```

ros_tutorials_topic/msg/MsgTutorial.msg

```txt
time stamp
int32 data
```

#### Writing the Publisher Node

The following option was previously configured in the 'CMakeLists.txt' file to create an executable file.

```txt
add_executable(topic_publisher src/topic_publisher.cpp)
```

```sh
roscd ros_tutorials_topic/src
nano topic_publisher.cpp
```

ros_tutorials_topic/src/topic_publisher.cpp

```cpp
#include "ros/ros.h"                            // ROS Default Header File
#include "ros_tutorials_topic/MsgTutorial.h"    // MsgTutorial Message File Header. The header file is automatically created when building the package.

int main(int argc, char **argv)                 // Node Main Function
{
  ros::init(argc, argv, "topic_publisher");     // Initializes Node Name
  ros::NodeHandle nh;                           // Node handle declaration for communication with ROS system

  // Declare publisher, create publisher 'ros_tutorial_pub' using the 'MsgTutorial'
  // message file from the 'ros_tutorials_topic' package. The topic name is
  // 'ros_tutorial_msg' and the size of the publisher queue is set to 100.
  ros::Publisher ros_tutorial_pub = nh.advertise<ros_tutorials_topic::MsgTutorial>("ros_tutorial_msg", 100);

  // Set the loop period. '10' refers to 10 Hz and the main loop repeats at 0.1 second intervals
  ros::Rate loop_rate(10);

  ros_tutorials_topic::MsgTutorial msg;     // Declares message 'msg' in 'MsgTutorial' message file format
  int count = 0;                            // Variable to be used in message

  while (ros::ok())
  {
    msg.stamp = ros::Time::now();           // Save current time in the stamp of 'msg'
    msg.data  = count;                      // Save the the 'count' value in the data of 'msg'

    ROS_INFO("send msg = %d", msg.stamp.sec);   // Prints the 'stamp.sec' message
    ROS_INFO("send msg = %d", msg.stamp.nsec);  // Prints the 'stamp.nsec' message
    ROS_INFO("send msg = %d", msg.data);        // Prints the 'data' message

    ros_tutorial_pub.publish(msg);          // Publishes 'msg' message

    loop_rate.sleep();                      // Goes to sleep according to the loop rate defined above.

    ++count;                                // Increase count variable by one
  }

  return 0;
}

```

#### Writing the Subscriber Node

The following is an option in the 'CMakeLists.txt' file to generate the executable file.

```txt
add_executable(topic_subscriber src/topic_subscriber.cpp)
```

```sh
roscd ros_tutorials_topic/src
nano topic_subscriber.cpp
```

ros_tutorials_topic/src/topic_subscriber.cpp

```cpp
#include "ros/ros.h"                          // ROS Default Header File
#include "ros_tutorials_topic/MsgTutorial.h"  // MsgTutorial Message File Header. The header file is automatically created when building the package.

// Message callback function. This is a function is called when a topic
// message named 'ros_tutorial_msg' is received. As an input message,
// the 'MsgTutorial' message of the 'ros_tutorials_topic' package is received.
void msgCallback(const ros_tutorials_topic::MsgTutorial::ConstPtr& msg)
{
  ROS_INFO("recieve msg = %d", msg->stamp.sec);   // Prints the 'stamp.sec' message
  ROS_INFO("recieve msg = %d", msg->stamp.nsec);  // Prints the 'stamp.nsec' message
  ROS_INFO("recieve msg = %d", msg->data);        // Prints the 'data' message
}

int main(int argc, char **argv)                         // Node Main Function
{
  ros::init(argc, argv, "topic_subscriber");            // Initializes Node Name

  ros::NodeHandle nh;                                   // Node handle declaration for communication with ROS system

  // Declares subscriber. Create subscriber 'ros_tutorial_sub' using the 'MsgTutorial'
  // message file from the 'ros_tutorials_topic' package. The topic name is
  // 'ros_tutorial_msg' and the size of the publisher queue is set to 100.
  ros::Subscriber ros_tutorial_sub = nh.subscribe("ros_tutorial_msg", 100, msgCallback);

  // A function for calling a callback function, waiting for a message to be
  // received, and executing a callback function when it is received.
  ros::spin();

  return 0;
}
```

#### Building a Node

Run catkin build.

```sh
cd ~/catkin_ws
catkin_make
```

#### Running the Publisher

Run the 'roscore' and the publisher.

```sh
roscore
rosrun ros_tutorials_topic topic_publisher
```

Verify that the 'ros_tutorial_msg' topic is running.

```sh
rostopic echo /ros_tutorial_msg
```

#### Running the Subscriber

Run the subscriber.

```sh
rosrun ros_tutorials_topic topic_subscriber
```


#### Checking the Communication Status of the Running Nodes

Let’s check the communication status of executed nodes using the 'rqt' tools.

```sh
rqt
rqt_graph
```

### Creating and Running Service Servers and Client Nodes


#### Creating a Package

```sh
cd ~/catkin_ws/src
catkin_create_pkg ros_tutorials_service message_generation std_msgs roscpp
```

#### Modifying the Package Configuration File (package.xml)

```sh
cd ros_tutorials_service
nano package.xml
```

ros_tutorials_service/package.xml

```xml
<?xml version="1.0"?>
<package>
  <name>ros_tutorials_service</name>
  <version>0.1.0</version>
  <description>ROS turtorial package to learn the service</description>
  <license>Apache License 2.0</license>
  <author email="pyo@robotis.com">Yoonseok Pyo</author>
  <maintainer email="pyo@robotis.com">Yoonseok Pyo</maintainer>
  <url type="bugtracker">https://github.com/ROBOTIS-GIT/ros_tutorials/issues</url>
  <url type="repository">https://github.com/ROBOTIS-GIT/ros_tutorials.git</url>
  <url type="website">http://www.robotis.com</url>
  <buildtool_depend>catkin</buildtool_depend>
  <build_depend>roscpp</build_depend>
  <build_depend>std_msgs</build_depend>
  <build_depend>message_generation</build_depend>
  <run_depend>roscpp</run_depend>
  <run_depend>std_msgs</run_depend>
  <run_depend>message_runtime</run_depend>
  <export></export>
</package>
```

#### Modifying the Build Configuration File (CMakeLists.txt)

```sh
nano CMakeLists.txt
```

ros_tutorials_service/CMakeLists.txt

```txt
cmake_minimum_required(VERSION 2.8.3)
project(ros_tutorials_service)

find_package(catkin REQUIRED COMPONENTS message_generation std_msgs roscpp)

add_service_files(FILES SrvTutorial.srv)
generate_messages(DEPENDENCIES std_msgs)

catkin_package(
  LIBRARIES ros_tutorials_service
  CATKIN_DEPENDS std_msgs roscpp
)

include_directories(${catkin_INCLUDE_DIRS})

add_executable(service_server src/service_server.cpp)
add_dependencies(service_server ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(service_server ${catkin_LIBRARIES})

add_executable(service_client src/service_client.cpp)
add_dependencies(service_client ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(service_client ${catkin_LIBRARIES})
```

#### Writing the Service File

The following option is added to the CMakeLists.txt file.

```txt
add_service_files(FILES SrvTutorial.srv)
```

```sh
roscd ros_tutorials_service
mkdir srv
cd srv
nano SrvTutorial.srv
```

ros_tutorials_service/srv/SrvTutorial.srv

```txt
int64 a
int64 b
---
int64 result
```

#### Writing the Service Server Node

The following option is added to the 'CMakeLists.txt' file.

```txt
add_executable(service_server src/service_server.cpp)
```

```sh
roscd ros_tutorials_service/src
nano service_server.cpp
```

ros_tutorials_service/src/service_server.cpp

```cpp
#include "ros/ros.h"                          // ROS Default Header File
#include "ros_tutorials_service/SrvTutorial.h"// SrvTutorial Service File Header (Automatically created after build)

// The below process is performed when there is a service request
// The service request is declared as 'req', and the service response is declared as 'res'
bool calculation(ros_tutorials_service::SrvTutorial::Request &req,
                 ros_tutorials_service::SrvTutorial::Response &res)
{
  // The service name is 'ros_tutorial_srv' and it will call 'calculation' function upon the service request.
  res.result = req.a + req.b;

  // Displays 'a' and 'b' values used in the service request and
  // the 'result' value corresponding to the service response
  ROS_INFO("request: x=%ld, y=%ld", (long int)req.a, (long int)req.b);
  ROS_INFO("sending back response: %ld", (long int)res.result);

  return true;
}

int main(int argc, char **argv)              // Node Main Function
{
  ros::init(argc, argv, "service_server");   // Initializes Node Name
  ros::NodeHandle nh;                        // Node handle declaration

  // Declare service server 'ros_tutorials_service_server'
  // using the 'SrvTutorial' service file in the 'ros_tutorials_service' package.
  // The service name is 'ros_tutorial_srv' and it will call 'calculation' function
  // upon the service request.
  ros::ServiceServer ros_tutorials_service_server = nh.advertiseService("ros_tutorial_srv", calculation);

  ROS_INFO("ready srv server!");

  ros::spin();    // Wait for the service request

  return 0;
}
```

#### Writing the Service Client Node

The following is an option in the 'CMakeLists.txt' file to generate the executable file.

```txt
add_executable(service_client src/service_client.cpp)
```

```sh
roscd ros_tutorials_service/src
nano service_client.cpp
```

ros_tutorials_service/src/service_client.cpp

```cpp
#include "ros/ros.h"                          // ROS Default Header File
#include "ros_tutorials_service/SrvTutorial.h"// SrvTutorial Service File Header (Automatically created after build)
#include <cstdlib>                            // Library for using the "atoll" function

int main(int argc, char **argv)               // Node Main Function
{
  ros::init(argc, argv, "service_client");    // Initializes Node Name

  if (argc != 3)  // Input value error handling
  {
    ROS_INFO("cmd : rosrun ros_tutorials_service service_client arg0 arg1");
    ROS_INFO("arg0: double number, arg1: double number");
    return 1;
  }

  ros::NodeHandle nh;       // Node handle declaration for communication with ROS system

  // Declares service client 'ros_tutorials_service_client'
  // using the 'SrvTutorial' service file in the 'ros_tutorials_service' package.
  // The service name is 'ros_tutorial_srv'
  ros::ServiceClient ros_tutorials_service_client = nh.serviceClient<ros_tutorials_service::SrvTutorial>("ros_tutorial_srv");

  // Declares the 'srv' service that uses the 'SrvTutorial' service file
  ros_tutorials_service::SrvTutorial srv;

  // Parameters entered when the node is executed as a service request value are stored at 'a' and 'b'
  srv.request.a = atoll(argv[1]);
  srv.request.b = atoll(argv[2]);

  // Request the service. If the request is accepted, display the response value
  if (ros_tutorials_service_client.call(srv))
  {
    ROS_INFO("send srv, srv.Request.a and b: %ld, %ld", (long int)srv.request.a, (long int)srv.request.b);
    ROS_INFO("receive srv, srv.Response.result: %ld", (long int)srv.response.result);
  }
  else
  {
    ROS_ERROR("Failed to call service ros_tutorial_srv");
    return 1;
  }
  return 0;
}
```

#### Building a Node

Go to the catkin folder and run the catkin build.

```sh
cd ~/catkin_ws && catkin_make
```

#### Running the Service Server

Run the 'roscore' and the service server.

```sh
roscore
rosrun ros_tutorials_service service_server
```

#### Running the Service Client

Run the service client.

```sh
rosrun ros_tutorials_service service_client 2 3
```

#### Using the rosservice call Command

```sh
rosservice call /ros_tutorial_srv 10 2
```

#### Using the GUI Tool, Service Caller

```sh
rqt
```

Next, select [Plugins] → [Services] → [Service Caller]

### Using Parameters

Let’s modify the ‘service_server.cpp’ source in the service server and the client node to use parameters to perform arithmetic operations, rather than just adding two values entered as service request.

#### Writing the Node using Parameters

Modify the ‘service_server.cpp’ source in the following order.

```sh
roscd ros_tutorials_service/src
nano service_server.cpp
```

ros_tutorials_service/src/service_server.cpp

```cpp
#include "ros/ros.h"                            // ROS Default Header File
#include "ros_tutorials_parameter/SrvTutorial.h"// action Library Header File

#define PLUS            1   // Addition
#define MINUS           2   // Subtraction
#define MULTIPLICATION  3   // Multiplication
#define DIVISION        4   // Division

int g_operator = PLUS;

// The process below is performed if there is a service request
// The service request is declared as 'req', and the service response is declared as 'res'
bool calculation(ros_tutorials_parameter::SrvTutorial::Request &req,
                 ros_tutorials_parameter::SrvTutorial::Response &res)
{
  // The operator will be selected according to the parameter value and calculate 'a' and 'b',
  // which were received upon the service request.
  // The result is stored as the Response value.
  switch(g_operator)
  {
    case PLUS:
         res.result = req.a + req.b; break;
    case MINUS:
         res.result = req.a - req.b; break;
    case MULTIPLICATION:
         res.result = req.a * req.b; break;
    case DIVISION:
         if(req.b == 0)
         {
           res.result = 0; break;
         }
         else
         {
           res.result = req.a / req.b; break;
         }
    default:
         res.result = req.a + req.b; break;
  }

  // Displays the values of 'a' and 'b' used in the service request, and the 'result' value
  // corresponding to the service response.
  ROS_INFO("request: x=%ld, y=%ld", (long int)req.a, (long int)req.b);
  ROS_INFO("sending back response: [%ld]", (long int)res.result);

  return true;
}

int main(int argc, char **argv)             // Node Main Function
{
  ros::init(argc, argv, "service_server");  // Initializes Node Name
  ros::NodeHandle nh;                       // Node handle declaration

  nh.setParam("calculation_method", PLUS);  // Reset Parameter Settings

  // Declare service server 'service_server' using the 'SrvTutorial' service file
  // in the 'ros_tutorials_service' package. The service name is 'ros_tutorial_srv' and
  // it is set to execute a 'calculation' function when a service is requested.
  ros::ServiceServer ros_tutorials_service_server = nh.advertiseService("ros_tutorial_srv", calculation);

  ROS_INFO("ready srv server!");

  ros::Rate r(10);  // 10 hz

  while (ros::ok())
  {
    nh.getParam("calculation_method", g_operator);  // Select the operator according to the value received from the parameter.
    ros::spinOnce();  // Callback function process routine
    r.sleep();        // Sleep for routine iteration
  }

  return 0;
}

```

#### Setting Parameters

The following code sets the parameter of 'calculation_method' to 'PLUS'.

```sh
nh.setParam("calculation_method", PLUS);
```

#### Reading Parameters

The following gets the parameter value from 'calculation_method' and sets it as the value of 'g_operator'.

```sh
nh.getParam("calculation_method", g_operator);
```

#### Building and Running Nodes

Rebuild the service server node in the ‘ros_tutorials_service’ package with the following command.

```sh
cd ~/catkin_ws && catkin_make
```

Run the 'service_server' node of the 'ros_tutorials_service' package.

```sh
roscore
rosrun ros_tutorials_service service_server
```

#### Displaying Parameter Lists

The 'rosparam list' command displays a list of parameters currently used in the ROS network.

```sh
rosparam list
```

#### Example of Using Parameters

Set the parameters according to the following command, and verify that the service processing has changed while requesting the same service each time.

```sh
rosservice call /ros_tutorial_srv 10 5    # Input variables a and b for arithmetic operation

rosparam set /calculation_method 2        # Subtraction
rosservice call /ros_tutorial_srv 10 5

rosparam set /calculation_method 3        # Multiplication
rosservice call /ros_tutorial_srv 10 5

rosparam set /calculation_method 4        # Division
rosservice call /ros_tutorial_srv 10 5
```

### Using roslaunch

The 'rosrun' is a command that executes just one node, and 'roslaunch' can run more than one node. Other features of the 'roslaunch' command include the ability to modify parameters of the package, rename the node name, the ROS_ROOT and ROS_PACKAGE_PAPATION_PATH settings, and change environment variables.

#### Using the roslaunch

To learn how to use roslaunch, rename the 'topic_publisher' and 'topic_subscriber' nodes previously created.

```sh
roscd ros_tutorials_topic
mkdir launch
cd launch
nano union.launch
```

union.launch

```xml
<launch>
  <node pkg="ros_tutorials_topic" type="topic_publisher" name="topic_publisher1"/>
  <node pkg="ros_tutorials_topic" type="topic_subscriber" name="topic_subscriber1"/>
  <node pkg="ros_tutorials_topic" type="topic_publisher" name="topic_publisher2"/>
  <node pkg="ros_tutorials_topic" type="topic_subscriber" name="topic_subscriber2"/>
</launch>
```

```sh
roslaunch ros_tutorials_topic union.launch --screen
```

Let’s take a look at the nodes currently running with the following command.

```sh
rosnode list

rqt_graph
```

Let’s modify the ‘union.launch’ file that we created earlier.

```sh
roscd ros_tutorials_topic/launch
nano union.launch
```

```xml
<launch>
  <group ns="ns1">
    <node pkg="ros_tutorials_topic" type="topic_publisher" name="topic_publisher"/>
    <node pkg="ros_tutorials_topic" type="topic_subscriber" name="topic_subscriber"/>
  </group>
  <group ns="ns2">
    <node pkg="ros_tutorials_topic" type="topic_publisher" name="topic_publisher"/>
    <node pkg="ros_tutorials_topic" type="topic_subscriber" name="topic_subscriber"/>
  </group>
</launch>
```

Once again, visualize the status of the connection and message transmission between nodes using 'rqt_graph'.


<div id='parte4'/>  

## Part 4 - 3D Modeling and Simulation

### Install the required ROS packages

```sh
sudo apt-get install ros-kinetic-ros-controllers ros-kinetic-gazebo* ros-kinetic-moveit* ros-kinetic-dynamixel-sdk ros-kinetic-dynamixel-workbench-toolbox ros-kinetic-robotis-math ros-kinetic-industrial-core
```

### Manipulator Modeling

```sh
cd ~/catkin_ws/src
catkin_create_pkg testbot_description urdf
cd testbot_description
mkdir urdf
cd urdf
nano testbot.urdf
```

Enter the following URDF example.

```xml
<?xml version="1.0" ?>
<robot name="testbot">

  <material name="black">
    <color rgba="0.0 0.0 0.0 1.0"/>
  </material>
  <material name="orange">
    <color rgba="1.0 0.4 0.0 1.0"/>
  </material>

  <link name="base"/>

  <joint name="fixed" type="fixed">
    <parent link="base"/>
    <child link="link1"/>
  </joint>

  <link name="link1">
    <collision>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 0.5"/>
      </geometry>
    </collision>
    <visual>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 0.5"/>
      </geometry>
      <material name="black"/>
    </visual>
    <inertial>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <mass value="1"/>
      <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
    </inertial>
  </link>

  <joint name="joint1" type="revolute">
    <parent link="link1"/>
    <child link="link2"/>
    <origin xyz="0 0 0.5" rpy="0 0 0"/>
    <axis xyz="0 0 1"/>
    <limit effort="30" lower="-2.617" upper="2.617" velocity="1.571"/>
  </joint>

  <link name="link2">
    <collision>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 0.5"/>
      </geometry>
    </collision>
    <visual>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 0.5"/>
      </geometry>
      <material name="orange"/>
    </visual>
    <inertial>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <mass value="1"/>
      <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
    </inertial>
  </link>

  <joint name="joint2" type="revolute">
    <parent link="link2"/>
    <child link="link3"/>
    <origin xyz="0 0 0.5" rpy="0 0 0"/>
    <axis xyz="0 1 0"/>
    <limit effort="30" lower="-2.617" upper="2.617" velocity="1.571"/>
  </joint>

  <link name="link3">
    <collision>
      <origin xyz="0 0 0.5" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 1"/>
      </geometry>
    </collision>
    <visual>
      <origin xyz="0 0 0.5" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 1"/>
      </geometry>
      <material name="black"/>
    </visual>
    <inertial>
      <origin xyz="0 0 0.5" rpy="0 0 0"/>
      <mass value="1"/>
      <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
    </inertial>
  </link>

  <joint name="joint3" type="revolute">
    <parent link="link3"/>
    <child link="link4"/>
    <origin xyz="0 0 1.0" rpy="0 0 0"/>
    <axis xyz="0 1 0"/>
    <limit effort="30" lower="-2.617" upper="2.617" velocity="1.571"/>
  </joint>

  <link name="link4">
    <collision>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 0.5"/>
      </geometry>
    </collision>
    <visual>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <geometry>
        <box size="0.1 0.1 0.5"/>
      </geometry>
      <material name="orange"/>
    </visual>
    <inertial>
      <origin xyz="0 0 0.25" rpy="0 0 0"/>
      <mass value="1"/>
      <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="1.0" iyz="0.0" izz="1.0"/>
    </inertial>
  </link>

</robot>
```

Check if the file is grammatically and logically correct.

```sh
check_urdf testbot.urdf
```

Relationship between URDF and link.

```sh
urdf_to_graphiz testbot.urdf
```

Check the robot model using RViz.

```sh
cd ~/catkin_ws/src/testbot_description
mkdir launch
cd launch
nano testbot.launch
```

```xml
<launch>
  <arg name="model" default="$(find testbot_description)/urdf/testbot.urdf" />
  <arg name="gui" default="True" />

  <param name="robot_description" textfile="$(arg model)" />
  <param name="use_gui" value="$(arg gui)"/>

  <node pkg="joint_state_publisher" type="joint_state_publisher" name="joint_state_publisher"/>
  <node pkg="robot_state_publisher" type="state_publisher" name="robot_state_publisher"/>
</launch>
```

```sh
roslaunch testbot_description testbot.launch
rviz
```

### OpenManipulator

Download the source code from GitHub on OpenManipulator.

```sh
cd ~/catkin_ws/src
git clone https://github.com/ROBOTIS-GIT/open_manipulator.git
cd ~/catkin_ws && catkin_make
```

The OpenManipulator folder copied from Github.

```sh
cd ~/catkin_ws/src/open_manipulator
ls

# Arduino                           → Library for Arduino
# open_manipulator                  → MetaPackage
# open_manipulator_description      → Modeling package
# open_manipulator_dynamixel_ctrl   → Dynamixel control package
# open_manipulator_gazebo           → Gazebo package
# open_manipulator_moveit           → MoveIt! package
# open_manipulator_msgs             → Message package
# open_manipulator_position_ctrl    → Position control package
# open_manipulator_with_tb3         → OpenManipulator and TurtleBot3 package
```

Open the urdf folder and the launch folder and look at the configuration.

```sh
roscd open_manipulator_description/urdf
ls

# materials.xacro                       → Material info
# open_manipulator_chain.xacro          → Manipulator modeling
# open_manipulator_chain.gazebo.xacro   → Manipulator Gazebo modeling
```

```sh
roscd open_manipulator_description/launch
ls

# open_manipulator.rviz                 → RViz configuration file
# open_manipulator_chain_ctrl.launch    → File to execute manipulator state info Publisher node
# open_manipulator_chain_rviz.launch    → File to execute manipulator modeling info visualization node
```

Run the launch file to visualize the completed URDF file in RViz and move the joint using the 'joint_state_publisher' GUI.

```sh
roslaunch open_manipulator_description open_manipulator_chain_rviz.launch
```

### Gazebo Setting

The tags for the Gazebo simulation are stored in the 'open_manipulator_chain.gazebo.xacro' file. Let’s take a look.

```sh
roscd open_manipulator_description/urdf
nano open_manipulator_chain.gazebo.xacro
```

Open these Gazebo launch files and see which nodes are included.

```sh
roscd open_manipulator_gazebo/launch
nano open_manipulator_gazebo.launch
```

See the OpenManipulator Chain in the Gazebo simulation space.

```sh
roslaunch open_manipulator_gazebo open_manipulator_gazebo.launch
```

Check the topic list.

```sh
rostopic list
```

Let’s move the robot using the following command.

```sh
rostopic pub /open_manipulator_chain/joint2_position/command std_msgs/Float64 "data: 1.0" --once
```

<div id='parte5'/>  

## Part 5 - Mobile Robots

Install TurtleBot3 packages and dependent packages

```sh
sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation
```

```sh
cd ~/catkin_ws/src/
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
git clone https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/catkin_ws && catkin_make
```

Launch the virtual robot

```sh
roscore
export TURTLEBOT3_MODEL=burger
roslaunch turtlebot3_fake turtlebot3_fake.launch
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

### Odometry and TF

Visualized tf topics in rqt_tf_tree

```sh
rosrun rqt_tf_tree rqt_tf_tree
```

### Gazebo Simulator

Empty World

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_empty_world.launch
```

Remote Control in Turtlebot3 World

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_world.launch
roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```

TurtleBot3 automatically moving and avoiding obstacles on Gazebo

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_simulation.launch
```

Visualized virtual LiDAR data and camera image in RViz

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_gazebo_rviz.launch
```

### Virtual SLAM and Navigation

#### Virtual SLAM Execution Procedure

Launch Gazebo

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

Launch SLAM

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_slam turtlebot3_slam.launch
```

Execute RViz

```sh
export TURTLEBOT3_MODEL=waffle
rosrun rviz rviz -d `rospack find turtlebot3_slam`/rviz/turtlebot3_slam.rviz
```

Remotely Control TurtleBot3

```sh
roslaunch turtlebot3_teleop turtlebot3_teleop_key.Launch
```

Save the Map

```sh
rosrun map_server map_saver -f ~/map
```

#### Virtual Navigation Execution Procedure

Execute Gazebo

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_gazebo turtlebot3_world.launch
```

Execute Navigation

```sh
export TURTLEBOT3_MODEL=waffle
roslaunch turtlebot3_navigation turtlebot3_navigation.launch map_file:=$HOME/map.yaml
```

Execute RViz and Set Destination

```sh
export TURTLEBOT3_MODEL=waffle
rosrun rviz rviz -d `rospack find turtlebot3_navigation`/rviz/turtlebot3_nav.rviz
```
