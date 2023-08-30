add_executable(dump_lines dump-lines.cc)
target_link_libraries(dump_lines libelf libdwarf)

add_executable(dump_syms dump-syms.cc)
target_link_libraries(dump_syms libelf libdwarf)

add_executable(dump_sections dump-sections.cc)
target_link_libraries(dump_sections libelf libdwarf)

add_executable(dump_segments dump-segments.cc)
target_link_libraries(dump_segments libelf libdwarf)

add_executable(dump_tree dump-tree.cc)
target_link_libraries(dump_tree libelf libdwarf)

add_executable(find_pc find-pc.cc)
target_link_libraries(find_pc libelf libdwarf)