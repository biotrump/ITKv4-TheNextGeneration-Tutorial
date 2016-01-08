cmake_minimum_required(VERSION 2.8)

# --------------------------------------------------
#openMP to avoid link error : omp_xxx error!
# undefined reference to `GOMP_loop_dynamic_start
if( NOT OPENMP_FOUND)
    find_package(OpenMP)
    if(OPENMP_FOUND)
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    endif()
endif( NOT OPENMP_FOUND)
#-----------------------------------------------------
#export OPENCV_OUT=/ssd/opencv-3.1.0/build/debug-x86-64
#ITK OpenCV bridges does not seem to work in openCV 3.1
#export OPENCV_OUT=/ssd/opencv/build_2.4.12.2/debug
#itk 4.8.x is not compatible to opencv 3.x cv::Mat
#export OPENCV_OUT=/ssd/opencv/build_3.0.0/x86_64_debug
#export OPENCV_OUT=/home/thomas/build/bcv-x86.gl/out/opencv/2.4.x-static/x86_64
set(OpenCV_DIR $ENV{OPENCV_OUT})
message("++++OpenCV_DIR:${OpenCV_DIR}")
FIND_PACKAGE( OpenCV REQUIRED)
if(OpenCV_FOUND)
	message("found")
	message( "OpenCV_INCLUDE_DIRS:${OpenCV_INCLUDE_DIRS}" )
	message( "OpenCV_INCLUDES:${OpenCV_INCLUDES}")
	message( "OpenCV_LIBRARIES:${OpenCV_LIBRARIES}")
	message( "OpenCV_LIBS:${OpenCV_LIBS}")
	message( "OpenCV_DEFINITIONS:${OpenCV_DEFINITIONS}")
else()
	message(FATAL_ERROR "openCV not foud")
endif()
if(OpenCV_FOUND)
  include_directories(${OpenCV_INCLUDE_DIRS})
endif()
message("*** OpenCV_LIBS:${OpenCV_LIBS}")
message("*** OpenCV_LIBRARIES:${OpenCV_LIBRARIES}")
#-----------------------------------------------------
#export VTK_DIR=/home/thomas/MIP/VTK/build/debug.6.3
#export ITK_OUT=/home/thomas/MIP/ITK/build/debug.4.8.1
set(ITK_DIR $ENV{ITK_OUT})
message("++++ITK_DIR:${ITK_DIR}")
FIND_PACKAGE( ITK REQUIRED)
if(ITK_FOUND)
	message("found")
	message( "ITK_INCLUDE_DIRS:${ITK_INCLUDE_DIRS}" )
	message( "ITK_INCLUDES:${ITK_INCLUDES}")
	message( "ITK_LIBRARIES:${ITK_LIBRARIES}")
	message( "ITK_LIBS:${ITK_LIBS}")
	message( "ITK_DEFINITIONS:${ITK_DEFINITIONS}")
else()
	message(FATAL_ERROR "ITK not foud")
endif()
if(ITK_FOUND)
#  include(${USE_ITK_FILE})
  include_directories(${ITK_INCLUDE_DIRS})
endif()
message("*** ITK_LIBS:${ITK_LIBS}")
message("*** ITK_LIBRARIES:${ITK_LIBRARIES}")
#---------------------------------------------------------
# Widgets finds its own dependencies (QtGui and QtCore).
find_package(Qt5Widgets REQUIRED)
#  find_package(Qt5Declarative)

# The Qt5Widgets_INCLUDES also includes the include directories for
# dependencies QtCore and QtGui
include_directories(${Qt5Widgets_INCLUDES})

# We need add -DQT_WIDGETS_LIB when using QtWidgets in Qt 5.
add_definitions(${Qt5Widgets_DEFINITIONS})

# Executables fail to build with Qt 5 in the default configuration
# without -fPIE. We add that here.
#  set(CMAKE_CXX_FLAGS "${Qt5Widgets_EXECUTABLE_COMPILE_FLAGS}")
#-----------------------------------------------------
# BasicFilteringITKOpenCVBridge
add_executable(BasicFilteringITKOpenCVBridge 
  BasicFilteringITKOpenCVBridge.cxx )
target_link_libraries(BasicFilteringITKOpenCVBridge
  ${ITK_LIBRARIES} ${OpenCV_LIBS})
qt5_use_modules(BasicFilteringITKOpenCVBridge 
				Widgets  
				OpenGL 
				Test 
				Concurrent)
				
# BasicFilteringITKOpenCVBridgeAnswer
add_executable(BasicFilteringITKOpenCVBridgeAnswer 
  BasicFilteringITKOpenCVBridgeAnswer.cxx )
target_link_libraries(BasicFilteringITKOpenCVBridgeAnswer
  ${ITK_LIBRARIES} ${OpenCV_LIBS})
qt5_use_modules(BasicFilteringITKOpenCVBridgeAnswer 
				Widgets  
				OpenGL 
				Test 
				Concurrent)
