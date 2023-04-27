cmake_minimum_required(VERSION 3.16)

project(DuckTape LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

enable_testing()
cmake_path(SET conan_build_info ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)

if(NOT EXISTS "${conan_build_info}")
    execute_process(COMMAND conan install ${CMAKE_SOURCE_DIR} WORKING_DIRECTORY ${CMAKE_BINARY_DIR} TIMEOUT 60 RESULT_VARIABLE return_code)
    
    if(NOT ${return_code} MATCHES 0)
        message(FATAL_ERROR "Conan install command failed")
    endif()

    message(STATUS "Conan install command succeeded")
endif()

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_subdirectory(src)
add_subdirectory(tests)