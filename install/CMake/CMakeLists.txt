CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
 set(CMAKE_CXX_FLAGS "-std=c++1y ${CMAKE_CXX_FLAGS}")
 set(CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS "${CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS} -undefined dynamic_lookup")
endif()

SET(PROJECT_NAME "CATS3")

project(${PROJECT_NAME})
# SET PADLM_OmpToolsTHS #
SET(DLM_REPO "/home/raffaele/CATS2/DLM")
SET(CATS_INSTALL "/home/raffaele/CATS2/DLM/install")
SET(GSL_INCLUDE "/usr/include/gsl")
SET(GSL_LIB "/usr/lib/x86_64-linux-gnu")
SET(ROOT_PATH "/home/raffaele/root")

SET(VERSION_MAJOR 3)
SET(VERSION "${VERSION_MAJOR}")
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CATS_INSTALL})

include_directories(${GSL_INCLUDE})
include_directories(${DLM_REPO}/CATS)
include_directories(${DLM_REPO}/CATS_Extentions)
include_directories(${DLM_REPO}/DLM_CppTools)
include_directories(${DLM_REPO}/DLM_MathTools)
include_directories(${DLM_REPO}/DLM_RootTools)
include_directories(${DLM_REPO}/DLM_FemtoTools)
include_directories(${DLM_REPO}/CECA)
include_directories(${ROOT_PATH}/include)
add_library(CATS SHARED   
              ${DLM_REPO}/CATS/CATS.cpp
              ${DLM_REPO}/CATS/CATStools.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_Ck.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_ResponseMatrix.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_CkDecomp.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_Source.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_StefanoPotentials.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_Potentials.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_CkModels.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_WfModel.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_CkDecomposition.cpp
              ${DLM_REPO}/CATS_Extentions/DLM_Fitters.cpp
              ${DLM_REPO}/DLM_CppTools/DLM_CppTools.cpp
              ${DLM_REPO}/DLM_CppTools/DLM_OmpTools.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_Integration.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_Random.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_Bessel.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_MathFunctions.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_Fit.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_RootFinder.cpp
              ${DLM_REPO}/DLM_MathTools/DLM_Unfold.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_CRAB_PM.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_SubPads.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_DrawingTools.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_HistoAnalysis.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_RootWrapper.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_MultiFit.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_DecayMatrix.cpp
              ${DLM_REPO}/DLM_RootTools/DLM_RootFit.cpp
              ${DLM_REPO}/DLM_FemtoTools/CommonAnaFunctions.cpp
              ${DLM_REPO}/CECA/CECA.cpp
              ${DLM_REPO}/CECA/TREPNI.cpp
              )

execute_process(COMMAND bash -c "${ROOT_PATH}/bin/root-config --cflags" OUTPUT_VARIABLE CFLAGS)
execute_process(COMMAND bash -c "${ROOT_PATH}/bin/root-config --libs" OUTPUT_VARIABLE LIBS)
string(REGEX REPLACE "
$" "" CFLAGS "${CFLAGS}")
string(REGEX REPLACE "
$" "" LIBS "${LIBS}")
set(CFLAGS " -O2 -lgomp -pthread -fopenmp -std=c++11 ${CFLAGS}")
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
set_target_properties( CATS PROPERTIES COMPILE_FLAGS ${CFLAGS} ARCHIVE_OUTPUT_DIRECTORY "${CATS_INSTALL}/lib" LIBRARY_OUTPUT_DIRECTORY "${CATS_INSTALL}/lib" RUNTIME_OUTPUT_DIRECTORY "${CATS_INSTALL}/bin")
target_link_libraries(  CATS -L${GSL_LIB} -lgsl -lgslcblas ${LIBS})
install (FILES ${DLM_REPO}/CATS/CATS.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS/CATStools.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS/CATSconstants.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_Ck.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_ResponseMatrix.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_CkDecomp.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_Source.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_StefanoPotentials.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_Potentials.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_CkModels.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_WfModel.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_CkDecomposition.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CATS_Extentions/DLM_Fitters.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_CppTools/DLM_CppTools.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_CppTools/DLM_Sort.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_CppTools/DLM_OmpTools.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_Histo.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_Integration.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_Random.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_Bessel.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_MathFunctions.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_Fit.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_RootFinder.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_MathTools/DLM_Unfold.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_CRAB_PM.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_SubPads.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_DrawingTools.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_HistoAnalysis.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_RootWrapper.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_MultiFit.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_DecayMatrix.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_RootTools/DLM_RootFit.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/DLM_FemtoTools/CommonAnaFunctions.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CECA/CECA.h DESTINATION ${CATS_INSTALL}/include)
install (FILES ${DLM_REPO}/CECA/TREPNI.h DESTINATION ${CATS_INSTALL}/include)
install (PROGRAMS ${DLM_REPO}/bin/cats-config DESTINATION ${CATS_INSTALL}/bin)

file(WRITE ${CATS_INSTALL}/bin/CMakeDLM.txt "${PROJECT_NAME}\n")
file(APPEND ${CATS_INSTALL}/bin/CMakeDLM.txt "${CATS_INSTALL}\n")
file(APPEND ${CATS_INSTALL}/bin/CMakeDLM.txt "${ROOT_PATH}\n")
file(APPEND ${CATS_INSTALL}/bin/CMakeDLM.txt "${GSL_INCLUDE}\n")
file(APPEND ${CATS_INSTALL}/bin/CMakeDLM.txt "${GSL_LIB}\n")

