cmake_minimum_required(VERSION 3.13)
project(guitarix)

# Global options from cpu_optimization.py
set(CMAKE_CXX_STANDARD 20)

# FIXME: the NAM and RTNeural specific definitions should be moved to the
# library specific build files
add_compile_definitions(
    DSP_SAMPLE_FLOAT
    NAM_SAMPLE_FLOAT
    neural_amp_modeler_EXPORTS
    RTNEURAL_DEFAULT_ALIGNMENT=32
    RTNEURAL_USE_EIGEN=1
)

find_package(PkgConfig REQUIRED)

pkg_check_modules(GTK3 REQUIRED IMPORTED_TARGET gtk+-3.0)
pkg_check_modules(GTKMM REQUIRED IMPORTED_TARGET gtkmm-3.0)
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

# conf.check(features='cxx cxxshlib', lib=['pthread'], uselib_store='PTHREAD')
find_package(Threads REQUIRED)

# conf.check_cxx(msg = msg, fragment=code, lib="boost_system", uselib_store="BOOST_SYSTEM", mandatory=1,includes='/opt/local/include', libpath='/opt/local/lib')
# conf.check_cxx(msg = msg, fragment=code, lib=["boost_iostreams","boost_system"], uselib_store="BOOST_IOSTREAMS", mandatory=1)
find_package(Boost 1.42 COMPONENTS system iostreams)

# TODO use system version or FFMPEG version first
add_library(gx_ZITA_CONVOLVER STATIC)
add_library(gx::ZITA_CONVOLVER ALIAS gx_ZITA_CONVOLVER)
target_include_directories(gx_ZITA_CONVOLVER PUBLIC src/zita-convolver)
target_sources(gx_ZITA_CONVOLVER PRIVATE src/zita-convolver/zita-convolver.cc)
target_compile_options(gx_ZITA_CONVOLVER PRIVATE "-fvisibility=hidden")
target_link_libraries(gx_ZITA_CONVOLVER PRIVATE PkgConfig::FFTW3 Threads::Threads)

# TODO use system version first
add_library(gx_ZITA_RESAMPLER STATIC)
add_library(gx::ZITA_RESAMPLER ALIAS gx_ZITA_RESAMPLER)
target_include_directories(gx_ZITA_RESAMPLER PUBLIC src/zita-resampler-1.1.0)
target_sources(gx_ZITA_RESAMPLER PRIVATE
               src/zita-resampler-1.1.0/resampler.cc
               src/zita-resampler-1.1.0/resampler-table.cc)
target_compile_options(gx_ZITA_RESAMPLER PRIVATE "-fvisibility=hidden")
target_link_libraries(gx_ZITA_RESAMPLER PRIVATE Threads::Threads)

add_subdirectory(src/NAM)
add_subdirectory(src/plugins)

add_subdirectory(libgxw/gxw)
add_subdirectory(src/RTNeural)

add_subdirectory(libgxwmm)

add_subdirectory(src/faust)
add_subdirectory(src/gx_head)
