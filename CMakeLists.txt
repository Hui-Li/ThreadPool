cmake_minimum_required(VERSION 2.8)
project(ThreadPool)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++11 -DNDEBUG -pthread -march=native -ftree-vectorize")

find_package(Boost COMPONENTS thread system)

IF(NOT ${Boost_FOUND})
    SET(BOOST_ROOT ~/local) # default
    SET(Boost_NO_SYSTEM_PATHS ON) # force to use own build
    find_package(Boost COMPONENTS thread system)
ENDIF(NOT ${Boost_FOUND})

if(Boost_FOUND)
    message(STATUS "Boost found")
    include_directories( ${Boost_INCLUDE_DIR})
    link_libraries(${Boost_LIBRARIES})
endif(Boost_FOUND)

set(threadpool
    boost/threadpool.hpp)

set(util
    util/Monitor.h)

add_library(lib SHARED ${threadpool} ${util})
set_target_properties(lib PROPERTIES LINKER_LANGUAGE CXX)

set(SOURCE_FILES main.cpp)
add_executable(ThreadPool ${SOURCE_FILES})
target_link_libraries(ThreadPool lib)