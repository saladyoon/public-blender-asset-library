cmake_minimum_required(VERSION 3.10..3.31.11)


# Current Blender version
set(BLENDER_VERSION_MAJOR "5")
set(BLENDER_VERSION_MINOR "2")
set(BLENDER_VERSION_PATCH "0")
set(BLENDER_VERSION
  "${BLENDER_VERSION_MAJOR}.${BLENDER_VERSION_MINOR}.${BLENDER_VERSION_PATCH}"
)


# Project info
project("Public Blender Asset Library"
  VERSION 0.1.0
  DESCRIPTION "My public asset library for Blender ${BLENDER_VERSION}."
)

set(PROJECT_CONTACT "saladyoon.sinecauseguy@gmail.com")
set(PROJECT_VENDOR "Georgy \"Salad Moon\" Tolstenko")


# Ignore out-of-source build directories
if(NOT PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
  file(WRITE ${CMAKE_BINARY_DIR}/.gitignore "*")
endif()


# Package info
set(CPACK_SYSTEM_NAME "${CMAKE_SYSTEM_NAME}")
set(CPACK_PACKAGE_DESCRIPTION "${PROJECT_DESCRIPTION}")
set(CPACK_PACKAGE_VENDOR "${PROJECT_VENDOR}")
set(CPACK_PACKAGE_CONTACT "${PROJECT_CONTACT}")
set(CPACK_RESOURCE_FILE_README "${CMAKE_SOURCE_DIR}/README")
set(CPACK_RESOURCE_FILE_LICENSE "${CMAKE_SOURCE_DIR}/COPYING")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}")

set(CPACK_PACKAGE_NAME "saladyoon-blender-asset-library")


# Generators
set(CPACK_GENERATOR "ZIP")

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
  list(APPEND CPACK_GENERATOR "RPM")

  # RPM package
  set(CPACK_RPM_PACKAGE_ARCHITECTURE "noarch")
  set(CPACK_RPM_PACKAGE_RELEASE 1)
  set(CPACK_RPM_PACKAGE_RELEASE_DIST ON)
  set(CPACK_RPM_PACKAGE_LICENSE "CC0-1.0")
endif()

include(CPack)


# Component groups
cpack_add_component_group("nodes"
  DISPLAY_NAME "Node groups"
  DESCRIPTION "A collection of node groups for shading, compositing and
  procedural generation."
)


# Components
cpack_add_component("common"
  DISPLAY_NAME "Common"
  DESCRIPTION "Common files and categories."
  REQUIRED
)

cpack_add_component("documentation"
  DISPLAY_NAME "Documentation"
  DESCRIPTION "Complete documentation in Markdown format."
  DISABLED
)

cpack_add_component("preview"
  DISPLAY_NAME "Preview library"
  DESCRIPTION "Assets for creating preview images for this library."
)

cpack_add_component("shading-common"
  DISPLAY_NAME "Common shading library"
  DESCRIPTION "Common shading nodes."
  GROUP "nodes"
  DEPENDS "preview"
)

cpack_add_component("compositing-common"
  DISPLAY_NAME "Common compositing library"
  DESCRIPTION "Common compositing nodes."
  GROUP "nodes"
  DEPENDS "preview"
)

cpack_add_component("procedural-common"
  DISPLAY_NAME "Common procedural library"
  DESCRIPTION "Common procedural nodes."
  GROUP "nodes"
  DEPENDS "preview"
)


# All the common directories
set(CMAKE_SOURCE_DATADIR "${CMAKE_SOURCE_DIR}/share")
set(CMAKE_SOURCE_DOCDIR "${CMAKE_SOURCE_DIR}/share/doc")


# Common files
install(
  FILES "${CMAKE_SOURCE_DATADIR}/blender_assets.cats.txt" "README" "COPYING"
  DESTINATION "share/${CPACK_PACKAGE_NAME}/"
  COMPONENT "common"
)


# Documentation
install(
  DIRECTORY "${CMAKE_SOURCE_DOCDIR}/"
  DESTINATION "share/doc/${CPACK_PACKAGE_NAME}/"
  COMPONENT "documentation"
)


# Preview library
install(
  FILES "${CMAKE_SOURCE_DATADIR}/preview.blend"
  DESTINATION "share/${CPACK_PACKAGE_NAME}/"
  COMPONENT "preview"
)


# Shading common library
install(
  FILES "${CMAKE_SOURCE_DATADIR}/nodes/shading-common.blend"
  DESTINATION "share/${CPACK_PACKAGE_NAME}/nodes"
  COMPONENT "shading-common"
)


# Compositing common library
install(
  FILES "${CMAKE_SOURCE_DATADIR}/nodes/compositing-common.blend"
  DESTINATION "share/${CPACK_PACKAGE_NAME}/nodes"
  COMPONENT "compositing-common"
)


# Procedural common library
install(
  FILES "${CMAKE_SOURCE_DATADIR}/nodes/procedural-common.blend"
  DESTINATION "share/${CPACK_PACKAGE_NAME}/nodes"
  COMPONENT "procedural-common"
)
