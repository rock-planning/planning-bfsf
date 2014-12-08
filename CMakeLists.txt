cmake_minimum_required(VERSION 2.6)

find_package(Rock)
rock_init(bfsf 0.1)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

include_directories(include)
include_directories(interfaces/ff-wrapped)
include_directories(interfaces/agnostic)
include_directories(external)

add_subdirectory(external/libff)

include_directories(planners/at_bfs_f-ff/src)
find_package(Boost COMPONENTS program_options)
rock_executable(at_bfs_f
    SOURCES
        include/aptk/bit_set.cxx
        interfaces/ff-wrapped/ff_to_aptk.cxx
        interfaces/agnostic/action.cxx
        interfaces/agnostic/cond_eff.cxx
        interfaces/agnostic/conj_comp_prob.cxx
        interfaces/agnostic/fl_conj.cxx
        interfaces/agnostic/fluent.cxx
        interfaces/agnostic/fwd_search_prob.cxx
        interfaces/agnostic/landmark_graph.cxx
        interfaces/agnostic/match_tree.cxx
        interfaces/agnostic/reachability.cxx
        interfaces/agnostic/strips_prob.cxx
        interfaces/agnostic/strips_state.cxx
        interfaces/agnostic/succ_gen.cxx
        planners/at_bfs_f-ff/src/main.cxx
        src/bit_array.cxx
        src/bit_set.cxx
        src/memory.cxx
    DEPS ff
    NOINSTALL
)
target_link_libraries(at_bfs_f ${Boost_LIBRARIES})

install(FILES at_bfs_f
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE
    WORLD_READ WORLD_EXECUTE
    DESTINATION bin
    RENAME bfsf-planner
)
