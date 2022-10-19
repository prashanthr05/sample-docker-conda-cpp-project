# minimum version of CMake required to build this project
# this is a mandatory function - without this running  CMake will fail
cmake_minimum_required(VERSION 3.15)

# Specify the details of the project: name, version and programming language
project(HelloWorld VERSION 0.1.0 LANGUAGES CXX)

# We will create an executable target with the name helloWorldApp
add_executable(helloWorldApp src/main.cpp)
