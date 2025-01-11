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
add_library(gxwmm_inc STATIC)
add_library(gx::gxwmm_inc ALIAS gxwmm_inc)

target_include_directories(gxwmm_inc PUBLIC "../gxwmm-generated")
target_include_directories(gxwmm_inc PRIVATE "../gxwmm-generated/private")

list(APPEND ACTUAL_SOURCES ${SOURCES})
list(TRANSFORM ACTUAL_SOURCES REPLACE "\.hg" "\.cc")
list(TRANSFORM ACTUAL_SOURCES PREPEND "../gxwmm-generated/")
target_sources(gxwmm_inc PRIVATE ${ACTUAL_SOURCES})

target_link_libraries(gxwmm_inc PUBLIC gx::gxw PkgConfig::GTKMM)
