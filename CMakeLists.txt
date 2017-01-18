cmake_minimum_required(VERSION 3.4)
cmake_policy(SET CMP0003 NEW)

project(cuteLogger)

# Find OS architecture
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(EX_PLATFORM 64)
else(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(EX_PLATFORM 32 )
endif(CMAKE_SIZEOF_VOID_P EQUAL 8)

if(WIN32)
    set(LIBRARY_OUTPUT_PATH C:/users/gregoire.borel/Documents/external_libs/cuteLogger/dll CACHE STRING "Path to the bin output" FORCE)
else(UNIX)
    set(LIBRARY_OUTPUT_PATH /media/data/external_libs/cuteLogger/dll CACHE STRING "Path to the bin output" FORCE)
    set(Qt5Core_DIR /opt/Qt/5.7/gcc_"${EX_PLATFORM}/lib/cmake/Qt5Core")
endif(WIN32)


if(${MSVC})
    if (${MSVC_VERSION} EQUAL 1800)
        set(NAVYA_COMPILER_VERSION "vc120-mt")
        set(Qt5Core_DIR "C:/Qt/5.7/msvc2013_${EX_PLATFORM}/lib/cmake/Qt5Core")
    elseif(${MSVC_VERSION} EQUAL 1900)
        set(NAVYA_COMPILER_VERSION "vc140-mt")
        set(Qt5Core_DIR "C:/Qt/5.7/msvc2015_${EX_PLATFORM}/lib/cmake/Qt5Core")
    endif(MSVC12)
endif(${CMAKE_CXX_COMPILER_ID} STREQUAL "MSVC")

add_definitions(-DCUTELOGGER_LIBRARY)

find_package(Qt5Core 5.7.0 REQUIRED)

include_directories(BEFORE include)

set(${PROJECT_NAME}_SOURCES
  src/Logger.cpp
  src/AbstractAppender.cpp
  src/AbstractStringAppender.cpp
  src/ConsoleAppender.cpp
  src/FileAppender.cpp
  src/RollingFileAppender.cpp

  include/Logger.h
  include/FileAppender.h
  include/CuteLogger_global.h
  include/ConsoleAppender.h
  include/AbstractStringAppender.h
  include/AbstractAppender.h
  include/RollingFileAppender.h
)

# OutputDebugAppender is only for Windows systems
if(WIN32)
  set(${PROJECT_NAME}_SOURCES ${${PROJECT_NAME}_SOURCES}
      src/OutputDebugAppender.cpp include/OutputDebugAppender.h)
endif(WIN32)

add_library(${PROJECT_NAME} SHARED ${${PROJECT_NAME}_SOURCES})
target_link_libraries(${PROJECT_NAME} Qt5::Core)
set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "${PROJECT_NAME}-${NAVYA_COMPILER_VERSION}")
set_target_properties(${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX "d")
