# ----------------------------------------------------------------------------
#  CMake file for libpng. See root CMakeLists.txt
#
# ----------------------------------------------------------------------------
project(libpng)

# List of C++ files:

include_directories("${CMAKE_CURRENT_SOURCE_DIR}")

if(ZLIB_FOUND)
    include_directories(${ZLIB_INCLUDE_DIR})
else()
    include_directories("${CMAKE_CURRENT_SOURCE_DIR}/../zlib")
endif()

file(GLOB lib_srcs *.c)
file(GLOB lib_hdrs *.h)

# ----------------------------------------------------------------------------------
# 				Define the library target:
# ----------------------------------------------------------------------------------

set(the_target "png")

add_library(${the_target} STATIC ${lib_srcs} ${lib_hdrs})

if(MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /W3")
endif()

if(UNIX)
    if(CMAKE_COMPILER_IS_GNUCXX OR CV_ICC)
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")
    endif()
endif()

install(TARGETS ${the_target} EXPORT png-exports DESTINATION lib)
install(FILES ${lib_hdrs} DESTINATION include)
