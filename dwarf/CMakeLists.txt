project(libdwarf CXX)

find_package(PythonInterp 3 REQUIRED)

add_custom_command(
  OUTPUT to_string.cc
  COMMAND echo "#include \"internal.hh\"" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
  COMMAND echo "DWARFPP_BEGIN_NAMESPACE" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
#   COMMAND python3 ${CMAKE_CURRENT_SOURCE_DIR}/../elfenum-print.py -u --hex --no-type --mask shf --mask pf -x loos -x hios -x loproc -x hiproc < ${CMAKE_CURRENT_SOURCE_DIR}/data.hh >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc
  COMMAND python3 ${CMAKE_CURRENT_SOURCE_DIR}/../elf/enum-print.py < ${CMAKE_CURRENT_SOURCE_DIR}/dwarf++.hh >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc
  COMMAND python3 ${CMAKE_CURRENT_SOURCE_DIR}/../elf/enum-print.py -s _ -u --hex -x hi_user -x lo_user < ${CMAKE_CURRENT_SOURCE_DIR}/data.hh >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc
  COMMAND echo "DWARFPP_END_NAMESPACE" >> ${CMAKE_CURRENT_BINARY_DIR}/to_string.cc VERBATIM
  DEPENDS ../elf/enum-print.py internal.hh
  COMMENT "Generating to_string.cc"
)

add_library(libdwarf OBJECT
abbrev.cc
attrs.cc
cursor.cc
die.cc
die_str_map.cc
dwarf.cc
elf.cc
expr.cc
line.cc
rangelist.cc
value.cc
to_string.cc
)

target_include_directories(libdwarf
    PRIVATE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    )


# add source files to t


target_compile_options(libdwarf
    PRIVATE
    -fPIC
    -Wall
    -Wextra
    -Wno-unused-parameter
    -Wpedantic
    -Wno-vla-extension
    -Wno-gnu-case-range
    -Werror
)


# shared and static libraries built from the same object files
add_library(libdwarf_shared SHARED $<TARGET_OBJECTS:libdwarf>)
add_library(libdwarf_static STATIC $<TARGET_OBJECTS:libdwarf>)


set_target_properties(libdwarf_shared PROPERTIES
    OUTPUT_NAME dwarf
    SOVERSION 1
    VERSION 1.0.0
)

set_target_properties(libdwarf_static PROPERTIES
    OUTPUT_NAME dwarf
    SOVERSION 1
    VERSION 1.0.0
)
