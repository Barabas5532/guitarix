find_package(PkgConfig REQUIRED)

pkg_check_modules(GTK3 REQUIRED IMPORTED_TARGET gtk+-3.0)

list(APPEND SOURCES "gxinit.cpp"
    "drawingutils.cpp"
    "GxFastMeter.cpp"
    "GxWaveView.cpp"
    "GxTuner.cpp"
    "GxRegler.cpp"
    "GxSelector.cpp"
    "GxSwitch.cpp"
    "GxToggleImage.cpp"
    "GxControlParameter.cpp"
    "GxIREdit.cpp"
    "GxRadioButton.cpp"
    "GxWheel.cpp"
    "GxWheelVertical.cpp"
    "GxKnob.cpp"
    "GxBigKnob.cpp"
    "GxMidKnob.cpp"
    "GxSmallKnob.cpp"
    "GxSmallKnobR.cpp"
    "GxHSlider.cpp"
    "GxMiniSlider.cpp"
    "GxVSlider.cpp"
    "GxEQSlider.cpp"
    "GxLevelSlider.cpp"
    "GxPaintBox.cpp"
    "GxMeterScale.cpp"
    "GxValueDisplay.cpp"
    "GxSimpleValueDisplay.cpp"
    "GxRackTuner.cpp"
    "GxPortDisplay.cpp"
    "GxPlayHead.cpp"
)

add_library(gxw STATIC)
add_library(gx::gxw ALIAS gxw)
target_sources(gxw PRIVATE ${SOURCES})
target_include_directories(gxw PRIVATE ".")
target_include_directories(gxw PUBLIC "..")

target_link_libraries(gxw PUBLIC PkgConfig::GTK3)
