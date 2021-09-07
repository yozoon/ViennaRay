cmake_minimum_required(VERSION 3.4)

# lowercase viennaRay is the python export
project("ViennaRayPython"
        VERSION ${VIENNARAY_VERSION})

message(STATUS "ViennaRay version: ${VIENNARAY_VERSION}")

# include viennaRay
set(ViennaRay_DIR "${ViennaRay_BINARY_DIR}")
set(ViennaRay_COMMON_TARGET ${PROJECT_NAME})
set(embree_DIR "${embree_DIR}")

set(VIENNARAY_PYTHON_SOURCE "pyWrap.cpp")
set(VIENNARAY_PYTHON_MODULE_NAME "viennaRay")

# message(STATUS "Pybind11: ${pybind11_DIR}")

SET(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)

if(VIENNARAY_BUILD_PYTHON_3)
  set(PYBIND11_PYTHON_VERSION 3 CACHE STRING "Python version")

#   message("embree_dir: " ${embree_DIR})
#   find_package(embree 3.0 PATHS ${embree_DIR} REQUIRED)
  find_package(ViennaRay REQUIRED PATHS ${ViennaRay_DIR} NO_DEFAULT_PATH)
  find_package(pybind11 REQUIRED PATHS ${pybind11_DIR})
  list(APPEND VIENNARAY_LIBRARIES embree)

  #make 3.6 python module
  # define dimension for build
  set(VIENNARAY_PYTHON_MODULE_NAME_2D "${VIENNARAY_PYTHON_MODULE_NAME}2d")

  pybind11_add_module(${VIENNARAY_PYTHON_MODULE_NAME_2D} ${VIENNARAY_PYTHON_SOURCE})
  target_include_directories(${VIENNARAY_PYTHON_MODULE_NAME_2D} PUBLIC ${VIENNARAY_INCLUDE_DIRS})
  target_link_libraries(${VIENNARAY_PYTHON_MODULE_NAME_2D} PRIVATE ${VIENNARAY_LIBRARIES})
  #define compile dimension
  target_compile_definitions(${VIENNARAY_PYTHON_MODULE_NAME_2D} PRIVATE -DVIENNARAY_PYTHON_DIMENSION=2)

  install(TARGETS ${VIENNARAY_PYTHON_MODULE_NAME_2D}
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX})

  # define dimension for build
  set(VIENNARAY_PYTHON_MODULE_NAME_3D "${VIENNARAY_PYTHON_MODULE_NAME}3d")

  pybind11_add_module(${VIENNARAY_PYTHON_MODULE_NAME_3D} ${VIENNARAY_PYTHON_SOURCE})
  target_include_directories(${VIENNARAY_PYTHON_MODULE_NAME_3D} PUBLIC ${VIENNARAY_INCLUDE_DIRS})
  target_link_libraries(${VIENNARAY_PYTHON_MODULE_NAME_3D} PRIVATE ${VIENNARAY_LIBRARIES})
  #define compile dimension
  target_compile_definitions(${VIENNARAY_PYTHON_MODULE_NAME_3D} PRIVATE -DVIENNARAY_PYTHON_DIMENSION=3)

  install(TARGETS ${VIENNARAY_PYTHON_MODULE_NAME_3D}
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX})
endif()

if(VIENNARAY_BUILD_PYTHON_2 AND NOT VIENNARAY_BUILD_PYTHON_3)
  set(PYBIND11_PYTHON_VERSION 2 CACHE STRING "Python version")

  # disable register warning, as Pyhton.h of Python 2 includes register variables
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-register")

  find_package(embree 3.0 PATHS ${embree_DIR} REQUIRED)
  find_package(ViennaRay REQUIRED PATHS ${ViennaRay_DIR} NO_DEFAULT_PATH)
  find_package(pybind11 REQUIRED PATHS ${pybind11_DIR})
  list(APPEND VIENNARAY_LIBRARIES embree)

  #make 2.7 python module
  set(VIENNARAY_PYTHON_MODULE_NAME_2D "${VIENNARAY_PYTHON_MODULE_NAME}2d")

  pybind11_add_module(${VIENNARAY_PYTHON_MODULE_NAME_2D} ${VIENNARAY_PYTHON_SOURCE})
  target_include_directories(${VIENNARAY_PYTHON_MODULE_NAME_2D} PUBLIC ${VIENNARAY_INCLUDE_DIRS})
  target_link_libraries(${VIENNARAY_PYTHON_MODULE_NAME_2D} PRIVATE ${VIENNARAY_LIBRARIES})
  #define compile dimension
  target_compile_definitions(${VIENNARAY_PYTHON_MODULE_NAME_2D} PRIVATE -DVIENNARAY_PYTHON_DIMENSION=2)

  install(TARGETS ${VIENNARAY_PYTHON_MODULE_NAME_2D}
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX})

  # define dimension for build
  set(VIENNARAY_PYTHON_MODULE_NAME_3D "${VIENNARAY_PYTHON_MODULE_NAME}3d")

  pybind11_add_module(${VIENNARAY_PYTHON_MODULE_NAME_3D} ${VIENNARAY_PYTHON_SOURCE})
  target_include_directories(${VIENNARAY_PYTHON_MODULE_NAME_3D} PUBLIC ${VIENNARAY_INCLUDE_DIRS})
  target_link_libraries(${VIENNARAY_PYTHON_MODULE_NAME_3D} PRIVATE ${VIENNARAY_LIBRARIES})
  #define compile dimension
  target_compile_definitions(${VIENNARAY_PYTHON_MODULE_NAME_3D} PRIVATE -DVIENNARAY_PYTHON_DIMENSION=3)

  install(TARGETS ${VIENNARAY_PYTHON_MODULE_NAME_3D}
    LIBRARY DESTINATION ${CMAKE_INSTALL_PREFIX})
endif()