# Boost 1.63 requires CMake 3.7 or newer
cmake_minimum_required(VERSION 3.7)

project(Slic3r)

if (MSVC OR MINGW OR APPLE)
	set(SLIC3R_STATIC_INITIAL 1)
else ()
	set(SLIC3R_STATIC_INITIAL 0)
endif ()

option(SLIC3R_STATIC 			"Compile Slic3r with static libraries (Boost, TBB, glew)" ${SLIC3R_STATIC_INITIAL})
option(SLIC3R_PROFILE 			"Compile Slic3r with an invasive Shiny profiler" 0)
option(SLIC3R_HAS_BROKEN_CROAK 	"Compile Slic3r for a broken Strawberry Perl 64bit" 0)
option(SLIC3R_MSVC_COMPILE_PARALLEL "Compile on Visual Studio in parallel" 1)

if (MSVC AND SLIC3R_MSVC_COMPILE_PARALLEL)
	set(CMAKE_C_FLAGS ${CMAKE_CXX_FLAGS} /MP)
	set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} /MP)
endif ()

add_subdirectory(xs)

install(PROGRAMS slic3r.pl DESTINATION bin RENAME slic3r-prusa3d)

file(GLOB MyVar var/*.png)
install(FILES ${MyVar} DESTINATION share/slic3r-prusa3d)
install(FILES lib/Slic3r.pm DESTINATION lib/slic3r-prusa3d)
install(DIRECTORY lib/Slic3r DESTINATION lib/slic3r-prusa3d)
