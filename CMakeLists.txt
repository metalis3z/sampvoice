cmake_minimum_required(VERSION 3.10)

project(SampVoice)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Enable shared library building
set(BUILD_SHARED_LIBS ON)

# Client library
add_library(client SHARED client/client.cpp)
set_target_properties(client PROPERTIES
    PREFIX ""
    OUTPUT_NAME "client"
)

# Server library
add_library(server SHARED control/control.cpp voice/voice.cpp)
set_target_properties(server PROPERTIES
    PREFIX ""
    OUTPUT_NAME "server"
)

# Include directories
include_directories(common)

# Platform-specific settings
if(WIN32)
    # Windows-specific settings
    set_target_properties(client PROPERTIES SUFFIX ".dll")
    set_target_properties(server PROPERTIES SUFFIX ".dll")
elseif(UNIX AND NOT APPLE)
    # Linux-specific settings
    set_target_properties(server PROPERTIES SUFFIX ".so")
endif()
