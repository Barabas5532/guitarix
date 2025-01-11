project(gxwmm)

find_package(PkgConfig REQUIRED)

pkg_check_modules(GTKMM REQUIRED IMPORTED_TARGET gtkmm-3.0)

list(APPEND SOURCES
     "controlparameter.hg"
     "switch.hg"
     "regler.hg"
     "knob.hg"
     "bigknob.hg"
     "midknob.hg"
     "smallknob.hg"
     "smallknobr.hg"
     "tuner.hg"
     "racktuner.hg"
     "fastmeter.hg"
     "waveview.hg"
     "selector.hg"
     "toggleimage.hg"
     "iredit.hg"
     "radiobutton.hg"
     "wheel.hg"
     "wheelvertical.hg"
     "hslider.hg"
     "minislider.hg"
     "vslider.hg"
     "eqslider.hg"
     "levelslider.hg"
     "paintbox.hg"
     "meterscale.hg"
     #"gradient.hg",
     "valuedisplay.hg"
     "simplevaluedisplay.hg"
     "portdisplay.hg"
     "playhead.hg"
)

# TODO support generation from hg files in addition to using pre-generated files

SET(GDIR  "../gxwmm-generated/")
foreach(SOURCE_FILE IN LISTS SOURCES)
    string(REPLACE ".hg" ".cc" D "${SOURCE_FILE}")
    cmake_path(APPEND GDIR "${D}" OUTPUT_VARIABLE S)
    configure_file("${S}" "${D}" COPYONLY)

    string(REPLACE ".hg" ".h" D "${SOURCE_FILE}")
    cmake_path(APPEND GDIR "${D}" OUTPUT_VARIABLE S)
    configure_file("${S}" "${D}" COPYONLY)

    string(REPLACE ".hg" "_p.h" D "${SOURCE_FILE}")
    cmake_path(APPEND GDIR "private/${D}" OUTPUT_VARIABLE S)
    configure_file("${S}" "private/${D}" COPYONLY)
endforeach()

add_library(gxwmm_inc STATIC)
add_library(gx::gxwmm_inc ALIAS gxwmm_inc)

target_include_directories(gxwmm_inc PUBLIC "${CMAKE_CURRENT_BINARY_DIR}/..")
target_include_directories(gxwmm_inc PUBLIC "..")
target_include_directories(gxwmm_inc PRIVATE "${CMAKE_CURRENT_BINARY_DIR}/private")

target_sources(gxwmm_inc PRIVATE "${CMAKE_CURRENT_BINARY_DIR}/knob.cc")

target_link_libraries(gxwmm_inc PUBLIC gx::gxw PkgConfig::GTKMM)
