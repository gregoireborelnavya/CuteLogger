CMAKE_MINIMUM_REQUIRED(VERSION 3.4)
CMAKE_POLICY(SET CMP0003 NEW)

PROJECT(cuteLogger)

IF(WIN32)
    SET(LIBRARY_OUTPUT_PATH C:/Users/gregoire.borel/Documents/external_libs/cuteLogger/dll CACHE STRING "Path to the bin output" FORCE)
    SET(Qt5Core_DIR C:/Qt/5.7/msvc2015_64/lib/cmake/Qt5Core)
ELSE(UNIX)
    SET(LIBRARY_OUTPUT_PATH /media/data/external_libs/cuteLogger/dll CACHE STRING "Path to the bin output" FORCE)
    SET(Qt5Core_DIR /opt/Qt/5.7/gcc_64/lib/cmake/Qt5Core)
ENDIF(WIN32)

if (${MSVC})
    if (${MSVC_VERSION} EQUAL 1800)
        set(NAVYA_COMPILER_VERSION "vc120-mt")
    elseif(${MSVC_VERSION} EQUAL 1900)
        set(NAVYA_COMPILER_VERSION "vc140-mt")
    endif(MSVC12)
endif(${CMAKE_CXX_COMPILER_ID} STREQUAL "MSVC")

ADD_DEFINITIONS(-DCUTELOGGER_LIBRARY)

FIND_PACKAGE(Qt5Core 5.7.0 REQUIRED)

INCLUDE_DIRECTORIES(BEFORE include)

SET(${PROJECT_NAME}_SOURCES
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
IF(WIN32)
  SET(${PROJECT_NAME}_SOURCES ${${PROJECT_NAME}_SOURCES}
      src/OutputDebugAppender.cpp include/OutputDebugAppender.h)
ENDIF(WIN32)

ADD_LIBRARY(${PROJECT_NAME} SHARED ${${PROJECT_NAME}_SOURCES})
TARGET_LINK_LIBRARIES(${PROJECT_NAME} Qt5::Core)
SET_TARGET_PROPERTIES(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "${PROJECT_NAME}-${NAVYA_COMPILER_VERSION}")
SET_TARGET_PROPERTIES(${PROJECT_NAME} PROPERTIES DEBUG_POSTFIX "d")
