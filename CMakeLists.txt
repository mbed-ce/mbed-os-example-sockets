# Copyright (c) 2020 ARM Limited. All rights reserved.
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.19)
cmake_policy(VERSION 3.19)

# Initialize Mbed OS build system. 
# Note: This block must be before the include of app.cmake
set(MBED_APP_JSON_PATH mbed_app.json)

include(mbed-os/tools/cmake/app.cmake)
add_subdirectory(mbed-os)

project(mbed-os-example-sockets)

add_executable(mbed-os-example-sockets
    source/main.cpp)
mbed_set_post_build(mbed-os-example-sockets)

target_include_directories(mbed-os-example-sockets
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(mbed-os-example-sockets
    PRIVATE
        mbed-os
        mbed-netsocket
        mbed-mbedtls
)

if("ism43362" IN_LIST MBED_TARGET_LABELS)
    # Note: The ism43362 library's CMake code provides an interface target, so its
    # code will get recompiled for every library that links to it.  Might want to change
    # it to a static library at some point...
    add_subdirectory(wifi-ism43362)
    target_link_libraries(mbed-os-example-sockets
        PRIVATE
            wifi-ism43362
    )
endif()
