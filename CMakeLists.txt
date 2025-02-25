# (C) Copyright 2024 UCAR
#

cmake_minimum_required( VERSION 3.14 FATAL_ERROR )

find_package( ecbuild 3.6 REQUIRED HINTS ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/../ecbuild)

project( jedi-bundle VERSION 8.0.0 LANGUAGES C CXX Fortran )

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include( ecbuild_bundle )

# Default release mode
set( ECBUILD_DEFAULT_BUILD_TYPE Release )

# Enable OpenMP and MPI
set( ENABLE_MPI ON CACHE BOOL "Compile with MPI" )
set( ENABLE_OMP ON CACHE BOOL "Compile with OpenMP" )

# Define bundle
ecbuild_bundle_initialize()

# add the automatically determined parts of the RPATH
# which point to directories outside the build tree to the install RPATH
set(CMAKE_INSTALL_RPATH_USE_LINK_PATH ON)

# when building, already use the install RPATH
set(CMAKE_BUILD_WITH_INSTALL_RPATH ON)

# Use external jedi-cmake
include( $ENV{jedi_cmake_ROOT}/share/jedicmake/Functions/git_functions.cmake )

#ecbuild_bundle( PROJECT eckit    GIT "https://github.com/ecmwf/eckit.git" TAG 1.24.4 )
#ecbuild_bundle( PROJECT fckit    GIT "https://github.com/ecmwf/fckit.git" TAG 0.11.0 )
#ecbuild_bundle( PROJECT atlas    GIT "https://github.com/ecmwf/atlas.git" TAG 0.35.0 )

option(BUILD_GSIBEC "Build GSIbec" OFF)
if(BUILD_GSIBEC)
  ecbuild_bundle( PROJECT gsibec   GIT "https://github.com/geos-esm/GSIbec" TAG 1.2.1 )
endif()

ecbuild_bundle( PROJECT gsw      GIT "https://github.com/jcsda/GSW-Fortran.git" BRANCH develop UPDATE )

ecbuild_bundle( PROJECT oops     GIT "https://github.com/jcsda/oops.git"        BRANCH develop UPDATE )
ecbuild_bundle( PROJECT vader    GIT "https://github.com/jcsda/vader.git"       BRANCH develop UPDATE )
ecbuild_bundle( PROJECT saber    GIT "https://github.com/jcsda/saber.git"       BRANCH develop UPDATE )

ecbuild_bundle( PROJECT crtm     GIT "https://github.com/jcsda/CRTMv3.git"      BRANCH develop UPDATE )

ecbuild_bundle( PROJECT ioda     GIT "https://github.com/jcsda/ioda.git"        BRANCH develop UPDATE )
ecbuild_bundle( PROJECT ufo      GIT "https://github.com/jcsda/ufo.git"         BRANCH develop UPDATE )

# Build IODA converters if requested
option(BUILD_IODA_CONVERTERS "Build IODA Converters" OFF)
if(BUILD_IODA_CONVERTERS)
  ecbuild_bundle( PROJECT iodaconv GIT "https://github.com/jcsda/ioda-converters.git" BRANCH develop UPDATE )
endif()

ecbuild_bundle( PROJECT fv3         GIT "https://github.com/jcsda/GFDL_atmos_cubed_sphere.git"       BRANCH release-stable UPDATE )
ecbuild_bundle( PROJECT femps       GIT "https://github.com/jcsda/femps.git"                         BRANCH develop UPDATE )
ecbuild_bundle( PROJECT fv3-jedi-lm GIT "https://github.com/jcsda/fv3-jedi-linearmodel.git"          BRANCH develop UPDATE )
ecbuild_bundle( PROJECT fv3-jedi    GIT "https://github.com/jcsda/fv3-jedi.git"                      BRANCH develop UPDATE )

ecbuild_bundle( PROJECT mom6        GIT "https://github.com/jcsda/MOM6.git"            BRANCH main-ecbuild UPDATE RECURSIVE )
ecbuild_bundle( PROJECT soca        GIT "https://github.com/jcsda/soca.git"            BRANCH develop UPDATE )

set(MPAS_DOUBLE_PRECISION "ON" CACHE STRING "MPAS-Model: Use double precision 64-bit Floating point.")
set(MPAS_CORES init_atmosphere atmosphere CACHE STRING "MPAS-Model: cores to build.")
ecbuild_bundle( PROJECT mpas           GIT "https://github.com/jcsda/MPAS-Model.git"   BRANCH release-stable UPDATE )
ecbuild_bundle( PROJECT mpas-jedi      GIT "https://github.com/jcsda/mpas-jedi.git"    BRANCH develop UPDATE )

ecbuild_bundle( PROJECT coupling   GIT "https://github.com/jcsda/coupling.git"         BRANCH develop UPDATE )

ecbuild_bundle_finalize()

