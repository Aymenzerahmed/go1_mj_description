cmake_minimum_required(VERSION 3.1)

project(go1_mj_description VERSION 1.0.0)

if(NOT TARGET mc_mujoco)
  find_package(mc_mujoco REQUIRED)
endif()

option(SRC_MODE "Use files in the repository instead of installed files" OFF)

set(Go1_XML_MODEL_PATH "xml/go1.xml")
set(Go1_PDGAINS_PATH "pdgains/PDgains_sim.dat")

if(SRC_MODE)
  set(Go1_XML_MODEL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/${Go1_XML_MODEL_PATH}")
  set(Go1_PDGAINS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/${Go1_PDGAINS_PATH}")
else()
  set(Go1_XML_MODEL_PATH "${MC_MUJOCO_SHARE_DESTINATION}/Go1/${Go1_XML_MODEL_PATH}")
  set(Go1_PDGAINS_PATH "${MC_MUJOCO_SHARE_DESTINATION}/Go1/${Go1_PDGAINS_PATH}")
endif()

configure_file(mc_mujoco/go1.in.yaml "${CMAKE_CURRENT_BINARY_DIR}/mc_mujoco/go1.yaml")
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/mc_mujoco/go1.yaml" DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}")

install(DIRECTORY meshes DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/Go1" FILES_MATCHING PATTERN "*.stl")
install(DIRECTORY pdgains DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/Go1" FILES_MATCHING PATTERN "*.dat")
install(DIRECTORY xml DESTINATION "${MC_MUJOCO_SHARE_DESTINATION}/Go1" FILES_MATCHING PATTERN "*.xml")
