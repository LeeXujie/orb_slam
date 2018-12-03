cmake_minimum_required(VERSION 2.8.3)
project(orb_slam)

#IF(NOT CMAKE_BUILD_TYPE)
#  SET(CMAKE_BUILD_TYPE Release)
#ENDIF()
MESSAGE("Build type: " ${CMAKE_BUILD_TYPE})

add_compile_options(-std=c++11)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Pangolin REQUIRED)

find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  image_transport
  roscpp
  sensor_msgs
  tf
)

catkin_package(
  CATKIN_DEPENDS
)

include_directories(
include/orb_slam1
include/orb_slam2
${catkin_INCLUDE_DIRS}
${PROJECT_SOURCE_DIR}
${EIGEN3_INCLUDE_DIR}
${Pangolin_INCLUDE_DIRS}
)

set(LIBS 
${OpenCV_LIBS}
${EIGEN3_LIBS}
${catkin_LIBRARIES}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
/usr/lib/x86_64-linux-gnu/libboost_system.so
/usr/lib/x86_64-linux-gnu/libboost_filesystem.so
)

add_executable(orbslam1_mono src/orbslam1_mono.cc)
target_link_libraries(orbslam1_mono ${LIBS} ${PROJECT_SOURCE_DIR}/lib/libORB_SLAM1.so)

add_executable(orbslam2_mono src/orbslam2_mono.cc)
target_link_libraries(orbslam2_mono ${LIBS} ${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so)

add_executable(orbslam2_stereo src/orbslam2_stereo.cc)
target_link_libraries(orbslam2_stereo ${LIBS} ${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so)

add_executable(orbslam2_rgbd src/orbslam2_rgbd.cc)
target_link_libraries(orbslam2_rgbd ${LIBS} ${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so)

add_executable(orbslam2_mono_ar 
src/AR/orbslam2_mono_ar.cc
src/AR/ViewerAR.h
src/AR/ViewerAR.cc
)
target_link_libraries(orbslam2_mono_ar ${LIBS} ${PROJECT_SOURCE_DIR}/lib/libORB_SLAM2.so)
