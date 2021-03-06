include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
set(COLLADA_LIBS minizip ${libpcrecpp_LIBRARIES} ${ZLIB_LIBRARIES} ${Boost_FILESYSTEM_LIBRARY} ${Boost_SYSTEM_LIBRARY})

set(COLLADA14_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include/1.4)
set(COLLADA15_INCLUDE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/include/1.5)
file(GLOB dae_files ${CMAKE_CURRENT_SOURCE_DIR}/src/dae/*.cpp)
if( LIBXML2_FOUND )
  file(GLOB libxmlplugin_files ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/LIBXMLPlugin/*.cpp)
  set(COLLADA_LIBS ${COLLADA_LIBS} ${LIBXML2_LIBRARIES})
else()
  set(libxmlplugin_files)
endif()
file(GLOB stddatabase_files ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/STLDatabase/*.cpp)
file(GLOB stderrplugin_files ${CMAKE_CURRENT_SOURCE_DIR}/src/modules/stdErrPlugin/*.cpp)

set(COLLADA_BASE_SOURCES ${dae_files} ${libxmlplugin_files} ${stddatabase_files} ${stderrplugin_files})
set(COLLADA_COMPILE_FLAGS "${libpcrecpp_CFLAGS_OTHERS} ${EXTRA_COMPILE_FLAGS} ${Boost_CFLAGS}")
# create dynamic libraries
set(COLLADA_COMPILE_FLAGS "${COLLADA_COMPILE_FLAGS} -DDOM_DYNAMIC -DDOM_EXPORT")

if( OPT_COLLADA15 )
  add_subdirectory(src/1.5)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/1.5 DESTINATION ${COLLADA_DOM_INCLUDE_INSTALL_DIR} COMPONENT ${COMPONENT_PREFIX}15dom-dev  PATTERN ".svn" EXCLUDE PATTERN ".~" EXCLUDE)
endif()
if( OPT_COLLADA14 )
  add_subdirectory(src/1.4)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/1.4 DESTINATION ${COLLADA_DOM_INCLUDE_INSTALL_DIR} COMPONENT ${COMPONENT_PREFIX}14dom-dev  PATTERN ".svn" EXCLUDE PATTERN ".~" EXCLUDE)
endif()

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/dae DESTINATION ${COLLADA_DOM_INCLUDE_INSTALL_DIR} COMPONENT ${COMPONENT_PREFIX}dom-dev PATTERN ".svn" EXCLUDE PATTERN ".~" EXCLUDE)
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/modules DESTINATION ${COLLADA_DOM_INCLUDE_INSTALL_DIR} COMPONENT ${COMPONENT_PREFIX}dom-dev  PATTERN ".svn" EXCLUDE PATTERN ".~" EXCLUDE)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/include/dae.h ${CMAKE_CURRENT_SOURCE_DIR}/include/dom.h DESTINATION ${COLLADA_DOM_INCLUDE_INSTALL_DIR} COMPONENT ${COMPONENT_PREFIX}dom-dev )
