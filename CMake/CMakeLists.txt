CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

#if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
# set(CMAKE_CXX_FLAGS "-std=c++1y ${CMAKE_CXX_FLAGS}")
# set(CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS "${CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS} -undefined dynamic_lookup")
#endif()

#It is recommended that you copy all CMake related files in a local folder of yours,
#else whenever you pull/push there will be conflicts to resolve

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

SET(PROJECT_NAME "CATS")

project(${PROJECT_NAME})
# SET PATHS #
SET(CATS_DESTINATION "/Users/lbariogl/TUM/Software/CATS/cats_install")#where to install CATS. Folder must exist.
SET(ROOT_INCLUDE "/Users/lbariogl/TUM/Software/ROOT/root_install/include")#where all ROOT .h files are
SET(ROOT_LIB "/Users/lbariogl/TUM/Software/ROOT/root_install/lib")#where the ROOT related .a and .so files are
SET(ROOT_PATH "/Users/lbariogl/TUM/Software/ROOT/root_install")#path to ROOT
SET(GSL_INCLUDE "/usr/local/include")#where all GSL related .h files are
SET(GSL_INCLUDE_2 "/usr/local/include/gsl")#where all GSL related .h files are
SET(GSL_LIB "/usr/local/lib")#where are the GSL .a and .so files
SET(CATS_FOLDER "/Users/lbariogl/TUM/Software/CATS/cats_src")#where is the CATS main folder located
# DO NOT CHANGE THE REST #

