## external project base directory for all projects in this hierarchy
set_property(DIRECTORY PROPERTY EP_BASE ${DEPENDENCIES_DIR})

#################################################
# Check embree dependency
#################################################
if(NOT embree_FOUND)
  add_subdirectory(embree)
  message(STATUS "embree_DIR: ${embree_DIR}")
endif()