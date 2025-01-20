find_program(SASSC_PROGRAM sassc REQUIRED)

list(APPEND scss_theme_files
"gx_head_Guitarix.scss"
"gx_head_Copper.scss"
"gx_head_Lavender.scss"
"gx_head_Sky.scss"
"gx_head_Olive.scss"
"gx_head_Hippie.scss"
"gx_head_Psycedelic.scss"
"gx_head_Grandma.scss"
"gx_head_Grungy_Sun_Dark.scss"
"gx_head_Grungy_Sun.scss"
"gx_head_Aluminium.scss"
"gx_head_Oak.scss"
"gx_head_Burl.scss"
"gx_head_White_Oak.scss"
"gx_head_Guitarix_Oak.scss"
"gx_head_Plain_Dark.scss"
"gx_head_Dark.scss"
"gx_head_Nebula.scss"
"gx_head_Camouflage.scss"
"gx_head_Orange.scss"
"gx_head_Rust.scss"
"gx_head_Gold.scss"
)

list(APPEND scss_additional_files "minimal.scss")

list(APPEND gxstyle_files 
"clear.css"
"background1.png"
"background2.png"
"live_wallpaper_aluminium.png"
"live_wallpaper_camouflage.png"
"live_wallpaper_copper.png"
"live_wallpaper_dark.png"
"live_wallpaper.png"
"live_wallpaper_guitarix_oak.png"
"live_wallpaper_lavender.png"
"live_wallpaper_oak.png"
"live_wallpaper_olive.png"
"live_wallpaper_orange.png"
"live_wallpaper_sky.png"
"live_wallpaper_white_oak.png"
"live_wallpaper_burl.png"
"live_wallpaper_psycedelic.png"
"live_wallpaper_hippie.png"
"live_wallpaper_grungy_sun.png"
"live_wallpaper_grungy_sun_dark.png"
"live_wallpaper_grandma.png"
"live_wallpaper_nebula.png"
"live_wallpaper_rust.png"
"live_wallpaper_gold.png"

"tactile_knob_large_151f.png"
"tactile_knob_small_99f.png"
"tactile_knob_medium_151f.png"
"tactile_wheel_101f.png"

"stereo.png"
"live_bypass.png"
"factory.png"
"live_mute.png"
"readonly.png"
"scratch.png"
"versiondiff.png"
"playhead.png"
"simplelevelslider.png"

"switch_on.png"
"switch_mid_off.png"
"switch_mid_on.png"
"switch_off.png"
"rack_expand.png"
"rack_shrink.png"
"rack_preset.png"
"insert.png"
"hslider.png"
"logo.png"
"logo_orange.png"
"logo_white.png"
"handle_left.png"
"handle_right.png"

# knobs
"knob_small.png"
"knob_small_r.png"
"knob_mid.png"
"knob_big.png"

###COPPER THEME
"copper.png"

###LAVENDER THEME
"lavender.png"

###SKY THEME
"sky.png"

###OLIVE THEME
"olive.png"

###HIPPIE THEME
"hippie.png"
"hippie2.png"
"knob_small_hippie.png"
"knob_small_r_hippie.png"
"knob_mid_hippie.png"
"knob_big_hippie.png"

###PSYCEDELIC THEME
"psycedelic.png"

###GRANDMA THEME
"grandma.png"

###GRUNGY SUN THEME
"grungy_sun_dark.png"
"grungy_sun_dark2.png"

###GRUNGY SUN THEME
"grungy_sun.png"
"grungy_sun2.png"

###ALUMINIUM THEME
"aluminium1.png"
"aluminium2.png"

###OAK THEME
"oak1.png"
"oak2.png"

###BURL THEME
"burl1.png"
"burl2.png"

###WHITE OAK THEME
"oak_white.png"
#"oak2.png"

###GUITARIX OAK THEME
"oak_green.png"
#"oak2.png"

###DARK THEME
"dark.png"

###NEBULA THEMA
"nebula.png"
"nebula2.png"

###CAMOUFLAGE THEME
"camouflage.png"

###ORANGE THEME
"orange.png"

###RUST THEME
"rust.png"
"rust2.png"

###GOLD THEME
"gold1.png"
"gold2.png"
)

function(setup_scss FILES)
foreach(src IN LISTS FILES)
    message(${src})

    cmake_path(REPLACE_EXTENSION src "css" OUTPUT_VARIABLE dst)
    cmake_path(ABSOLUTE_PATH dst BASE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})
    message(${dst})
    
    # TODO add debug vs release mode arguments
    add_custom_command(
        OUTPUT ${dst}
        COMMAND ${SASSC_PROGRAM} ${src} ${dst}
        WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}
        MAIN_DEPENDENCY ${src}
    )

    # Create dependency of generated files on all to make sure they are
    # generated for install
    add_custom_target("target_${src}" ALL DEPENDS ${dst})
        
    install(FILES ${dst} DESTINATION share/gx_head/skins)
endforeach()
endfunction()

setup_scss("${scss_theme_files}")
setup_scss("${scss_additional_files}")

# TODO: skins are not working
install(FILES ${gxstyle_files} DESTINATION share/gx_head/skins)
