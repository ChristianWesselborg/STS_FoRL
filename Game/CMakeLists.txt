cmake_minimum_required (VERSION 3.8)

project(sts_rl)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(PYBIND11_PYTHON_VERSION 3.7.3 CACHE STRING "")
find_package(pybind11 CONFIG REQUIRED)  

add_executable (${PROJECT_NAME} "Main.cpp" "enemies.cpp" "enemies.h" "events.cpp" "events.h"
"gamestate.h" "gamestate1.cpp" "gamestate2.cpp" "gamestate3.cpp" "ic_cards.cpp" "ic_cards.h"
"pc_info.cpp" "pc_info.h" "top.cpp" "top.h" "card.cpp" "card.h" "character.cpp" 
"character.h" "effects_triggers.cpp" "effects_triggers.h" "potions.h" "visuals.h"
"savestate.h" "potions.cpp" "relic.cpp"  "visuals.cpp" "savestate.cpp")

set(
    RL_FILES 
    "CardEmbedding.png"
    "DQN.py"
    "NN.py"
    "PRED.py"
)

set(COPIED "")
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/scripts/)
foreach(FILE IN LISTS RL_FILES)
    add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND cp ${CMAKE_CURRENT_LIST_DIR}/../RL/${FILE} ${CMAKE_CURRENT_BINARY_DIR}/scripts/${FILE}
        COMMENT "COPYING ${FILE}"
        VERBATIM
    )
endforeach()

target_link_libraries(${PROJECT_NAME} PRIVATE pybind11::embed pybind11::module pybind11::pybind11)

# set binary filenames
set_target_properties( ${PROJECT_NAME} PROPERTIES OUTPUT_NAME_DEBUG ${PROJECT_NAME}-debug )
set_target_properties( ${PROJECT_NAME} PROPERTIES OUTPUT_NAME_RELWITHDEBINFO ${PROJECT_NAME}-relwidthdebinfo )
set_target_properties( ${PROJECT_NAME} PROPERTIES OUTPUT_NAME_RELEASE ${PROJECT_NAME}-release )
set_target_properties( ${PROJECT_NAME} PROPERTIES OUTPUT_NAME_MINSIZEREL ${PROJECT_NAME}-minsizerel )

# set warning levels
# if (MSVC)
#     target_compile_options(${PROJECT_NAME} PUBLIC /W4 /permissive)
# else()
#     target_compile_options(${PROJECT_NAME} PUBLIC -Wall -Wextra -pedantic)
# endif()