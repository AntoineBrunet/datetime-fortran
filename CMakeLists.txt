# cmake version, project name, language
cmake_minimum_required(VERSION 2.8 FATAL_ERROR)
project(datetime-fortran Fortran)

# compiler flags for gfortran
if(CMAKE_Fortran_COMPILER_ID MATCHES GNU)
  set(CMAKE_Fortran_FLAGS_DEBUG "-O0 -g -C -fbacktrace")
  set(CMAKE_Fortran_FLAGS_RELEASE "-O3")
endif()

# compiler flags for ifort
if(CMAKE_Fortran_COMPILER_ID MATCHES Intel)
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -assume realloc_lhs -heap-arrays")
  set(CMAKE_Fortran_FLAGS_DEBUG "-O0 -g -C -traceback")
  set(CMAKE_Fortran_FLAGS_RELEASE "-O3")
endif()

# library to archive (libdatetime.a)
add_library(datetime src/lib/datetime.f90 src/lib/mod_datetime.f90 src/lib/mod_timedelta.f90 src/lib/mod_clock.f90 src/lib/mod_strftime.f90 src/lib/mod_constants.f90)

# tests
if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
	add_executable(datetime_tests src/tests/datetime_tests.f90)
	target_link_libraries(datetime_tests datetime)
	add_test(datetime_tests bin/datetime_tests)
endif()
