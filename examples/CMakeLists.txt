# AXERA is pleased to support the open source community by making ax-samples available.
#
# Copyright (c) 2022, AXERA Semiconductor (Shanghai) Co., Ltd. All rights reserved.
#
# Licensed under the BSD 3-Clause License (the "License"); you may not use this file except
# in compliance with the License. You may obtain a copy of the License at
#
# https://opensource.org/licenses/BSD-3-Clause
#
# Unless required by applicable law or agreed to in writing, software distributed
# under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
# CONDITIONS OF ANY KIND, either express or implied. See the License for the
# specific language governing permissions and limitations under the License.
#
# Author: ls.wang
#

# example generation function
function (axera_example name)
    add_executable(${name})

    # add srcs
    foreach (file IN LISTS ARGN)
        target_sources (${name} PRIVATE ${file})
    endforeach()

    # headers
    target_include_directories (${name} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
    target_include_directories (${name} PRIVATE ${BSP_MSP_DIR}/include)

    # libs
    target_link_libraries (${name} PRIVATE pthread dl stdc++fs)
    target_link_libraries (${name} PRIVATE ax_interpreter_external ax_interpreter ax_sys axsyslog)
    target_link_libraries (${name} PRIVATE ax_ivps ax_npu_cv_kit)
    target_link_libraries (${name} PRIVATE ax_run_joint)

    # folders
    target_link_directories(${name} PRIVATE ${BSP_MSP_DIR}/lib)

    # opencv
    target_include_directories (${name} PRIVATE ${OpenCV_INCLUDE_DIRS})
    target_link_libraries(${name} PRIVATE ${OpenCV_LIBS})

    install (TARGETS ${name} DESTINATION bin)
endfunction()

set (OpenCV_DIR ${CMAKE_SOURCE_DIR}/3rdparty/opencv/lib/cmake/opencv4)
find_package(OpenCV REQUIRED)

# open when need debug on board
# add_definitions("-g -O0")

axera_example (ax_classification ax_classification_steps.cc)
axera_example (ax_classification_nv12 ax_classification_nv12_resize_steps)
axera_example (ax_yolov5s        ax_yolov5s_steps.cc)
axera_example (ax_yoloxs         ax_yoloxs_steps.cc)
axera_example (ax_yolo_fastest   ax_yolo_fastest_steps.cc)
axera_example (ax_yolo_fastest_body ax_yolo_fastest_body_steps.cc)
axera_example (ax_hrnet          ax_hrnet_steps.cc)
axera_example (ax_yolov3         ax_yolov3_steps.cc)
axera_example (ax_yolov3_tiny    ax_yolov3_tiny_steps.cc)
axera_example (ax_yolov4         ax_yolov4_steps.cc)
axera_example (ax_yolov4_tiny    ax_yolov4_tiny_steps.cc)
axera_example (ax_nanodet        ax_nanodet_steps.cc)
axera_example (ax_paddle_yolov3  ax_paddle_yolov3_steps.cc)

axera_example (ax_cv_test        ax_crop_resize_nv12.cc)