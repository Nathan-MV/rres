# Setup the project and settings
project(rres C)
set(PROJECT_VERSION 4.5.0)
set(API_VERSION 450)

include(GNUInstallDirs)

# Define source and header files
set(RRES_SOURCE src/rres.c)
set(RRES_HEADER src/rres.h)

# Read the contents of the source file
file(READ ${RRES_HEADER} FILE_CONTENTS)

# Write the contents to the destination file
file(WRITE ${RRES_SOURCE} "${FILE_CONTENTS}")

add_library(rres STATIC ${RRES_SOURCE} ${RRES_HEADER})

set_target_properties(rres PROPERTIES
                      PUBLIC_HEADER "${RRES_HEADER}"
                      VERSION ${PROJECT_VERSION}
                      SOVERSION ${API_VERSION}
                      )

if (WITH_PIC)
    set_property(TARGET rres PROPERTY POSITION_INDEPENDENT_CODE ON)
endif ()

target_link_libraries(rres PRIVATE raylib)

# Registering include directories
target_include_directories(rres
                           PUBLIC
                           $<INSTALL_INTERFACE:include>
                           $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
                           PRIVATE
                           ${CMAKE_CURRENT_SOURCE_DIR}
                           )

# Copy the header files to the build directory for convenience
file(COPY ${RRES_HEADER} DESTINATION "include")
