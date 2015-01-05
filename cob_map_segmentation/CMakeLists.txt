cmake_minimum_required(VERSION 2.6)
project(cob_map_segmentation)

set(catkin_RUN_PACKAGES 
        roscpp
        actionlib_msgs
        sensor_msgs
        nav_msgs
        cv_bridge
)

set(catkin_BUILD_PACKAGES 
	${catkin_RUN_PACKAGES}
	cmake_modules
)
## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
## Creates a bunch of environment variables that may be used later in the script
## e.g. catkin_INCLUDE_DIRS

find_package(catkin REQUIRED COMPONENTS genmsg actionlib_msgs actionlib
	${catkin_BUILD_PACKAGES}
)

find_package(OpenCV REQUIRED)
find_package(Boost REQUIRED COMPONENTS filesystem)

add_action_files(DIRECTORY action FILES MapSegmentation.action)
generate_messages(DEPENDENCIES actionlib_msgs sensor_msgs)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
INCLUDE_DIRS
	ros/include
LIBRARIES
CATKIN_DEPENDS
	${catkin_RUN_PACKAGES}
DEPENDS
	OpenCV
  Boost
)


###########
## Build ##
###########
## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include ${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS})
include_directories(
	ros/include
	${catkin_INCLUDE_DIRS}
	${OpenCV_INCLUDE_DIRS}
	${Boost_INCLUDE_DIRS}
)

## Declare a cpp library

#set(project_CPP_FILES
#)

## Declares a shared library. Resulting name will be lib<name>.so

## Declare a cpp executable
#add_executable(map_segmentation_action_client
#	ros/src/map_segmentation_action_client.cpp 
#)
#target_link_libraries(map_segmentation_action_client
#	${catkin_LIBRARIES}
#	${OpenCV_LIBRARIES}
#	${Boost_LIBRARIES}
#)

add_executable(map_segmentation_action_server
	ros/src/map_segmentation_action_server.cpp
)
target_link_libraries(map_segmentation_action_server
	${catkin_LIBRARIES}
	${OpenCV_LIBRARIES}
	${Boost_LIBRARIES}
)
## Specify libraries to link a library or executable target against

add_dependencies( map_segmentation_action_server ${catkin_EXPORTED_TARGETS})

# set build flags for targets
#set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-D__LINUX__ -DBOOST_FILESYSTEM_VERSION=2")

#############
## Install ##
#############
## Mark executables and/or libraries for installation
install(TARGETS 
    #map_segmentation_action_client
    map_segmentation_action_server
	ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
	LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
	RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY 
		ros/include/${PROJECT_NAME}
	DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
	FILES_MATCHING PATTERN "*.h"
	PATTERN ".svn" EXCLUDE
)

install(DIRECTORY ros/launch
	DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/ros
	PATTERN ".svn" EXCLUDE
)
