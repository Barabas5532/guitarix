cmake_minimum_required(VERSION 3.13)

project(gx_head)

# TODO support calling out to gperf like the HAVE_GPERF branch in wscript
configure_file(engine/jsonrpc_methods-generated.cc jsonrpc_methods.cc COPYONLY)
configure_file(engine/jsonrpc_methods-generated.h jsonrpc_methods.h COPYONLY)

list(APPEND SOURCES_ENGINE_SHARED 
    "engine/gx_json.cpp"
    "engine/gx_faust_plugins.cpp"
    "engine/gx_internal_plugins.cpp"
    "engine/gx_neural_plugins.cpp"
    "engine/gx_engine_audio.cpp"
    "engine/gx_paramtable.cpp"
    "engine/gx_convolver.cpp"
    "engine/gx_resampler.cpp"
    "engine/gx_system.cpp"
    "engine/gx_logging.cpp"
    "engine/gx_pluginloader.cpp"
)

list(APPEND SOURCES_ENGINE 
    "./engine/ladspaplugin.cpp"
    "./engine/ladspaback.cpp"
    "./engine/tunerswitcher.cpp"
    "./engine/jsonrpc.cpp"
    "./engine/gx_jack.cpp"
    "./engine/gx_preset.cpp"
    "./engine/gx_internal_ui_plugins.cpp"
    "./engine/gx_pitch_tracker.cpp"
    "./engine/gx_engine.cpp"
)

list(APPEND SOURCES_GUI
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

add_executable(gx_head)
target_sources(gx_head
    PRIVATE
        ${SOURCES_ENGINE}
        ${SOURCES_ENGINE_SHARED}
        ${SOURCES_GUI}
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
                      gx::ZITA_CONVOLVER
                      gx::ZITA_RESAMPLER
    gx::faust
    gx::gxw
    gx::gxwmm
    gx::librtneural
    gx::plugins
    gx::nam
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