SET(VERSION_MAJOR 3)
SET(VERSION_MINOR 0)
SET(VERSION_PATCH 0)
SET(VERSION "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CATS_DESTINATION})

include_directories(${CATS_FOLDER}/CATS)
include_directories(${CATS_FOLDER}/DLM_RootTools)
include_directories(${CATS_FOLDER}/DLM_MathTools)
include_directories(${CATS_FOLDER}/DLM_CppTools)
include_directories(${GSL_INCLUDE})
include_directories(${GSL_INCLUDE_2})
include_directories(${ROOT_PATH}/include)

add_library(CATSbasic SHARED 		${CATS_FOLDER}/CATS/CATS.cpp 
					${CATS_FOLDER}/CATS/CATStools.cpp 
					${CATS_FOLDER}/DLM_CppTools/DLM_CppTools.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Random.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Histo.cpp)

add_library(CATSextended SHARED ${CATS_FOLDER}/DLM_RootTools/DLM_CRAB_PM.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Integration.cpp
          #${CATS_FOLDER}/DLM_RootTools/DLM_DecayMatrix.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Random.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Bessel.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_MathFunctions.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_CkModels.cpp 
					${CATS_FOLDER}/CATS_Extentions/DLM_Potentials.cpp 
					${CATS_FOLDER}/CATS_Extentions/DLM_Source.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_StefanoPotentials.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_ResponseMatrix.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_Ck.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_CkDecomp.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_WfModel.cpp)					

add_library(CATSdev	SHARED ${CATS_FOLDER}/CATS_Extentions/DLM_CkDecomposition.cpp 
					${CATS_FOLDER}/CATS_Extentions/DLM_CkDecomp.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_Fitters.cpp 
					${CATS_FOLDER}/CATS_Extentions/DLM_ResponseMatrix.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_CkModels.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_Potentials.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_Source.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Integration.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_Bessel.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_CRAB_PM.cpp
					${CATS_FOLDER}/DLM_MathTools/DLM_MathFunctions.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_StefanoPotentials.cpp
					${CATS_FOLDER}/CATS_Extentions/DLM_Ck.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_SubPads.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_DrawingTools.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_HistoAnalysis.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_RootWrapper.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_DecayMatrix.cpp
					${CATS_FOLDER}/DLM_RootTools/DLM_MultiFit.cpp
					)

execute_process(COMMAND bash -c "${ROOT_PATH}/bin/root-config --cflags" OUTPUT_VARIABLE CFLAGS)
execute_process(COMMAND bash -c "${ROOT_PATH}/bin/root-config --libs" OUTPUT_VARIABLE LIBS)

#execute_process(COMMAND bash -c "${ROOT_PATH}/bin/root-config --glibs" OUTPUT_VARIABLE GLIBS)
#execute_process(COMMAND bash -c "${ROOT_PATH}/bin/root-config --incdir" OUTPUT_VARIABLE ROOT_INCLUDE)
#execute_process(COMMAND bash -c "${CATS_FOLDER}/cats-config --incdir" OUTPUT_VARIABLE CATS_INCLUDE)
#execute_process(COMMAND bash -c "${CATS_FOLDER}/cats-config --libs-${CATS_TYPE}" OUTPUT_VARIABLE CATS_LIBS)

string(REGEX REPLACE "\n$" "" CFLAGS "${CFLAGS}")
string(REGEX REPLACE "\n$" "" LIBS "${LIBS}")

#string(REGEX REPLACE "\n$" "" GLIBS "${GLIBS}")
#string(REGEX REPLACE "\n$" "" ROOT_INCLUDE "${ROOT_INCLUDE}")
#string(REGEX REPLACE "\n$" "" CATS_INCLUDE "${CATS_INCLUDE}")
#string(REGEX REPLACE "\n$" "" CATS_LIBS "${CATS_LIBS}")

#string(APPEND CFLAGS " -O2 -Wall -lgomp -pthread -fopenmp -std=c++11")
#string(APPEND CFLAGS " -O2 -pthread ")

#find_package(OpenMP)
#if (OPENMP_FOUND)
#    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
#    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
#    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
#endif()

set_target_properties( 	CATSbasic CATSextended CATSdev PROPERTIES COMPILE_FLAGS ${CFLAGS} ARCHIVE_OUTPUT_DIRECTORY "${CATS_DESTINATION}/lib" LIBRARY_OUTPUT_DIRECTORY "${CATS_DESTINATION}/lib" RUNTIME_OUTPUT_DIRECTORY "${CATS_DESTINATION}/bin")

target_link_libraries(  CATSbasic -L${GSL_LIB} -Xpreprocessor -fopenmp -lomp -lgsl -lgslcblas ${LIBS})
target_link_libraries(  CATSextended -L${GSL_LIB} -Xpreprocessor -fopenmp -lomp -lgsl -lgslcblas CATSbasic ${LIBS})
target_link_libraries(  CATSdev -L${GSL_LIB} -Xpreprocessor -fopenmp -lomp -lgsl -lgslcblas CATSbasic ${LIBS})

#install (TARGETS CATSbasic DESTINATION ${CATS_DESTINATION}/lib)
#install (TARGETS CATSextended DESTINATION ${CATS_DESTINATION}/lib)
#install (TARGETS CATSdev DESTINATION ${CATS_DESTINATION}/lib)

install (FILES ${CATS_FOLDER}/CATS/CATS.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS/CATStools.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS/CATSconstants.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_RootTools/DLM_CRAB_PM.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_RootTools/DLM_SubPads.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_RootTools/DLM_DrawingTools.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_RootTools/DLM_DecayMatrix.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_MathTools/DLM_Integration.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_MathTools/DLM_Random.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_MathTools/DLM_Histo.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_MathTools/DLM_Bessel.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_MathTools/DLM_MathFunctions.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_CkDecomp.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_CkDecomposition.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_CkModels.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_Fitters.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_Potentials.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_ResponseMatrix.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_Source.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_Ck.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_StefanoPotentials.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/CATS_Extentions/DLM_WfModel.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_RootTools/DLM_RootWrapper.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_RootTools/DLM_HistoAnalysis.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_CppTools/DLM_Sort.h DESTINATION ${CATS_DESTINATION}/include)
install (FILES ${CATS_FOLDER}/DLM_CppTools/DLM_CppTools.h DESTINATION ${CATS_DESTINATION}/include)
install (PROGRAMS ${CATS_FOLDER}/bin/cats-config DESTINATION ${CATS_DESTINATION}/bin)

file(WRITE ${CATS_DESTINATION}/bin/CMakeDLM.txt "${PROJECT_NAME}\n")
file(APPEND ${CATS_DESTINATION}/bin/CMakeDLM.txt "${CATS_DESTINATION}\n")
file(APPEND ${CATS_DESTINATION}/bin/CMakeDLM.txt "${ROOT_PATH}\n")
file(APPEND ${CATS_DESTINATION}/bin/CMakeDLM.txt "${GSL_INCLUDE}\n")
file(APPEND ${CATS_DESTINATION}/bin/CMakeDLM.txt "${GSL_INCLUDE_2}\n")
file(APPEND ${CATS_DESTINATION}/bin/CMakeDLM.txt "${GSL_LIB}\n")


