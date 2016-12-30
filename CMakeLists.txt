cmake_minimum_required (VERSION 3.0)

project (spherical_harmonics CXX)

add_compile_options (-std=c++14)

set (HDR_FILES
	${CMAKE_CURRENT_SOURCE_DIR}/sh/image.h
	${CMAKE_CURRENT_SOURCE_DIR}/sh/default_image.h
	${CMAKE_CURRENT_SOURCE_DIR}/sh/spherical_harmonics.h)

set (SRC_FILES
	${CMAKE_CURRENT_SOURCE_DIR}/sh/default_image.cc
	${CMAKE_CURRENT_SOURCE_DIR}/sh/spherical_harmonics.cc)

set (TEST_FILES
	${CMAKE_CURRENT_SOURCE_DIR}/sh/spherical_harmonics_test.cc)

add_definitions(-D_USE_MATH_DEFINES)
include_directories (${CMAKE_CURRENT_SOURCE_DIR} ${EIGEN_ROOT} ${GTEST_ROOT}/include)
link_directories (${GTEST_ROOT}/lib)
add_library (libsh STATIC ${HDR_FILES} ${SRC_FILES})
add_executable (test_sh ${TEST_FILES})
target_link_libraries (test_sh libsh gtest gtest_main)
set_property (TARGET libsh PROPERTY PUBLIC_HEADER ${HDR_FILES})

install (TARGETS libsh
	EXPORT spherical_harmonics-targets
	RUNTIME DESTINATION bin
	PUBLIC_HEADER DESTINATION include/sh
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)

install (EXPORT spherical_harmonics-targets DESTINATION cmake)
