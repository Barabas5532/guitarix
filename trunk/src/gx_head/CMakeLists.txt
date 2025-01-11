cmake_minimum_required(VERSION 3.13)

project(gx_head)

find_package(PkgConfig REQUIRED)

pkg_check_modules(GTK3 REQUIRED IMPORTED_TARGET gtk+-3.0)
pkg_check_modules(gtkmm REQUIRED IMPORTED_TARGET gtkmm-3.0)
pkg_check_modules(lilv-0 REQUIRED IMPORTED_TARGET lilv-0)
pkg_check_modules(lrdf REQUIRED IMPORTED_TARGET lrdf)
pkg_check_modules(GLIBMM REQUIRED IMPORTED_TARGET glibmm-2.4)
pkg_check_modules(GIOMM REQUIRED IMPORTED_TARGET giomm-2.4)

# conf.check_cfg(package='jack', args=['--cflags','--libs','jack >= 1.9.2'], uselib_store='JACK', msg="Checking for 'jackd2'", mandatory=1)
pkg_check_modules(JACK REQUIRED IMPORTED_TARGET jack)

# conf.check_cfg(package='sndfile', args=conf.env['OS_SNDFILE_CFGFLAGS'], uselib_store='SNDFILE', mandatory=1)
pkg_check_modules(SNDFILE REQUIRED IMPORTED_TARGET sndfile)

# conf.check_cfg(package='gthread-2.0', args=['--cflags','--libs','gthread-2.0 >= 2.24'], uselib_store='GTHREAD', mandatory=1)
pkg_check_modules(GTHREAD REQUIRED IMPORTED_TARGET gthread-2.0)

# conf.check_cfg(package='gmodule-export-2.0', args='--cflags --libs', uselib_store='GMODULE_EXPORT', mandatory=1)
pkg_check_modules(GMODULE_EXPORT REQUIRED IMPORTED_TARGET gmodule-export-2.0)

# conf.check_cfg(package='libcurl', args=['--cflags','--libs','libcurl >= 7.26.0'], uselib_store='CURL', mandatory=1)
pkg_check_modules(CURL REQUIRED IMPORTED_TARGET libcurl)

# conf.check_cfg(package='fftw3f', args=['--cflags','--libs','fftw3f >= 3.3.8'], uselib_store='FFTW3', mandatory=1)
pkg_check_modules(FFTW3 REQUIRED IMPORTED_TARGET fftw3f)

# conf.check_cfg(package='lrdf', args='--cflags --libs', uselib_store='LRDF', mandatory=1)
pkg_check_modules(LRDF REQUIRED IMPORTED_TARGET lrdf)

# conf.check_cxx(msg = msg, fragment=code, lib="boost_system", uselib_store="BOOST_SYSTEM", mandatory=1,includes='/opt/local/include', libpath='/opt/local/lib')
# conf.check_cxx(msg = msg, fragment=code, lib=["boost_iostreams","boost_system"], uselib_store="BOOST_IOSTREAMS", mandatory=1)

find_package(Boost 1.42 COMPONENTS system iostreams)

# TODO support calling out to gperf like the HAVE_GPERF branch in wscript
configure_file(engine/jsonrpc_methods-generated.cc jsonrpc_methods.cc COPYONLY)
configure_file(engine/jsonrpc_methods-generated.h jsonrpc_methods.h COPYONLY)

add_executable(gx_head)
target_sources(gx_head
    PRIVATE
        "engine/gx_json.cpp"
        "engine/gx_faust_plugins.cpp"
        "engine/gx_internal_plugins.cpp"
        "engine/gx_engine_audio.cpp"
        "engine/gx_paramtable.cpp"
        "engine/gx_convolver.cpp"
        "engine/gx_resampler.cpp"
        "engine/gx_system.cpp"
        "engine/gx_logging.cpp"
        "engine/gx_pluginloader.cpp"
        "./engine/ladspaplugin.cpp"
        "./engine/ladspaback.cpp"
        "./engine/tunerswitcher.cpp"
        "./engine/jsonrpc.cpp"
        "./engine/gx_jack.cpp"
        "./engine/gx_preset.cpp"
        "./engine/gx_internal_ui_plugins.cpp"
        "./engine/gx_pitch_tracker.cpp"
        "./engine/gx_engine.cpp"
        "./gui/machine.cpp"
        "./gui/ladspalist.cpp"
        "./gui/gx_stackbox_builder.cpp"
        "./gui/gx_main_window.cpp"
        "./gui/liveplay.cpp"
        "./gui/rack.cpp"
        "./gui/gx_preset_window.cpp"
        "./gui/gx_jack_options.cpp"
        "./gui/gx_ui_builder.cpp"
        "./gui/gx_gui_helpers.cpp"
        "./gui/gx_child_process.cpp"
        "./gui/gx_main_midi.cpp"
        "./gui/gx_jconv_settings.cpp"
        "./gui/gx_sequencer_settings.cpp"
        "./gui/gxw_mm_controllers.cpp"
        "./gui/gx_main_boxes.cpp"
        "./gui/gx_portmap.cpp"
        "./gui/gx_main.cpp"
    )

