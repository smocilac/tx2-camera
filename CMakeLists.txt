cmake_minimum_required(VERSION 2.8)
set (CMAKE_CXX_STANDARD 11)
project( camera_test )

# setup CUDA
find_package(CUDA)
message("-- CUDA version: ${CUDA_VERSION}")

set(
	CUDA_NVCC_FLAGS
	${CUDA_NVCC_FLAGS}; 
    -O3 
	-gencode arch=compute_53,code=sm_53
	-gencode arch=compute_62,code=sm_62
)

if(CUDA_VERSION_MAJOR GREATER 9)
	message("-- CUDA ${CUDA_VERSION_MAJOR} detected, enabling SM_72")

	set(
		CUDA_NVCC_FLAGS
		${CUDA_NVCC_FLAGS}; 
		-gencode arch=compute_72,code=sm_72
	)

	find_package(OpenCV 3.0.0 COMPONENTS core calib3d REQUIRED)
endif()
include_directories(/usr/local/cuda/include)


add_executable( camera_test fps_test_gstreamer.cpp )

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include/include-jetson-inference)

include_directories(/usr/include/gstreamer-1.0 /usr/lib/aarch64-linux-gnu/gstreamer-1.0/include /usr/include/glib-2.0 /usr/include/libxml2 /usr/lib/aarch64-linux-gnu/glib-2.0/include/ )

target_link_libraries( camera_test ${OpenCV_LIBS} ${CMAKE_CURRENT_SOURCE_DIR}/lib/libjetson-utils.so ${CMAKE_CURRENT_SOURCE_DIR}/lib/libjetson-video.so /usr/local/cuda/lib64/libcudart.so)

