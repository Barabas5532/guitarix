cmake_minimum_required(VERSION 3.13)
project(guitarix)

add_subdirectory(libgxw/gxw)
add_subdirectory(src/RTNeural)

add_subdirectory(libgxwmm)

add_subdirectory(src/faust)
add_subdirectory(src/gx_head)
