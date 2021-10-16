cmake_minimum_required(VERSION 3.0.0)
project(LEA VERSION 1.1.0)

include(CTest)
enable_testing()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
set(LEA_ARCH "Unknown")
if(${CMAKE_GENERATOR_PLATFORM})
    set(LEA_ARCH ${CMAKE_GENERATOR_PLATFORM})
else()
    if(${CMAKE_C_COMPILER} MATCHES "x86_64")
        set(LEA_ARCH "x64")
    elseif(${CMAKE_C_COMPILER} MATCHES "x86")
        set(LEA_ARCH "x86")
    elseif(${CMAKE_C_COMPILER} MATCHES "arm")
        set(LEA_ARCH "arm")
    elseif(${CMAKE_SYSTEM_PROCESSOR} MATCHES "AMD64")
        set(LEA_ARCH "x64")
    else()
        set(LEA_ARCH ${CMAKE_SYSTEM_PROCESSOR})
    endif()
endif()

set(SOURCES )
if(${LEA_ARCH} MATCHES "x64" OR ${LEA_ARCH} MATCHES "x86")
    if(NOT MSVC)
        add_compile_options(-mxop)
        add_compile_options(-mpclmul)
        add_compile_options(-msse2)
        add_compile_options(-mavx2)
    else()
        add_compile_options(/arch:AVX2)
    endif()

set(SOURCES 
    src/cpu_info_ia32.c
    src/lea_base.c
    src/lea_core.c
    src/lea_core_xop.c
    src/lea_gcm_generic.c
    src/lea_gcm_pclmul.c
    src/lea_t_fallback.c
    src/lea_t_generic.c
    src/lea_t_sse2.c
    src/lea_t_xop.c
    src/lea_t_avx2_sse2.c
    src/lea_t_avx2_xop.c
    )

endif()

add_library(LEA ${SOURCES})