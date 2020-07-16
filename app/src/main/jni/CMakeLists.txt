# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.9 FATAL_ERROR)
#cmake_policy(SET CMP0048 NEW)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

set(JNI_LIB_DIR      ${CMAKE_SOURCE_DIR}/../jniLibs)
set(LIBYUV_INCLUDES  ${CMAKE_SOURCE_DIR}/libyuv)
add_library( # Sets the name of the library.
             rgb2yuv

             # Sets the library as a shared library.
             SHARED

             # Headers
             libyuv.h

             # Provides a relative path to your source file(s).
             libyuv.cpp
             rgb2yuv.cpp
             )

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log
              android
              )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

# Include libyuv
#add_library(libyuv-lib SHARED IMPORTED)
#set_target_properties(libyuv-lib
#
#        PROPERTIES IMPORTED_LOCATION
#        ${JNI_LIB_DIR}/${ANDROID_ABI}/libyuv.so
#        )
#
#add_custom_command(TARGET rgb2yuv POST_BUILD
#        COMMAND ${CMAKE_COMMAND} -E copy
#        ${JNI_LIB_DIR}/${ANDROID_ABI}/libyuv.so
#        ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}
#        )

target_include_directories(rgb2yuv
        PUBLIC ${LIBYUV_INCLUDES}
        )


target_link_libraries( # Specifies the target library.
                       rgb2yuv

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib}
                       android
#                       libyuv-libs
                       ${JNI_LIB_DIR}/${ANDROID_ABI}/libyuv.so
                       )
