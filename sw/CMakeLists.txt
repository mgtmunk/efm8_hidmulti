project( read_test )
cmake_minimum_required(VERSION 2.8)

# header files
set (MAIN_INCLUDE ${PROJECT_SOURCE_DIR}) 
set (HIDAPI_INCLUDE ${PROJECT_SOURCE_DIR}/../lib/hidapi/hidapi) 

# source files

if (${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    set(HIDAPI_SOURCES ${PROJECT_SOURCE_DIR}/../lib/hidapi/windows/hid.c)
    set(EXTRA_LIBS ${IOKIT_LIBRARY} setupapi.lib)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    set(HIDAPI_SOURCES ${PROJECT_SOURCE_DIR}/../lib/hidapi/linux/hid.c)
    set(EXTRA_LIBS ${IOKIT_LIBRARY} udev)
elseif (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    set(HIDAPI_SOURCES ${PROJECT_SOURCE_DIR}/../lib/hidapi/mac/hid.c)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(CORE_FOUNDATION_LIBRARY CoreFoundation)
    mark_as_advanced(IOKIT_LIBRARY CORE_FOUNDATION_LIBRARY)
    set(EXTRA_LIBS ${IOKIT_LIBRARY} ${CORE_FOUNDATION_LIBRARY})
else ()
    message( FATAL_ERROR "The host build platform is not supported." )
endif()

include_directories(${HIDAPI_INCLUDE})
add_executable (read_test read_test.c ${HIDAPI_SOURCES})
target_link_libraries(read_test ${EXTRA_LIBS})