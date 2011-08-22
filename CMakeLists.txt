CMAKE_MINIMUM_REQUIRED(VERSION 2.0)

PROJECT(LabelGeometryImageFilter)

# This is needed for testing with ctest
ENABLE_TESTING()

# Don't build anything unless the version of CMake is high enough.
# The matching ELSE/ENDIF should be the last lines in the file.
IF("${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}" GREATER 1.8)
	

FIND_PACKAGE(ITK)
IF(ITK_FOUND)
  INCLUDE(${ITK_USE_FILE})
ELSE(ITK_FOUND)
  MESSAGE(FATAL_ERROR
          "Cannot build project without ITK.  Please set ITK_DIR.")
ENDIF(ITK_FOUND)

# Configure the default ITK_DATA_ROOT for the location of ITK Data.
#FIND_PATH(ITK_DATA_ROOT ITKData.readme ${ITK_SOURCE_DIR}/Testing/Data $ENV{ITK_DATA_ROOT})
#MARK_AS_ADVANCED(ITK_DATA_ROOT)


#Set any libraries that your project depends on.
#examples: ITKCommon, VTKRendering, etc
SET(Libraries
ITKCommon
ITKIO
)


#SET(INPUTDATA ${ITK_DATA_ROOT}/Input)
SET(INPUTDATA ${PROJECT_SOURCE_DIR})

SET(CurrentExe "itkLabelGeometryImageFilterTest")
ADD_EXECUTABLE(${CurrentExe} itkLabelGeometryImageFilterTest.cxx)
TARGET_LINK_LIBRARIES(${CurrentExe} ${Libraries})

#the following line is an example of how to add a test to your project.
#Testname is the title for this particular test.  ExecutableToRun is the
#program which will be running this test.  It can either be a part of this
#project or an external executable.  After that list any args that are needed
#for this test.  Include as many tests as you like.  If your project doesn't have
#any tests you can comment out or delete the following line.
ADD_TEST(LabelGeometryTest ${CurrentExe} ${INPUTDATA}/itkLabelGeometryImageFilterTestBinaryImage.png itkLabelGeometryImageFilterTestImage.png 2 ${INPUTDATA}/itkLabelGeometryImageFilterTestIntensityImage.png)
ADD_TEST(LabelGeometryTest ${CurrentExe} ${INPUTDATA}/itkLabelGeometryImageFilterTestBinaryVolume.mhd itkLabelGeometryImageFilterTestVolume.mhd 3 ${INPUTDATA}/itkLabelGeometryImageFilterTestIntensityVolume.mhd)


# If the version of CMake was too old, complain and build nothing.
# These should be the last lines in this file.
ELSE("${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}" GREATER 1.8)
  MESSAGE(SEND_ERROR
          "This version of CMake is too old to build this project.  "
          "Please upgrade to CMake 2.0.")
ENDIF("${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}" GREATER 1.8)
