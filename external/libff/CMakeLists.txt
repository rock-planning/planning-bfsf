find_package(BISON)
find_package(FLEX)

add_definitions("-O6 -ansi")

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
# FLEX_TARGET(Name FlexInput FlexOutput [COMPILE_FLAGS <string>])
FLEX_TARGET(F_OPS lex-ops_pddl.l ${CMAKE_CURRENT_BINARY_DIR}/lex.ops_pddl.c
    COMPILE_FLAGS "-Pops_pddl")
FLEX_TARGET(F_FCT lex-fct_pddl.l ${CMAKE_CURRENT_BINARY_DIR}/lex.fct_pddl.c
    COMPILE_FLAGS "-Pfct_pddl")

## BISON_TARGET(Name FlexInput FlexOutput [COMPILE_FLAGS <string>])
BISON_TARGET(B_OPS scan-ops_pddl.y
    ${CMAKE_CURRENT_BINARY_DIR}/scan-ops_pddl.tab.c COMPILE_FLAGS "-pops_pddl -bscan-ops_pddl")
BISON_TARGET(B_FCT scan-fct_pddl.y
    ${CMAKE_CURRENT_BINARY_DIR}/scan-fct_pddl.tab.c COMPILE_FLAGS "-pfct_pddl -bscan-fct_pddl")

#ADD_FLEX_BISON_DEPENDENCY(F_OPS B_OPS)
#ADD_FLEX_BISON_DEPENDENCY(F_FCT B_FCT)

set_source_files_properties(${BISON_B_OPS_OUTPUTS} PROPERTIES OBJECT_DEPENDS
    ${FLEX_F_OPS_OUTPUTS})
set_source_files_properties(${BISON_B_FCT_OUTPUTS} PROPERTIES OBJECT_DEPENDS
    ${FLEX_F_FCT_OUTPUTS})


add_library(ff
        STATIC
        expressions.c
        inst_easy.c
        inst_final.c
        inst_hard.c
        inst_pre.c
        libff.c
        memory.c
        output.c
        parse.c
        relax.c
        search.c
        utility.c
        ${BISON_B_OPS_OUTPUTS}
        ${BISON_B_FCT_OUTPUTS}
)

#rock_executable(ff_bin
#    SOURCES    main.c
#    DEPS ff)

