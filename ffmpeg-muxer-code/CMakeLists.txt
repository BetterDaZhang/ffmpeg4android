
cmake_minimum_required(VERSION 3.4.1)


file(GLOB native_files "src/main/cpp/*.cpp")

add_library(native-encode
           SHARED
           ${native_files})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")
set(CMAKE_VERBOSE_MAKEFILE on)

#获取上级目录
get_filename_component(PARENT_DIR ${CMAKE_SOURCE_DIR} PATH)
#使用单个库版本
set(LIBFFMPEG_DIR ${PARENT_DIR}/ffmpeg-single)

add_library(ffmpeg-single
           SHARED
           IMPORTED)

set_target_properties(ffmpeg-single
                    PROPERTIES IMPORTED_LOCATION
                    ${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/libffmpeg.so
                    )

# libffmpeg库头文件
include_directories(${LIBFFMPEG_DIR}/libs/${ANDROID_ABI}/include)

target_link_libraries(native-encode
                   ffmpeg-single
                   android
                   log)

