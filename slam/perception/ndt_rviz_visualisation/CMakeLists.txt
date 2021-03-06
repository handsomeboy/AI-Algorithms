cmake_minimum_required(VERSION 2.8.3)
project(ndt_rviz_visualisation)

find_package(catkin REQUIRED COMPONENTS rviz ndt_map)
# find_package(catkin REQUIRED COMPONENTS rviz ndt_map)
catkin_package()
include_directories(${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

# find_package(Qt5 COMPONENTS QtCore QtGui REQUIRED)
find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)
# MESSAGE( STATUS "THE BAD INCLUDE:         " ${Qt5Core_INCLUDE_DIRS} )
# include(${QT_USE_FILE})
include_directories(${Qt5Core_INCLUDE_DIRS})
include_directories(${Qt5Gui_INCLUDE_DIRS})

find_package(Qt5Widgets REQUIRED)
if(${Qt5Widgets_VERSION} VERSION_LESS 5.7.0)
    message("QT less than 5.7")
else()
    message("Qt more than version 5.7, enabling c++11 support")
    set(CMAKE_CXX_STANDARD 11)
endif()

include_directories(include)

add_definitions(-DQT_NO_KEYWORDS)


qt5_wrap_cpp(MOC_FILES
  include/ndt_rviz_visualisation/ndt_display.hpp
  include/ndt_rviz_visualisation/ndt_line_display.hpp
)

## Here we specify the list of source files, including the output of
## the previous command which is stored in ``${MOC_FILES}``.
set(SOURCE_FILES
  src/ndt_display.cpp
  src/ndt_visual.cpp
  src/ndt_line_display.cpp
  src/ndt_line_visual.cpp
#  src/ndt_vector_display.cpp
  ${MOC_FILES}
)

## An rviz plugin is just a shared library, so here we declare the
## library to be called ``${PROJECT_NAME}`` (which is
## "rviz_plugin_tutorials", or whatever your version of this project
## is called) and specify the list of source files we collected above
## in ``${SOURCE_FILES}``.
add_library(${PROJECT_NAME} ${SOURCE_FILES})

## Link the library with whatever Qt libraries have been defined by
## the ``find_package(Qt4 ...)`` line above, and with whatever libraries
## catkin has included.
##
## Although this puts "rviz_plugin_tutorials" (or whatever you have
## called the project) as the name of the library, cmake knows it is a
## library and names the actual file something like
## "librviz_plugin_tutorials.so", or whatever is appropriate for your
## particular OS.
target_link_libraries(${PROJECT_NAME} ${QT_LIBRARIES} ${catkin_LIBRARIES})
## END_TUTORIAL


catkin_package(
    DEPENDS  
    CATKIN_DEPENDS rviz ndt_map
    INCLUDE_DIRS include 
    LIBRARIES ${PROJECT_NAME}
)

## Install rules

install(TARGETS
  ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(FILES 
  plugin_description.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

#install(DIRECTORY media/
#  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/media)

#install(DIRECTORY icons/
#  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/icons)

#install(PROGRAMS scripts/send_test_msgs.py
#  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

