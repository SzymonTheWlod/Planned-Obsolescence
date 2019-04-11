FIND_PACKAGE(Git QUIET)
if(GIT_FOUND AND EXISTS "${CMAKE_SOURCE_DIR}/.git")

    EXECUTE_PROCESS(
            COMMAND
            ${GIT_EXECUTABLE} rev-parse --show-toplevel
            WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}"
            OUTPUT_VARIABLE GIT_ROOT_LOCATION
            OUTPUT_STRIP_TRAILING_WHITESPACE
            RESULT_VARIABLE GIT_RESULT
            ERROR_QUIET)

    FILE(   COPY ${CMAKE_CURRENT_SOURCE_DIR}/pre-commit
            DESTINATION ${GIT_ROOT_LOCATION}/.git/hooks
            FILE_PERMISSIONS
            OWNER_READ OWNER_WRITE OWNER_EXECUTE
            GROUP_READ GROUP_WRITE GROUP_EXECUTE
            WORLD_READ WORLD_EXECUTE)

    FIND_PROGRAM(
            CFE NAMES clang-format
            HINTS ${CMAKE_CURRENT_SOURCE_DIR}/../Tools/*/${PLATFORM}/)

    MESSAGE("ClangFormat   " ${CFE})

    if(CFE)
        EXECUTE_PROCESS(COMMAND git config hooks.clangFormatExe ${CFE})
    else()
        EXECUTE_PROCESS(COMMAND git config --unset hooks.clangFormatExe)
    endif()
endif()
