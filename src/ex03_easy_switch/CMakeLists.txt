set(BASE_DIRECTORY "${CMAKE_SOURCE_DIR}")
configure_file(baseDirectory.h.in baseDirectory.h)

if(DMO_USE_CUDA)
    set(EXEC_NAME ex03_easy_switch_GPU)


    set(EXEC_FILES
        main.cpp
        registerDMO.cu
        switch_DMO_CPU_GPU.h
        UserMetrics.h
        )

    #add project executable
    add_executable(${EXEC_NAME}
    	${EXEC_FILES}
    )

    target_include_directories(${EXEC_NAME} PRIVATE 
    	${SRC}
        ${CMAKE_CURRENT_BINARY_DIR}
        ### externals ###
        ${OPENMESH_INCLUDE_DIRS}
    )

    target_link_libraries(${EXEC_NAME} PRIVATE 
    	DMO::DMO
        ${LIBS} 
    )

    target_compile_definitions(${EXEC_NAME} PRIVATE -D_SILENCE_EXPERIMENTAL_FILESYSTEM_DEPRECATION_WARNING)

    if(MSVC)
        set_target_properties(${EXEC_NAME} PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY "${PROJECT_SOURCE_DIR}")
    endif()
endif()

set(EXEC_NAME ex03_easy_switch_CPU)

set(EXEC_FILES
    main.cpp
    UserMetrics.h
    switch_DMO_CPU_GPU.h
    )

#add project executable
add_executable(${EXEC_NAME}
    ${EXEC_FILES}
)

target_include_directories(${EXEC_NAME} PRIVATE 
    ${SRC}
    ${CMAKE_CURRENT_BINARY_DIR}
    ### externals ###
    ${OPENMESH_INCLUDE_DIRS}
)

target_link_libraries(${EXEC_NAME} PRIVATE 
    DMO::DMO
    ${LIBS} 
)

#provide additional way of restricting GPU usage even if available
target_compile_definitions(${EXEC_NAME} PRIVATE -D_FORCE_CPU_USAGE)

target_compile_definitions(${EXEC_NAME} PRIVATE -D_SILENCE_EXPERIMENTAL_FILESYSTEM_DEPRECATION_WARNING)

if(MSVC)
    set_target_properties(${EXEC_NAME} PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY "${PROJECT_SOURCE_DIR}")
endif()