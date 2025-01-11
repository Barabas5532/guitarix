LIST(APPEND UNUSED_SOURCES
        "preamp.dsp"
        "inputgain.dsp"
        "AntiAlias.dsp"
        "HighShelf.dsp"
        "drive.dsp"
        "osc_tube.dsp"
        "reso_tube.dsp"
        "tube.dsp"
        "tube3.dsp"
        "tubevibrato.dsp"
        "multifilter.dsp"
        "eq.dsp"
        "outputgain.dsp"
        "feed.dsp"
        "balance1.dsp"
        "amp2.dsp"
        "stage3.dsp"
        "distortion.dsp"
        "distortion1.dsp"
        "noisegate.dsp"
        "sloop.dsp" ##float
        "vibe_lfo_triangle.dsp"
        "vibe_mono_lfo_triangle.dsp"
    )

LIST(APPEND SOURCES_FLOAT "jconv_post.dsp")

LIST(APPEND SOURCES
        "jconv_post_mono.dsp"
        "presence_level.dsp"
        "cabinet_impulse_former.dsp"
        "cabinet_impulse_former_st.dsp"
        "preamp_impulse_former.dsp"
        "preamp_impulse_former_st.dsp"
        "drumseq.dsp"
        "low_high_cut.dsp"
)

LIST(APPEND SOURCES_STATIC
        "vibe_mono_lfo_sine.dsp"
        "vibe_lfo_sine.dsp"
)

LIST(APPEND SOURCES_PLUGIN_FLOAT
        "echo.dsp"
        "delay.dsp"
        "chorus.dsp"
        "chorus_mono.dsp"
        "stereodelay.dsp"
        "stereoecho.dsp"
        "digital_delay.dsp"
        "digital_delay_st.dsp"
        "phaser.dsp"
        "phaser_mono.dsp"
        "crybaby.dsp"
        "autowah.dsp"
)

LIST(APPEND SOURCES_PLUGIN_DOUBLE
        "selecteq.dsp"

        # tone stacks
        "tonestack_default.dsp"  # this one also ok in single precision
        "tonestack_bassman.dsp"
        "tonestack_twin.dsp"
        "tonestack_princeton.dsp"
        "tonestack_jcm800.dsp"
        "tonestack_jcm2000.dsp"
        "tonestack_mlead.dsp"
        "tonestack_m2199.dsp"
        "tonestack_ac30.dsp"
        "tonestack_soldano.dsp"
        "tonestack_mesa.dsp"
        "tonestack_jtm45.dsp"
        "tonestack_ac15.dsp"
        "tonestack_peavey.dsp"
        "tonestack_ibanez.dsp"
        "tonestack_roland.dsp"
        "tonestack_ampeg.dsp"
        "tonestack_ampeg_rev.dsp"
        "tonestack_sovtek.dsp"
        "tonestack_bogner.dsp"
        "tonestack_groove.dsp"
        "tonestack_crunch.dsp"
        "tonestack_fender_blues.dsp"
        "tonestack_fender_default.dsp"
        "tonestack_fender_deville.dsp"
        "tonestack_gibsen.dsp"
        "tonestack_engl.dsp"
)

LIST(APPEND SOURCES_PLUGIN
        "low_high_pass.dsp"
        "gx_distortion.dsp"
        "bitdowner.dsp"
        "thick_distortion.dsp"
        "impulseresponse.dsp"
        "compressor.dsp"
        "expander.dsp"
        "overdrive.dsp"
        "freeverb.dsp"
        "biquad.dsp"
        "tremolo.dsp"
        "flanger_mono.dsp"
        "gx_feedback.dsp"
        "flanger.dsp"
        "moog.dsp"
        "gx_ampmodul.dsp"
        "tonecontroll.dsp"
        "stereoverb.dsp"
        "softclip.dsp"
        "gx_ampout.dsp"
        "gx_ampout_ladspa.dsp"
        "noise_shaper.dsp"
        "bassbooster.dsp"
        "gx_outputlevel.dsp"
        "gx_outputlevel_ladspa.dsp"
        "gxfeed.dsp"
        "highbooster.dsp"
        "peak_eq.dsp"
        "dattorros_progenitor.dsp"
        "ring_modulator.dsp"
        "ring_modulator_st.dsp"
        "panoram_enhancer.dsp"
        "graphiceq.dsp"
        "bass_enhancer.dsp"
        "gain.dsp"
        "duck_delay.dsp"
        "duck_delay_st.dsp"
        "baxandall.dsp"
        "distortion2.dsp"
        "fuzzface.dsp"
        "trbuff.dsp"
        "colwah.dsp"
        "dallaswah.dsp"
        "foxwah.dsp"
        "maestrowah.dsp"
        "voxwah.dsp"
        "selwah.dsp"
        "jenwah.dsp"
        "colbwah.dsp"
        "rolwah.dsp"
        "jenbasswah.dsp"
        "fizz_remover.dsp"

        # amps
        "gxamp.dsp"
        "gxamp2.dsp"
        "gxamp3.dsp"
        "gxamp4.dsp"
        "gxamp5.dsp"
        "gxamp6.dsp"
        "gxamp7.dsp"
        "gxamp8.dsp"
        "gxamp9.dsp"
        "gxamp10.dsp"
        "gxamp11.dsp"
        "gxamp12.dsp"
        "gxamp13.dsp"
        "gxamp14.dsp"
        "gxamp15.dsp"
        "gxamp16.dsp"
        "gxamp17.dsp"
        "gxamp18.dsp"
        "gxnoamp.dsp"

        # poweramps
        "champ.dsp"
        "epiphone.dsp"
        "orangedarkterror.dsp"
        "princeton.dsp"
        "plexiel34.dsp"
        "supersonic.dsp"

        # bypass mode
        "balance.dsp"
        "dcblocker.dsp"
        "hardlim.dsp"
    )

    # TODO
    # Also implement running the dsp2cc script instead of using the pre-built
    # files. This implementation is the same as the else branch of the
    # `if bld.env['FAUST']` in wscript.
    SET(GDIR  "../faust-generated/")
    foreach(SOURCE_FILE IN LISTS
            SOURCES
            SOURCES_STATIC
            SOURCES_FLOAT
            SOURCES_PLUGIN_FLOAT
            SOURCES_PLUGIN_DOUBLE
            SOURCES_PLUGIN
    )
        string(REPLACE ".dsp" ".cc" D "${SOURCE_FILE}")
        cmake_path(APPEND GDIR "${D}" OUTPUT_VARIABLE S)
        configure_file("${S}" "${D}" COPYONLY)
    endforeach()

    foreach(SOURCE_FILE IN LISTS
            SOURCES
            SOURCES_FLOAT
    )
        string(REPLACE ".dsp" ".h" D "${SOURCE_FILE}")
        cmake_path(APPEND GDIR "${D}" OUTPUT_VARIABLE S)
        configure_file("${S}" "${D}" COPYONLY)
    endforeach()

    add_library(gx_faust INTERFACE)
    add_library(gx::faust ALIAS gx_faust)
    target_include_directories(gx_faust INTERFACE "${CMAKE_CURRENT_BINARY_DIR}/..")
