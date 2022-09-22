## external project base directory for all projects in this hierarchy
set_property(DIRECTORY PROPERTY EP_BASE ${DEPENDENCIES_DIR})

#################################################
# Check embree dependency
#################################################
if(DEFINED ${embree_DIR})
  find_package(embree 3.0 REQUIRED
    PATHS ${embree_DIR})
  message(STATUS "embree_DIR: ${embree_DIR}")
else()
  find_package(embree 3.0)
  if(embree_FOUND)
    message(STATUS "Found embree at ${embree_DIR}.")
  else()
    message(STATUS "Couldn't find embree. Dependency will be built during the first invocation of the build system (e.g. `make`).")
    add_subdirectory(embree)
    message(STATUS "embree_DIR: ${embree_DIR}")
    set(EMBREE_RAY_MASK OFF)
  endif()
endif()
set(EMBREE_RAY_MASK ${EMBREE_RAY_MASK} CACHE BOOL "Whether embree was built with ray masking support" FORCE)