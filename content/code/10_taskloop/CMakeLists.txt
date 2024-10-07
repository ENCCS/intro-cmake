# set minimum cmake version
cmake_minimum_required(VERSION 3.18)

# project name and language
project(taskloop LANGUAGES CXX)

add_executable(taskloop taskloop.cpp)

# FIXME find the suitable OpenMP component

# FIXME link with imported target
target_link_libraries(taskloop ...)
