cmake_minimum_required(VERSION 2.8.3)
project(remittance_calib)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  gflags_catkin
  glog_catkin
  pcl_catkin
  eigen_catkin
)


catkin_package(
 INCLUDE_DIRS include
 LIBRARIES ${PROJECT_NAME}
 CATKIN_DEPENDS gflags_catkin glog_catkin pcl_catkin eigen_catkin
)
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

set(PUBLIC_HPP
  include/remittance_calib/pcl_point.hpp
  include/remittance_calib/io.hpp
  include/remittance_calib/pdf.hpp
  include/remittance_calib/types.hpp
  include/remittance_calib/calibrator.hpp
)
set(LIB_CPP
  src/io.cpp
  src/types.cpp
  src/calibrator.cpp
)
## Declare a C++ library
add_library(${PROJECT_NAME} SHARED ${PUBLIC_HPP} ${LIB_CPP} )
target_link_libraries(${PROJECT_NAME}
${catkin_LIBRARIES})

# Tests
catkin_add_gtest(${PROJECT_NAME}_tests
  test/test_main.cpp

  WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/test
)
target_link_libraries(${PROJECT_NAME}_tests
  ${PROJECT_NAME}
  ${catkin_LIBRARIES}
)


## Executable 1
add_executable(calibrate src/run_calibration.cpp)
target_link_libraries(calibrate ${PROJECT_NAME}  ${catkin_LIBRARIES} )
## Executable 2
add_executable(correct src/run_correction.cpp)
target_link_libraries(correct ${PROJECT_NAME}  ${catkin_LIBRARIES})
## Executable 2
add_executable(merge src/merge_pointclouds.cpp)
target_link_libraries(merge ${PROJECT_NAME}  ${catkin_LIBRARIES})
