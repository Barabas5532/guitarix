project(gx_librtneural)

add_library(gx_librtneural STATIC)
add_library(gx::librtneural ALIAS gx_librtneural)

target_sources(gx_librtneural PRIVATE RTNeural/RTNeural/RTNeural.cpp)

# XXX: Removed .. includes because it belongs to the main project and shouldn't
# be included into this library
target_include_directories(gx_librtneural PUBLIC RTNeural/RTNeural)
target_include_directories(gx_librtneural PRIVATE
                           RTNeural/modules/Eigen
                           RTNeural/modules/json
)

set_target_properties(gx_librtneural PROPERTIES CXX_STANDARD 17)
target_compile_options(gx_librtneural PRIVATE
                       "-Wno-sign-compare"
                       "-Wno-reorder"
                       "-Wno-infinite-recursion"
                       "-Ofast"
)

target_compile_definitions(gx_librtneural PUBLIC
                           "RTNEURAL_DEFAULT_ALIGNMENT=32"
                           "RTNEURAL_USE_EIGEN=1"
                           "RTNEURAL_NAMESPACE=RTNeural"
)

# TODO LTO config
