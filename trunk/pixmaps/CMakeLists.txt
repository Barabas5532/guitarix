list(APPEND pixmap_files
    "gx_on.png"
    "gx_off.png"
    "jackd_on.png"
    "jackd_off.png"
    "insert_on.png"
    "insert_off.png"
    "gx_bypass.png"
    "gx_head.png"
    #"gx_head-midi.png" # no currently used
    #"gx_head-warn.png"
    "gx_log_grey.png"
    "gx_log_yellow.png"
    "gx_log_red.png"
    "gx_splash.png"
    )
install(FILES ${pixmap_files} DESTINATION share/pixmaps)
