    cmake_minimum_required(VERSION 3.22)
    set(name ijkplay)
    project(${name})

    set(root_project ${CMAKE_CURRENT_LIST_DIR})


    set(FFMPEG_LIB_PATH ${root_project}/libs/${CMAKE_ANDROID_ARCH_ABI})

    include_directories(${root_project}/ijkj4a)
    include_directories(${root_project}/ijksdl)
    include_directories(${root_project}/ijksoundtouch)
    include_directories(${root_project}/ijkplayer)
    include_directories(${root_project}/ijkyuv/include)
    include_directories(${root_project}/ijksoundtouch/include)
    include_directories(${root_project}/libs/include)

    include_directories(${root_project})

    file(GLOB_RECURSE SOURCE_IJKJ4A ${root_project}/ijkj4a/*.c  ${root_project}/ijkj4a/*.cpp)
    file(GLOB_RECURSE SOURCE_SDL ${root_project}/ijksdl/*.c  ${root_project}/ijksdl/*.cpp)
    file(GLOB_RECURSE SOURCE_IJKPLAYER ${root_project}/ijkplayer/*.c ${root_project}/ijkplayer/*.cpp)
    file(GLOB_RECURSE SOURCE_SOUNDTOUCH ${root_project}/ijksoundtouch/*.c ${root_project}/ijksoundtouch/*.cpp)

    set(SOURCE_YUV
            ${root_project}/ijkyuv/source/compare.cc
            ${root_project}/ijkyuv/source/compare_common.cc
            ${root_project}/ijkyuv/source/compare_posix.cc
            ${root_project}/ijkyuv/source/convert.cc
            ${root_project}/ijkyuv/source/convert_argb.cc
            ${root_project}/ijkyuv/source/convert_from.cc
            ${root_project}/ijkyuv/source/convert_from_argb.cc
            ${root_project}/ijkyuv/source/convert_to_argb.cc
            ${root_project}/ijkyuv/source/convert_to_i420.cc
            ${root_project}/ijkyuv/source/cpu_id.cc
            ${root_project}/ijkyuv/source/format_conversion.cc
            ${root_project}/ijkyuv/source/planar_functions.cc
            ${root_project}/ijkyuv/source/rotate.cc
            ${root_project}/ijkyuv/source/rotate_argb.cc
            ${root_project}/ijkyuv/source/rotate_mips.cc
            ${root_project}/ijkyuv/source/row_any.cc
            ${root_project}/ijkyuv/source/row_common.cc
            ${root_project}/ijkyuv/source/row_mips.cc
            ${root_project}/ijkyuv/source/row_posix.cc
            ${root_project}/ijkyuv/source/scale.cc
            ${root_project}/ijkyuv/source/scale_argb.cc
            ${root_project}/ijkyuv/source/scale_common.cc
            ${root_project}/ijkyuv/source/scale_mips.cc
            ${root_project}/ijkyuv/source/scale_posix.cc
            ${root_project}/ijkyuv/source/video_common.cc
    )

    if(${CMAKE_ANDROID_ARCH_ABI} STREQUAL "armeabi-v7a")
        set(SOURCE_YUV_ABI
                ${root_project}/ijkyuv/source/compare_neon.cc
                ${root_project}/ijkyuv/source/rotate_neon.cc
                ${root_project}/ijkyuv/source/row_neon.cc
                ${root_project}/ijkyuv/source/scale_neon.cc
        )
    elseif(${CMAKE_ANDROID_ARCH_ABI} STREQUAL "arm64-v8a")
        set(SOURCE_YUV_ABI
                ${root_project}/ijkyuv/source/compare_neon64.cc
                ${root_project}/ijkyuv/source/rotate_neon64.cc
                ${root_project}/ijkyuv/source/row_neon64.cc
                ${root_project}/ijkyuv/source/scale_neon64.cc
        )
    endif()

    list(APPEND  ${SOURCE_YUV}  ${SOURCE_YUV_ABI})
    set(SOURCE_FILE
            ${SOURCE_IJKJ4A}
            ${SOURCE_SDL}
            ${SOURCE_IJKPLAYER}
            ${SOURCE_SOUNDTOUCH}
            ${SOURCE_FILE})


    add_library(${name}
            SHARED
            prof.c
            ${SOURCE_IJKJ4A}
            ${SOURCE_SDL}
            ${SOURCE_IJKPLAYER}
            ${SOURCE_SOUNDTOUCH}
            ${SOURCE_YUV}
    )


    target_link_libraries(${name}
            log
            android
            EGL
            GLESv2
            jnigraphics
            OpenSLES
            m z
            ${FFMPEG_LIB_PATH}/libijkffmpeg.so

    )

