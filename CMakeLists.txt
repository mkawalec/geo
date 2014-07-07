cmake_minimum_required(VERSION 2.8)
project(geo)
include_directories(src include)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/")

set(HEADER_FILES 
    )

set(CPP_FILES
    src/main.cpp
    ${HEADER_FILES}
    )


message(status "${CMAKE_CURRENT_SOURCE_DIR}")
add_executable(db ${CPP_FILES})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -Wall -Wextra -Wno-ignored-qualifiers -pedantic -W -std=c++0x -g")

unset(Boost_INCLUDE_DIR CACHE)
unset(Boost_LIBRARY_DIRS CACHE)
set(Boost_USE_STATIC_LIBS        OFF)
set(Boost_USE_MULTITHREADED      ON)
set(Boost_USE_STATIC_RUNTIME     OFF)
set(BOOST_ALL_DYN_LINK           ON)   # force dynamic linking for all libraries


find_package(Boost 1.54 COMPONENTS system program_options date_time REQUIRED)
message(status "** Boost Include: ${Boost_INCLUDE_DIR}")
message(status "** Boost Libraries: ${Boost_LIBRARY_DIRS}")
message(status "** Boost Libraries: ${Boost_LIBRARIES}")

INCLUDE_DIRECTORIES(
                 ${INCLUDE_DIRECTORIES}
                 ${Boost_INCLUDE_DIR} 
                 )
LINK_DIRECTORIES(
                 ${Boost_LIBRARY_DIRS} 
                 ${FFTW_LIBRARIES} 
                 )

target_link_libraries(db -lm 
                                    ${Boost_LIBRARIES} 
                     )

find_package(Doxygen)
if(DOXYGEN_FOUND)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile @ONLY)
    add_custom_target(doc
        ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating API documentation with Doxygen" VERBATIM
        )
endif(DOXYGEN_FOUND)
