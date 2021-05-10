cmake_minimum_required(VERSION 2.8.3)

set(PROJECT_NAME lib_cvgekf)
project(${PROJECT_NAME})

### Use version 2011 of C++ (c++11). By default ROS uses c++98
#see: http://stackoverflow.com/questions/10851247/how-to-activate-c-11-in-cmake
#see: http://stackoverflow.com/questions/10984442/how-to-detect-c11-support-of-a-compiler-with-cmake
#add_definitions(-std=c++11)
#add_definitions(-std=c++0x)
add_definitions(-std=c++03)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)


set(CVGEKFLIB_SOURCE_DIR
        src/source)

set(CVGEKFLIB_INCLUDE_DIR
        src/include)

set(CVGEKFLIB_HEADER_FILES
        ${CVGEKFLIB_INCLUDE_DIR}/extendedKalmanFilter.h
#        ${CVGEKFLIB_INCLUDE_DIR}/matrixLib.h   # moved to lib_cvgutils
        ${CVGEKFLIB_INCLUDE_DIR}/model.h
)



set(CVGEKFLIB_SOURCE_FILES
        ${CVGEKFLIB_SOURCE_DIR}/extendedKalmanFilter.cpp
#        ${CVGEKFLIB_SOURCE_DIR}/matrixLib.cpp  # moved to lib_cvgutils
        ${CVGEKFLIB_SOURCE_DIR}/model.cpp
)


find_package(catkin REQUIRED
		COMPONENTS lib_cvgutils)

find_package(OpenCV REQUIRED)

catkin_package(
        INCLUDE_DIRS ${CVGEKFLIB_INCLUDE_DIR}
        LIBRARIES lib_cvgekf
        DEPENDS OpenCV
        CATKIN_DEPENDS lib_cvgutils
  )


include_directories(${CVGEKFLIB_INCLUDE_DIR})
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(${catkin_INCLUDE_DIRS})



add_library(lib_cvgekf ${CVGEKFLIB_SOURCE_FILES} ${CVGEKFLIB_HEADER_FILES})
add_dependencies(lib_cvgekf ${catkin_EXPORTED_TARGETS})
target_link_libraries(lib_cvgekf ${OpenCV_LIBS})
target_link_libraries(lib_cvgekf ${catkin_LIBRARIES})

