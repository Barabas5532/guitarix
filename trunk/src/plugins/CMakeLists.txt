# TODO this is simplified a lot to make the gx_head build for now

#conf.check_cfg(package='eigen3', args='--cflags', uselib_store="EIGEN3", mandatory=1)
pkg_check_modules(EIGEN3 REQUIRED IMPORTED_TARGET eigen3)

list(APPEND LIB_SOURCES
"zita_rev1.cc"
"ts9sim.cc"
"flanger_gx.cc"
"mbc.cc"
"mbcs.cc"
"mbd.cc"
"mbe.cc"
"mbdel.cc"
"mbchor.cc"
"mbclipper.cc"
"gcb_95.cc"
"fuzzfacerm.cc"
"fuzzfacefm.cc"
"hfb.cc"
"muff.cc"
"susta.cc"
"scream.cc"
"hornet.cc"
"foxeylady.cc"
"lpbboost.cc"
"hogsfoot.cc"
"cstb.cc"
"coverd.cc"
"rangem.cc"
"ruin.cc"
"ffreak.cc"
"fumaster.cc"
"fuzzdrive.cc"
"rolandwah.cc"
"astrofuzz.cc"
"buffb.cc"
"eldist.cc"
"mole.cc"
"buzz.cc"
"bfuzz.cc"
"axface.cc"
"aclipper.cc"
"bmp.cc"
"mxrdist.cc"
"bossds1.cc"
"bassboom.cc"
)
list(TRANSFORM LIB_SOURCES PREPEND "generated/")

list(APPEND LIB_CSOURCES 
"vibe.cc"
"metronome.cc"
"abgate.cc"
"jcm800pre.cpp"
"reversedelay.cc"
"vumeter.cc"
"vumeter_st.cc"
)

add_library(gx_plugins STATIC)
add_library(gx::plugins ALIAS gx_plugins)
target_include_directories(gx_plugins PUBLIC
"."
"../headers"
"../gx_head/engine"
"../gx_head/engine/tabels"
)

target_sources(gx_plugins PRIVATE ${LIB_SOURCES} ${LIB_CSOURCES})
target_link_libraries(gx_plugins PUBLIC PkgConfig::EIGEN3 gx::faust)