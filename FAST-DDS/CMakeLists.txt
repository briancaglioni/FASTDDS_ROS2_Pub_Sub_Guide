
cmake_minimum_required(VERSION 2.8.12)

project("generated_code")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)

# Find requirements
find_package(fastcdr REQUIRED)
find_package(fastrtps REQUIRED)


message(STATUS "Configuring String...")
add_library(String_lib String.cxx)
target_link_libraries(String_lib fastcdr fastrtps)

add_executable(String StringPubSubTypes.cxx StringPublisher.cxx StringSubscriber.cxx StringPubSubMain.cxx)
target_link_libraries(String fastcdr fastrtps
        String_lib )



