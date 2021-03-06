PROJECT(RAPP_CALIBRATE_CAMERA)

cmake_minimum_required(VERSION 2.8.3)

find_package(OpenCV REQUIRED)
find_package(RappRobots REQUIRED)

set(Boost_USE_STATIC_LIBS ON)
find_package(Boost COMPONENTS system thread random regex program_options filesystem REQUIRED)
if (Boost_FOUND)
    set(BUILD_SHARED_LIBS OFF)
    include_directories(${Boost_INCLUDE_DIR})
endif()

find_package(Threads REQUIRED)
#find_package(Rapp REQUIRED)
message(STATUS "${RAPP_LIBRARIES_STATIC}")
find_library(RAPP_ROBOT_COMMUNICATION NAMES rapp_robot_communication)
message(STATUS "${RAPP_ROBOT_COMMUNICATION}")
find_library(RAPP_ROBOT_INFO NAMES rapp_robot_info) 
message(STATUS "${RAPP_ROBOT_INFO}")
find_library(RAPP_ROBOT_LOCALIZATION NAMES rapp_robot_localization)
message(STATUS "${RAPP_ROBOT_LOCALIZATION}")
find_library(RAPP_ROBOT_NAVIGATION NAMES rapp_robot_navigation)
message(STATUS "${RAPP_ROBOT_NAVIGATION}")
find_library(RAPP_ROBOT_VISION NAMES rapp_robot_vision)
message(STATUS "${RAPP_ROBOT_VISION}")

#set(CMAKE_FIND_LIBRARY_SUFFIXES ".a")
find_library(RAPP_LIBRARIES_STATIC NAMES rapp)

#include_directories(${RAPP_INCLUDE_DIRS}/rapp)
include_directories(${CMAKE_INSTALL_PREFIX}/include/rapp)
#set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${Rapp_DIR} ${RappRobots_DIR})
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --std=gnu++1y")

add_executable(learn_object main.cpp)

set(RAPP_LIBRARIES ${RAPP_LIBRARIES_STATIC} 
                   ${RAPP_ROBOT_COMMUNICATION}
                   ${RAPP_ROBOT_INFO}
                   ${RAPP_ROBOT_LOCALIZATION}
                   ${RAPP_ROBOT_NAVIGATION}
                   ${RAPP_ROBOT_VISION}
                   ${OPENSSL_LIBRARIES} 
                   ${Boost_LIBRARIES}
                   ${CMAKE_THREAD_LIBS_INIT})

target_link_libraries(learn_object ${RAPP_LIBRARIES} ${OpenCV_LIBS})
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS}-static-libstdc++ -static-libgcc")
set(CMAKE_CXX_FLAGS "--std=gnu++1y -march=atom -mtune=atom -mfpmath=sse")
