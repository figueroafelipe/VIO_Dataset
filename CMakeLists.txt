cmake_minimum_required(VERSION 2.8.3)
project(piksi_multi_rtk_ros)

## Find catkin macros and libraries
find_package(catkin REQUIRED
  COMPONENTS
    geometry_msgs
    piksi_rtk_msgs
    sensor_msgs
    roslib
    rospy
    roscpp
    std_msgs
    cv_bridge
    image_transport
    message_generation
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS
#  LIBRARIES
  CATKIN_DEPENDS
    geometry_msgs
    sensor_msgs
    piksi_rtk_msgs
    roslib
    rospy
    cv_bridge
    image_transport
    message_runtime
#  DEPENDS
)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
catkin_python_setup()

#############
## Install ##
#############
 catkin_install_python(PROGRAMS bin/piksi_multi bin/geodetic_survey
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY cfg launch log_surveys rosbag_record_scripts
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)














include_directories(
  ${catkin_INCLUDE_DIRS}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread")

find_package(OpenCV REQUIRED)

include_directories(include 
${catkin_INCLUDE_DIRS}  
${OpenCV_INCLUDE_DIRS})

include_directories("./include/loitor_stereo_visensor")



add_library(loitorimu src/loitorimu.cpp)

add_executable(loitor_stereo_visensor src/loitor_stereo_visensor.cpp) 
target_link_libraries(loitor_stereo_visensor ${catkin_LIBRARIES})
target_link_libraries(loitor_stereo_visensor loitorusbcam loitorimu usb-1.0 ${OpenCV_LIBS})
add_dependencies(loitor_stereo_visensor loitor_vis_generate_messages_cpp) 







