cmake_minimum_required( VERSION 2.8 )

project( SimplexMesh )

find_package( ITK REQUIRED) 
include( ${USE_ITK_FILE} ) 

include_directories( ${CMAKE_SOURCE_DIR}/src )
include_directories( ${PointInCircle_DIR} )
include_directories( ${WalkInTriangulation_DIR} )
include_directories( ${DelaunayTriangulation_DIR} )

# add source file
#
set( SimplexMesh_src
  src/SimplexMesh.cxx
  ${predicates.c_location}
  ${DelaunayTriangulation_DIR}/itkPointSetToDelaunayTriangulationFilter.hxx  
  ${DelaunayTriangulation_DIR}/itkPointSetToMeshFilter.hxx
  )

# add header
#
set( SimplexMesh_header
  src/itkQuadEdgeMeshToQuadEdgeMeshWithDualFilter.h 
  src/itkQuadEdgeMeshWithDual.h
  src/itkQuadEdgeMeshWithDualAdaptor.h
  src/itkBarycentreDualPointFunctor.h
  src/itkCircumcentreDualPointFunctor.h
  ${WalkInTriangulation_DIR}/itkWalkInTriangulationFunction.h
  ${PointInCircle_DIR}/itkPointInCircleGeometricalPredicateFunctor.h
  ${DelaunayTriangulation_DIR}/itkPointSetToDelaunayTriangulationFilter.h
  ${DelaunayTriangulation_DIR}/itkPointSetToMeshFilter.h
  )

# if generate by Xcode add headers to executable
#
if( ${CMAKE_GENERATOR} MATCHES "Xcode" )
  set( SimplexMesh_src
    ${SimplexMesh_src}
    ${SimplexMesh_header}
    )
endif()

# create the executable that will be used by all tests
#
add_executable( SimplexMesh
  ${SimplexMesh_src}
  )

target_link_libraries( SimplexMesh
  ${ITK_LIBRARIES}
  )

# test
#
include( CTest )

add_test( SimplexMesh01
  SimplexMesh plan
  )
add_test( SimplexMesh02
  SimplexMesh sphere
  )

