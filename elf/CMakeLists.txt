project(libelf CXX)

find_package(PythonInterp 3 REQUIRED)


add_custom_command(
  OUTPUT to_string.cc
  COMMAND echo "#include \"data.hh\"" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
  COMMAND echo "#include \"to_hex.hh\"" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
  COMMAND echo "ELFPP_BEGIN_NAMESPACE" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
  COMMAND python3 ${CMAKE_CURRENT_SOURCE_DIR}/enum-print.py -u --hex --no-type --mask shf --mask pf -x loos -x hios -x loproc -x hiproc < ${CMAKE_CURRENT_SOURCE_DIR}/data.hh >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc
  COMMAND echo "ELFPP_END_NAMESPACE" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
  DEPENDS enum-print.py data.hh
  COMMENT "Generating to_string.cc.aux"
)




add_library(libelf OBJECT
elf++.hh
data.hh
common.hh
to_hex.hh
elf.cc
mmap_loader.cc
to_string.cc
)

target_include_directories(libelf
    PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

# add source files to t


target_compile_options(libelf
    PRIVATE
    -fPIC
    -Wall
    -Wextra
    -Wpedantic
    -Werror
)



# shared libraries need PIC

# shared and static libraries built from the same object files
add_library(libelf_shared SHARED $<TARGET_OBJECTS:libelf>)
add_library(libelf_static STATIC $<TARGET_OBJECTS:libelf>)


set_target_properties(libelf_shared PROPERTIES
    OUTPUT_NAME elf
    SOVERSION 1
    VERSION 1.0.0
)

set_target_properties(libelf_static PROPERTIES
    OUTPUT_NAME elf
    SOVERSION 1
    VERSION 1.0.0
)

