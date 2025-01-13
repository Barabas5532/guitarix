list(APPEND SRC
     "./NeuralAmpModelerCore/NAM/activations.cpp"
     "./NeuralAmpModelerCore/NAM/convnet.cpp"
     "./NeuralAmpModelerCore/NAM/dsp.cpp"
     "./NeuralAmpModelerCore/NAM/get_dsp.cpp"
     "./NeuralAmpModelerCore/NAM/lstm.cpp"
     "./NeuralAmpModelerCore/NAM/util.cpp"
     "./NeuralAmpModelerCore/NAM/wavenet.cpp"
)

list(APPEND INCL
     "../headers"
     ".."
     "./NeuralAmpModelerCore/NAM/"
     "./NeuralAmpModelerCore/Dependencies/eigen/"
     "./NeuralAmpModelerCore/Dependencies/nlohmann/"
)

add_library(gx_nam STATIC)
add_library(gx::nam ALIAS gx_nam)
target_include_directories(gx_nam PUBLIC ${INCL})
target_sources(gx_nam PRIVATE ${SRC})
set_target_properties(gx_nam PROPERTIES CXX_STANDARD 17)
target_compile_options(gx_nam PRIVATE "-Wno-sign-compare" "-Ofast")

# TODO LTO