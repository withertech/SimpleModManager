#
# CMakeLists for the Homebrew Application
#

# Replace this with the name of your application
set(HOMEBREW_NAME "SimpleModManagerConsole")
set(HOMEBREW_APP "${HOMEBREW_NAME}")

# Meta information about the app
set(HOMEBREW_TITLE ${HOMEBREW_NAME})
set(HOMEBREW_AUTHOR "Nadrino")
set(HOMEBREW_ICON "${CMAKE_CURRENT_SOURCE_DIR}/assets/icon_gui.jpg")

file(GLOB SOURCE_FILES "${SMM_CORE_SOURCE_DIR}/*.cpp")

if (cmake_VERBOSE)
    list(LENGTH SOURCE_FILES SOURCE_FILES_LENGTH)
    cmake_info("Loaded ${SOURCE_FILES_LENGTH} source file(s)")
endif ()

build_switch_binaries(${HOMEBREW_APP}.elf ${HOMEBREW_TITLE} ${HOMEBREW_AUTHOR} ${HOMEBREW_ICON} ${APP_VERSION}) # need to be defined before add_executable... CMake works in myterious ways

add_executable(${HOMEBREW_APP}.elf
        ${SOURCE_FILES}
        )

target_link_libraries(
        ${HOMEBREW_APP}.elf
        switch::libnx
        -L/opt/devkitpro/portlibs/switch/lib
        ${ZLIB_LIBRARIES}
        #        -lSDL2 -march=armv8-a -fPIE -L/opt/devkitpro/libnx/lib -lEGL -lglapi -ldrm_nouveau -lnx
)

set_target_properties(${HOMEBREW_APP}.elf PROPERTIES
        LINKER_LANGUAGE CXX # Replace this with C if you have C source files
        LINK_FLAGS "-specs=${LIBNX}/switch.specs -Wl,-no-as-needed -Wl,-Map,.map"
        )