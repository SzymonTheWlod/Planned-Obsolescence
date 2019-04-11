## copy the configs across
file(GLOB CONFIG_FILES
        LIST_DIRECTORIES false
        CONFIGURE_DEPENDS
        RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
        ".*?"
        )

file(COPY ${CONFIG_FILES} DESTINATION ${CMAKE_SOURCE_DIR})

find_package(Git QUIET)
if(GIT_FOUND AND EXISTS "${CMAKE_SOURCE_DIR}/.git")

    foreach(ARG  ${CONFIG_FILES})
        execute_process(
                COMMAND ${GIT_EXECUTABLE} add "${CMAKE_SOURCE_DIR}/${ARG}"
                WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
                OUTPUT_VARIABLE GIT_UPDATE_CONFIG_FILES
                OUTPUT_STRIP_TRAILING_WHITESPACE
                RESULT_VARIABLE GIT_RESULT
                OUTPUT_QUIET
                ERROR_QUIET)

        execute_process(
                COMMAND ${GIT_EXECUTABLE} commit "${CMAKE_SOURCE_DIR}/${ARG}" -m "Updated ${ARG}"
                WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
                OUTPUT_VARIABLE GIT_UPDATE_CONFIG_FILES
                OUTPUT_STRIP_TRAILING_WHITESPACE
                RESULT_VARIABLE GIT_RESULT
                OUTPUT_QUIET
                ERROR_QUIET)
    endforeach()

    list(APPEND  OLD_FILES .codacy.yml .codeclimate)
    foreach(ARG  ${OLD_FILES})
        if(EXISTS "${CMAKE_SOURCE_DIR}/${ARG}")
            execute_process(
                    COMMAND ${GIT_EXECUTABLE} rm -f "${CMAKE_SOURCE_DIR}/${ARG}"
                    WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
                    OUTPUT_VARIABLE GIT_UPDATE_CONFIG_FILES
                    OUTPUT_STRIP_TRAILING_WHITESPACE
                    RESULT_VARIABLE GIT_RESULT
                    ERROR_QUIET)
        endif()
    endforeach()
endif()
