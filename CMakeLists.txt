cmake_minimum_required(VERSION 2.6)

project( libmfx )

# FIXME Adds support for using system/other install of intel media sdk
set(INTELMEDIASDK_PATH ${CMAKE_CURRENT_SOURCE_DIR})

add_definitions(-DMFX_MODULES_DIR=\".\" -DMFX_PLUGINS_CONF_DIR=\".\")

if (CMAKE_SYSTEM_NAME MATCHES "Linux")
  add_definitions(-fPIC)
  list(APPEND SOURCES
    src/mfxloader.cpp
    src/mfxparser.cpp
  )
endif (CMAKE_SYSTEM_NAME MATCHES "Linux")

if (CMAKE_SYSTEM_NAME MATCHES "Windows")
  add_definitions(-DWIN32)
  list(APPEND SOURCES
    src/main.cpp
    src/mfx_dispatcher.cpp
    src/mfx_dispatcher_log.cpp
    src/mfx_function_table.cpp
    src/mfx_critical_section.cpp
    src/mfx_dxva2_device.cpp
    src/mfx_library_iterator.cpp
    src/mfx_load_dll.cpp
    src/mfx_load_plugin.cpp
    src/mfx_plugin_hive.cpp
    src/mfx_win_reg_key.cpp
    src/mfx_driver_store_loader.cpp
  )
endif (CMAKE_SYSTEM_NAME MATCHES "Windows")

include_directories(
  ${INTELMEDIASDK_PATH}
  ${INTELMEDIASDK_PATH}/mfx
)

add_definitions(
  -DMFX_VA
  -D_LIB
)

configure_file (${CMAKE_CURRENT_SOURCE_DIR}/libmfx.pc.cmake ${CMAKE_BINARY_DIR}/libmfx.pc @ONLY)

add_library( mfx STATIC ${SOURCES} )
install (DIRECTORY ${INTELMEDIASDK_PATH}/mfx DESTINATION ${CMAKE_INSTALL_PREFIX}/include FILES_MATCHING PATTERN "*.h")
install (FILES ${CMAKE_BINARY_DIR}/libmfx.pc DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/pkgconfig)
install (TARGETS mfx ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