target_include_directories(gx_head
    PRIVATE
    "."
    "./engine"
    "./engine/tabels"
    "../.."
    ".." # need ".." for config.h
    "../headers"
    "../NAM/NeuralAmpModelerCore/NAM/"
    "../NAM/NeuralAmpModelerCore/Dependencies/nlohmann"
    "../NAM/NeuralAmpModelerCore/Dependencies/eigen"
    # Remove RTNeural includes because just linking to it should be enough
    "${CMAKE_CURRENT_BINARY_DIR}" # for jsonrpc_methods.{cc,h}
)

target_link_libraries(gx_head
    PRIVATE
    PkgConfig::JACK
                      PkgConfig::SNDFILE
                      PkgConfig::GTHREAD
                      PkgConfig::GMODULE_EXPORT
                      PkgConfig::CURL
                      PkgConfig::GTK3
                      PkgConfig::gtkmm
    PkgConfig::GIOMM
                      PkgConfig::FFTW3
                      PkgConfig::LRDF
    PkgConfig::lilv-0
    Boost::system
                      Boost::iostreams
                      # TODO
                      #gx::ZITA_CONVOLVER
                      #gx::ZITA_RESAMPLER
    gx::faust
    gx::gxw
    gx::gxwmm_inc
    gx::librtneural

)

list(APPEND BUILDER_FILES "./builder/midi.glade"
        "./builder/midi_channel.glade"
        "./builder/ports.glade"
        "./builder/iredit.glade"
        "./builder/iredit_mono.glade"
        "./builder/jackstarter.glade"
        "./builder/mainpanel.glade"
        "./builder/ampbox.glade"
        "./builder/liveplay.glade"
        "./builder/ladspaliste.glade"
        "./builder/crybaby_ui.glade"
        "./builder/wah_ui.glade"
        "./builder/vibe_ui.glade"
        "./builder/vibe_stereo_ui.glade"
        "./builder/reversedelay_ui.glade"
        "./builder/gx_distortion_ui.glade"
        "./builder/gx_record_ui.glade"
        "./builder/gx_st_record_ui.glade"
        "./builder/gx_detune_ui.glade"
        "./builder/abgate_ui.glade"
        "./builder/amp.tonestack_ui.glade"
        "./builder/ampimpulse_ui.glade"
        "./builder/ampimpulse_st_ui.glade"
        "./builder/cabinet_ui.glade"
        "./builder/cabinet_stereo_ui.glade"
        "./builder/jconv_ui.glade"
        "./builder/jconv_st_ui.glade"
        "./builder/oscilloscope_ui.glade"
        "./builder/drumsequencer_ui.glade"
        "./builder/dubber_ui.glade"
        "./builder/metronome_ui.glade"
        "./builder/nam_ui.glade"
        "./builder/snam_ui.glade"
        "./builder/mnam_ui.glade"
        "./builder/rtneural_ui.glade"
        "./builder/srtneural_ui.glade"
        "./builder/mrtneural_ui.glade"
        "./builder/tremolo_ui.glade"
        "./builder/pluginpreset_inputwindow.glade"
        "./builder/pluginpreset_listwindow.glade"
        "./builder/pluginpreset_connectwindow.glade"
        "./builder/selectinstance.glade"
        "./builder/Sequencer.glade"
        "./builder/vumeter_ui.glade"
        "./builder/vumeter_st_ui.glade"
        "./builder/poweramps_ui.glade"
        "./builder/menu.ui"
        "./builder/gx_plugins.png"
        "./builder/gx_rack.png"
        "./builder/gx_show.png"
        "./builder/gx_split.png"
        "./builder/gx_tuner.png"
        "./builder/gx_config.png"
        "./builder/gx_fold.png"
        "./builder/gx_fullscreen.png"
    )

# TODO
# guitarix.lds - linker script
# Install BUILDER_FILES to somewhere (?)
# add extra files if some options are enabled
# avahi
# bluez
# pluginlib
