cmake_minimum_required(VERSION 3.12)

# Check if using project as standalone or as subproject
get_directory_property(hasParent PARENT_DIRECTORY)
if(NOT hasParent)
    include(pico_sdk_import.cmake)
endif()

project(waveshare_pico_10dof_imu)

if(NOT hasParent)
    pico_sdk_init()
endif()


# lps22hb
add_library(lps22hb src/lps22hb/lps22hb.c)
target_include_directories(lps22hb PRIVATE include)
target_link_libraries(lps22hb PUBLIC hardware_i2c pico_stdlib)

# icm20948
add_library(icm20948 src/icm20948/icm20948.c)
target_include_directories(icm20948 PRIVATE include)
target_link_libraries(icm20948 PUBLIC hardware_i2c pico_stdlib)

set(${PROJECT_NAME}_INCLUDE_DIRS ${CMAKE_CURRENT_LIST_DIR}/include
    CACHE INTERNAL "${PROJECT_NAME}: Include Directories" FORCE)

# main
add_executable(imu src/main.c)
target_include_directories(imu PRIVATE include)

pico_enable_stdio_usb(imu 1)
pico_enable_stdio_uart(imu 1)

pico_add_extra_outputs(imu)

# Pull in our pico_stdlib which pulls in commonly used features
target_link_libraries(imu pico_stdlib icm20948 lps22hb hardware_i2c)
