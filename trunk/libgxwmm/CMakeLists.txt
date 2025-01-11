project(libgxwmm)

add_subdirectory(gxwmm)

add_executable(gxw_demo)
target_sources(gxw_demo PRIVATE gxw_demo.cc)